---
description: >-
  Traffic analysis is a fundamental step to gather further information on the
  attacked network and can also reveal sensitive data.
---

# Traffic Analysis

#### Capturing traffic

Before you can start analyzing wireless traffic, you have to set up your wireless adapter. Connect the adapter to your PC if it is an external adapter such as a USB or PCMCI card (otherwise you can use your integrates wireless card).

You should verify that your adapter has been correctly recognized by the OS. In Kali Linux you'd use `iwconfig` tool for this purpose.

Now disconnect your wireless card from any AP you might be associated with and try capturing from `wlan0` with Wireshark. AT this point, you will notice that you are not able to capture any packet.

This is due to how the wireless drivers stack internally cooperates with the operating systems and processes in order to achieve transparent Wi-fi communications.

Processes are not provided with the raw packets data. Instead the wireless stack delivers the packets as normal Ethernet frames.

The network stack of the OS is then charged with the duty to process those packets and deliver them to the proper processes as it normally does with Ethernet frames. This means that in normal conditions, Wireshark or any other traffic analysis tool, cannot receive raw 802.11 frames. Actually, if your wireless adapter is not associated to any Wi-Fi network, you will not be able to "sense" any data.

To demonstrate this concept, try to associate your wireless adapter to your test network and start a new Wireshark capture from the same wlan0 interface you previously used. You will suddenly start to see a lot of packets flowing. If you do not, try browsing a website or pinging your AP to generate some traffic.

Another limitation of this approach is that you will only see packets directed to your station. you will not be able to sniff traffic from other wireless clients.

This kind of behavior can be useful if you want to debug high level protocols.

Most of the attacks we will discuss in the next modules need lower access to the network stack so we need to set up our wireless interface to do so.

#### Monitor mode

In the Ethernet world: promiscuous mode. In the 802.11 jargon: monitor mode.

An interface configured to work in monitor mode will expose 802.11 frames to higher level protocols and will also accept frames directed to other STAs.

Not all operating systems or drivers natively support monitor mode. Monitor mode is an optional feature of NDIS 6 so it must still be implemented by the adapter driver.

Linux 802.11 stack has native support for monitor mode and many adapter drivers offer that support.

If you are using Kali Linux, then you are almost finished as most drivers included in that distribution are already patched to enable monitor mode on many adapters.

If your configuration is considerably different, you will probably need to find out specific information on how to configure your wireless pentesting attack machine.

The easiest way to put your interface into monitor mode on Linux is to use `airmon-ng` from the `aircrack-ng` suite:

```bash
> airmon-ng start <interface>
```

This command will crate a new virtual Wi-Fi interface, probably named "mon0", but this vary depending on the drivers you are using.

#### Channel hopping

There is still a limitation that is mostly physical. Even if you can now sniff all the packets being transmitted over the wireless medium, you are still restricted to one channel at a time.&#x20;

This is due to how wireless adapters internally de-modulate the received electromagnetic waves and can not be changed.

However, there's a trick you can use to achieve an approximation that is called "Channel Hopping". It refers to the technique of constantly switching the channel on which the wireless adapter operates.

Obviously, while locked to a specific channel, the wireless adapter still cannot receive frames sent on any others so this technique is mostly useful for recon purposes than to really capture data.

{% hint style="warning" %}
**Wireshark does not support Channel hopping**.&#x20;

Better to use `airodump-ng`.
{% endhint %}

If you have a supported card, you could also hop on more than one wireless band using the `--band` option and specifying a combination of a, b and g letters (a uses 5 GHz band, b and g use 2.4 GHz).

#### Wireshark filters

Keep in mind that Wi-Fi networks generate a lot of frames. For example, beacon frames are usually sent about every 100ms. As usually, you do not need to analyze them.&#x20;

Wireshark has a lot of filtering options and its filter expressions are quite powerful. There are filters available for almost every field included in a 802.11 frame. The first variable that we'll examine is the type/subtype aggregate, namely `wlan.fc.type_subtype`. To see it in action, type the following:

```
wlan.fc.type_subtype != 0x08
```

This will match all of the wlan (802.11) frames that do not have 0x08 as the type\_subtype value in the fc (Frame Control) field. Once you apply the filter, you can also save a new version of the capture file that does not include them. It'll be much easier to work on the new cleaned file.

As imagined, you can also filter by frame type. So if you want to get only Data Frames (type equals to 0x02), you should use the next filter:

```
wlan.fc.type == 0x02
```

Here a list of some other interesting filters:

```bash
wlan.bssid         # filters by AP MAC address
wlan_mgm.ssid      # shows Management Frames related to a SSID
wlan.addr          # all frames "from or "to" a specific MAC
wlan.da            # search frame with specific Destination Address
wlan.sa            # search frame with specific Source Address
wlan.fc.wep        # WEP encrypted frames
```

#### Traffic decryption

Wireshark can also help you decrypt captured wireless packets. You will have to provide the key as Wireshark _is not a cracking tool_.

To enable traffic decryption, click on `Edit > Preferences` and select `IEEE 802.11` from the left menu (under _Protocols_ section).

WPA uses a per-session key generated by two communicating stations, you must collect the 4-way handshake between those two stations for Wireshark to be able to correctly calculate the key and decrypt the conversation data. To ensure you captured the handshake, apply a filter using "eapol".

`aircrack-ng` suite also offers a decryption tool. It is called `airdecap-ng`

In the same way as Wireshark, it can decrypt WEP, WPA or WPA encrypted packages.

```bash
> airdecap-ng -w <wep_key_in_hex> <.cap>
```

You do not need to specify the output file. `airedecap-ng` will automatically append the `-dec` suffix to the input filename and create a new file. Upon execution, you will be shown a report of decrypted packets.

The syntax to decrypt WPA packets is somewhat different but not complex:

```bash
> airdecap-ng -p <wpa_passphrase> -e <SSID> <.cap>
```

The main difference is that you also need to specify the network SSID. You should use plain ASCII characters to specify the passphrase.

If you noticed, `airdecap-ng` removed the 802.11 header from all of the decrypted frames and also filtered out all management and control frames. This can be annoying as sometimes you may want to debug or analyze the traffic with all the information. To disable this behavior and keep the headers too, just use the `-l` flag when running the command.&#x20;

{% embed url="https://www.aircrack-ng.org/doku.php?id=airodump-ng" %}

{% embed url="https://www.aircrack-ng.org/doku.php?id=aireplay-ng" %}

### :arrow\_forward: Protocol and Wireshark Filters

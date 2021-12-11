---
description: >-
  Since discovering wireless network mostly requires just traffic sniffing,
  mature tools are available for all of the major platforms.
---

# Discover Wi-Fi Networks

## Tools

&#x20;Some of the best tools are:

* Kismet, `airodump-ng` (Linux)
* InSSIDer Office (Windows)
* KisMAC (Mac OS X)

### **InSSIDer Office**

Intuitive tool developed by MetaGeek. It is a commercial Windows-only tool but it's available in a free trial mode. There's a _Networks panel_, we find information about reachable networks in the area.

The InSSIDer Networks panel table provides us with:

* SSID (Service Set Identifier)
* Signal strength
* Channel(s)
* Encryption level (WEP/WPA/WPA2)
* AP MAC address
* Wi-Fi protocol

As a side note, InSSIDer also provides the Channels pane which gives us a great way to optimize our Wi-Fi performances by choosing the less crowded channels.

### **Kismet**

While less intuitive, it offers more useful features for the wireless pentester. Kismet is based on a client/server architecture. The server provides data while the client application uses them to display information gathered from one or more servers. This architecture is further extensible with another subject: drones.

These drones are simple wireless devices that only scan the air and feed captured frames to a specified server.

The first thing to do when using Kismet is putting your wireless adapter into monitor mode.

The simples way to start sniffing with Kismet and your monitoring interface is through:

```bash
> kismet -c <mon_interface>
```

In the upper panel, you will find a list of available wireless networks. The central panel will display information about clients connected to the selected network. The plot displays packets and data rates. The bottom panel shows an informative log that can be useful for debugging purposes. Sidebar shows useful statistics as well.

If you want to dive into a particular network, you can click on its row in the Networks list and a details window will open up.

A more interesting feature allows you to see the clients that communicate with a particular AP/Network. Information about currently or previously associated clients is fundamental for some of the most powerful attacks.

Kismet uses color coding to help you to identify features of listed networks. Chosen color depends on the value of the C (Encryption type) column:

* Green. N (None)
* Red. W (WEP)
* Yellow. O (Other), typically WPA or WPA2

### **airodump-ng**

Comprehensive wireless sniffing tool included in the famous `aircrack-ng` suite. It comes with a very essential text-based user interface. Despite this, it has a lot of useful feautres and it perfectly integrates with all of the other tools of the `aircrack-ng` suite which makes it the perfect pentester companion.

It can:

* Perform automatic channel switching
* Filter captured traffic by BSSID or cypher suite
* Determine the list of clients associated to a network and their MAC addresses
* Provide information on signal level, network traffic, security settings

Upper section: shows information about current channel, elapsed time and date-time.

Upper left: will notify you of particular events as you will see in later modules when we explore the attacks.&#x20;

Network lists: gives you information about network found in your area.&#x20;

The tabular view is very similar to what we encountered when trying InSSIDer or Kismet.

\#Data and #/s columns: will be of much information value when we learn how to crack WEP keys.

RXQ column: Receive Quality, measured as the percentage of successfully received management and data frames over the last 10 seconds. A value of 100 represents a perfect signal. It is calculated by looking at sequence numbers of received frames and looking for gaps between them.

Bottom section shows information about network clients.

The two numbers under Rate represents the "AP to client" and "Client to AP" last detected data rates respectively, whereas "Lost" indicates the number of lost packets coming from the particular clients. The number is obtained in a similar way to RXQ, by looking at sequence numbers of received management and data frames.

```bash
> airodump-ng -w filename -c 1,6,11 -t wep -b <BSSID> mon0
```

## Hidden SSID

Almost all APs have an option to cloak the SSID value they broadcast in all the beacon frames. When this option is set, the AP will simply replace the original SSID value with a null value. While this can be a simple measure to stop a newbie from "seeing" the network, it does not deliver strong protection.&#x20;

Passive de-cloaking attacks work by sniffing frames transmitted over the network. Many Wi-Fi frames transmitted by both the AP and the STAs will contain an SSID field in plain-text.

Some examples are:

* Probe request
* Probe responses
* Association requests
* Re-association requests

{% hint style="info" %}
**Wireshark moment**

If you are sitting in an area crowded by Wi-Fi networks, you will probably receive a lot of frames in just a few seconds. To focus only on beacon frames you can use this Wireshark filter:

```
wlan[0] == 0x80
```
{% endhint %}

It's simply a matter of time before a station sends out a frame containing the SSID information.

While a passive attack should work most of the time, there is a slight chance that if the network traffic is very low, an active attack may be necessary.

**Active attacks**

Active attacks involve sending DEAUTHENTICATE messages for an active station to the station's AP. This will force the STA to rejoin the network in order to communicate with the original AP. Since the network is hidden to the STA too, it will have to cycle through all the channels sending Probe Request frames, allowing us to intercept the Probe responses containing the target SSID field.

&#x20;Lab:

* Configure your AP to hide the SSID value
* Associate a victim client to the network
* Configure your monitoring interface and start sniffing through it with Kismet

Kismet will show a placeholder for the hidden networks.

The next step of the attack requires locking our wireless adapter to the target network's channel. You can achieve this through Kismet by clicking on the Kismet menu and selecting _Configure Channel_.&#x20;

Click _Lock_ in the next window and write the proper channel into the input field.

Now you need to get the list of STAs associated to the target hidden network as you will next try to deauthenticate one; write down your victim client MAC address. To perform de-authentication:

```bash
> aireplay-ng -0 <num> -c <client_mac> -a <BSSID> <interface>
# -0 num : stands for deauth attack repeated for num times or use 0 (zero) for infinite loop
```

If Wireshark was running during the attack, you can take a look at the Probe Response frames sent by the target client after deauthentication. Use this expression to filter out uninteresting frames:

```bash
wlan.fc.type_subtype == 0x05
```

So cloaking the SSID value of a wireless network can not stop an attacker from discovering it.

In a small office environment, with low client mobility and more permanent connections, a simple de-authentication attack can reveal the SSID in a couple of seconds while in a bigger environment a passive scanning will suffice as Probe Requests/Responses will be much more frequent.

{% embed url="https://www.metageek.com/" %}

{% embed url="https://www.kismetwireless.net/" %}

{% embed url="https://www.wireshark.org/" %}

```bash
iwconfig
kismet -c mon0
airodump-ng mon0
airodump-ng -c 11 -a mon0
```

# Environment Setup

> **Consideration #1**
>
> * The Linux 802.11 subsystem is fragmented. Available tools and commands depend on the driver you are using.

The most reliable way to determine if you are using mac80211 drivers is by running following command from a terminal window:

```bash
> lsmod | grep mac80211
mac80211    1378841    1    rtl8187
```

Modules listed on the right side are mac80211 drivers. In this particular example `rtl8187` is the driver of our Wi-Fi Dongle, based on a Realtek chip.

> **Consideration #2**
>
> another difference to note between the various Linux wireless drivers is the naming scheme for the network interface; older drivers use different prefixes like `eth`, `wifi` or `ath`.

The mac80211 framework set a standard prefix, `wlan`.&#x20;

{% embed url="https://www.aircrack-ng.org/doku.php?id=install_drivers" %}

### Adapter configuration

#### 1st step

As a first step, you must verify that your wireless card has been correctly detected:

```bash
> iwconfig
```

If you want to get even more information, you can use another command available for `mac802111` drivers: `iw` tool.

```bash
> iw list
```

You will get a list of all of your adapter's supported modes and capabilities.

It's worth noting that the `iwconfig` utility can be used to set various parameters for your wireless interface. For example this command sets the card's Wi-fi channel to 11:

```bash
> iwconfig wlan0 channel 11
> iw dev wlan0 set channel 11
```

> Maximum transmission power level is also controlled by country laws. In the 802.11 specifications, every country represents a so-called regulatory domain, often shortened to _regdomain_. Each _regdomain_ is identified by the correspondent ISO country code.
>
> **Please not that using a high transmission power may be illegal in your country!**

By default, many wireless adpaters are configured to work with regdomain set to 0. When running with this configuration, most adpaters will not deliver their maximum performance. However you can change internal regdomain setting with command line utilities.&#x20;

{% hint style="info" %}
**Bolivia**

A trick that is often used to increase maximum transmit power of a wireless adapter consists in setting the country code to match Bolivia's.&#x20;

```bash
> iw reg set BO
> iw dev wlan0 set txpower fixed 30dbm
```

This commands se the maximum transmission power to 30dBm which as we know, corresponds to 1000mW.&#x20;

You can confirm everything worked by launching `iwconfig`.
{% endhint %}

Usually the `wlan0` interface can only be used to connect to _intrastructure_ or _ad-hoc_ networks, although more can be done.

#### 2nd step

The next step is to setup a **monitor interface**_,_ a virtual interface that can be used to sniff traffic and perform low level network operations through your adapter.

```bash
> airmon-ng start wlan0
> iwconfig mon0
> airmon-ng stop mon0 # stop / dete the monitor interface!
```

`airmon-ng` can also help you to detect and resolve blocked device conditions.

```bash
> airmon-ng check kill
```

#### 3rd step

The last thing we should do is to check if everything is working fine with `aireplay-ng` tool, in this way we can test whether packet injection is working.&#x20;

`-9` is 'test-mode'. Upon execution, `aireplay-ng` will send out broadcast probe requests. If any AP responds, `arieplay-ng` will print a message informing you that your card can successfully inject.

> Remember to start your monitor interface first and set the card to the desired channel.

Every AP in the respondents list is the directly probed 30 times and a percentage of responses received is given for each one.&#x20;

This percentage is an excellent indication of the link quality for the specified AP.

{% embed url="https://www.aircrack-ng.org/doku.php?id=aireplay-ng" %}

{% embed url="https://www.aircrack-ng.org/doku.php?id=airmon-ng" %}


---
description: >-
  Learn the security mechanisms implemented in Wi-Fi architectures as well as
  their weaknesses and how to exploit them.
---

# WIP - Wi-Fi Security

{% hint style="danger" %}
**This document is still in progress...**&#x20;
{% endhint %}

## Prerequisites

While normal pentesting operations can be performed with standard hardware, when it ocmes to Wi-Fi pentesting, you must choose your equipment carefully.

### Hardware

Almost all new notebooks come equipped with a WNIC (Wireless Network Interface Controller) that enables Wi-Fi communications.

These controllers use the common Mini PCI (or more recently, Mini PCI-Express) bus and are totally integrated in the notebook chassis.

This does not make them the perfect solution for penetration testers as they can not provide the necessary signal power and receiving sensitivity - crucial parameters for a successful wireless attack.

A better option is to buy an external USB Wi-Fi dongle.

This is especially true if you plan to perform your attacks from a virtualized environment as an integrated wireless card will simply work as an Ethernet card in your guest OS.

When looking for a new Wi-Fi adapter, you have to pay attention to these three factor:

* Signal power
* Receiver sensitivity
* Linux Driver support

The last one is probably the most important.

While there are tons of compatible wireless adapters, some _standards_ emerged inside the Linux Wi-Fi pentesting community.

The **Alpha AWUS036H** has become quite a popular device and has been tested and confrmed to work very well with the `aircrack-ng suite`.

It comes with the **Realtek RTL8187L** chipset, which is widely supported.

It also has a huge transmit (TX) power of 1000mW.

The external connector RP-SMA type enables substitution of the standard antenna so you can choose the best fit for your scenario.

It's also **802.11g wireless standard** compatible.

### Antennas

Most Wi-Fi adapters are sold with integrated antennas. These kind of adapters are good for home or small office use but they are not the best choice when it comes to penetration testing.

If you are going to do some serious tests, you will need a more flexible adapter that gives you the ability to change the attached antenna to fit your scenario.

Connection range of a Wi-Fi adapter mostly depends on the antenna **power gain**. This characteristic is often specified using decibels (dB) as a relative numeric value compared to a reference antenna. Most of the time you will find gain specified as _dBi_, on rare occasions, it can be noted using _dBd_.&#x20;

You can convert with this formula

$$dBd = 2.14dBi$$&#x20;

There are two main types of antennas:

* Omnidirectional
* Directional

#### Omnidirectional antennas

They are designed to pick up signals coming from multiple directions - typically in a 360 degrees area.&#x20;

Because of that, they are commonly found on all wireless-enabled consumer devices, especially on routers and Access Points as they must support connection over a wider area.&#x20;

Their gain ranges from 2 dBi to 9 dBi.\
\
**Rubber ducky** antenna is well known and shipped with Alpha Wi-Fi adapters.

#### Directional antennas

Because omnidirectional antennas power spreads in each direction, their power gain is inherently lower than directional antennas which focus their energy across a narrow angle. This characteristic makes them ideal for long-range connections but as the antenna pattern gets narrower, you will limit your coverage area.

Directional antennas can provide gains over 12 dBi.

As directional antennas can be useful, even for home users, many homemade designs have emerged during the past few years and were popularized over the Internet.

**Cantennas**

Made of cans. Prigles' packages are used for the purpose.&#x20;

This type of antenna can provide gains of **8 dBi and up**.

**WokFi**

Parabolic antenna made from Asian works (or other metallic dishes) that can boost your power gain to **12 dBi and higher** as they have a very narrow beam-width.

As with "cantennas", you can find a lot of tutorials on the Internet that will help you build one from cheap materials.

**Yagi-Uda**

Directional antenna that can be used for very long-distance connections.&#x20;

Commercial products reach gains over **20 dBi**.&#x20;

### Signal Strength

Most of tools use dBm nottation to express signal levels.

dBm are relative measures too and their reference value is **1mW**.&#x20;

To calculate the dBm value from an absolute power value, you can use this formula

$$dBm = 10*log10(power/1mW)$$&#x20;

| dBM | mW   |
| --- | ---- |
| 0   | 1    |
| 10  | 10   |
| 15  | 31   |
| 20  | 100  |
| 27  | 500  |
| 30  | 1000 |

When using negative notation, the expressed power is a fraction of the reference **1mW** power.&#x20;

So -30dBm corresponds to 1µW or one thousandth of a milliwatt.

Given that, lower absolute value dBm values guarantee stronger signals.

Ex: -30dBm signal is much stronger than -70dBm

> As a quick reference, you should remember that a 10dBm decrease is a hundredth reduction of the power.

{% embed url="https://www.wikihow.com/Make-a-Cantenna" %}

{% embed url="https://www.aircrack-ng.org/doku.php?id=compatibility_drivers#determine_the_chipset" %}

{% embed url="https://www.instructables.com/Wifi-Signal-Strainer-WokFi/" %}



















##

##

##

##

##

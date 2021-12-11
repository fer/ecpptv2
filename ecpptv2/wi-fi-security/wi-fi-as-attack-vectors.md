# Wi-Fi as Attack Vectors

## Wi-Fi as Attack Vectors

### Wi-Fi as Attack Vectors

As a penetration tester, you should know that Wi-Fi networks are not only interesting as an attack point but can turn out to be useful attack vector too.

#### Rogue AP

In some situations it's simpler to get a wireless client to connect to our attacking system instead of recovering the network WEP or WPA key.

> Caffe-Latte attack consisted in tricking a wireless client into connecting to a fake AP with the purpose of collecting a sufficient long keystream.&#x20;
>
> With that keystream, we are able to forge our packets and flood the client itself with ARP requests, collecting a lot of IVs.

Imagine being able to to set up a "Free Wi-Fi" Access Point to control all of the communications through it.  As you are totally in control of the packet flow, you could launch all of your favorite attacks you have learned to apply in the wired word such as MitM, ARP poisoning, traffic sniffing or even browser vulnerabilities.

{% hint style="info" %}
**airbase-ng**

* Implementations of the Caffe-Latte and Hirte attacks
* Act as _ad-hoc_ or _infrastructure_ AP
* Encrypt and decrypt traffic
* Can capture WPA/WPA2 handshakes
* Packets manipulation and external commands
* Filtering by BSSID or client MAC

Given that `aribase-ng` has a pretty list of options, you are encouraged to take a look at its own man page.
{% endhint %}

Imagine this simple situation:

* Sam is an employee at "ACME Corporation"
* There's a Wi-Fi network called "ACMEWiFi" at his workplace
* This Wi-Fi network is using WEP encryption and Shared Key authentication
* Sam normally takes a cup of tea at his preferred bar.
* Unfortunately, he also has some backlog so he boots up his notebook to continue working on ACME's matters while sitting at the bar table.
* Sam's notebook will start to probe the neighborhood area in order to discover available networks.

> **Attack**
>
> If we could set up a fake AP that spoofs the "ACMEWifi" SSID name and advertise itself as a WEP network, we could trick Sam's notebook into starting an authentication/association message exchange with our machine!
>
> The objective of the attack is getting Sam's notebook to connect to the fake AP.
>
> Since Shared Key Authentication is enabled, you will be able to recover a good amount of keystream (generally 140 bytes) which is more than what you need to forge your own ARP requests with packetforge-ng.

> **Recover PRGA with a rogue AP**
>
> Here's our setup:
>
> * A victim client unassociated from any AP
> * Our attacking machine ("fake" AP)
>
> ```bash
> > airmon-ng start <interface>
> > airodump-ng  -c <channel> -w <outfile> <interface>
> # You will need to lock the wireless card to a specific channel.
>
> > airbase-ng -c <channel> -e <SSID> -s -W 1 <interface>
> # -F <file> let airbase-ng save all of the captured information to a file
> # -s force the client to authenticate using the SKA method
> # -W 1 instructs airbase-ng to set the WEP bit in the beacons 
> # as some clients can get confused otherwise.
>
> # At this point you should have gotten a keystream after the victim client
> # connected to the fake AP. airodump-ng should see a SKA handshake was captured
> # and saved in a .xor file for our convenience.
> ```

> **Initiate a WPA/WPA handshake**
>
> We can also use the same technique to obtain a WPA/WPA2 handshake. Configure your client network connection as WPA2 and give it a password. Then launch `airbase-ng` with these options:
>
> ```bash
> > airbase-ng -c <channel> -e <SSID> -W 1 -Z 4 <interface>
> # -Z is used to specify WPA2 optiosn while 4 stands for CCMP encryption scheme.
> ```
>
> The victim client can be tricked into connecting to the spoofed AP and we are able to collect the WPA2 handshake, whenever the client was able to associate.
>
> To prove the attack really worked, we try to crack the captured handshake by passing `aircrack-ng` the configured password through a shell "pipe":
>
> ```bash
> echo largecream | aircrack-ng file.cap -e <SSID> -q -w -
> ```
>
> This is just a convenient way to test one password. Notice we used a dash after the `-w` option to instruct `aircrack-ng` to use the standard input as word list source.
>
> The fun part of this attack is that **we were able to get a real handshake without actually knowing the network PSK.** How is this possible?
>
> In the first message of the 4-way handshake, the AP sends an Anonce to the authenticating client which in turn sends its SNonce plus the MIC. Now the AP has all of the needed information to actually try to crack the PSK, the subsequent steps in the handshake are not eve needed.

> **Man in the Middle Attack**
>
> Setup:
>
> * A victim client unassociated from any AP
> * The attacker machine should be connected to the internet through a wired interface
>
> You will need Internet access on the attack point as we will be running MitM attacks that require packet forwarding towards the real destination in order to sniff the resulting traffic.
>
> Steps:
>
> 1. Set up a fake AP
> 2. Start a DHCP server to provide the network configuration to connecting clients
> 3. Forward all the traffic toward the Internet but ...
> 4. .. act as MitM eavesdropping all the communications
>
> ```bash
> # First we put the wireless interface into monitor mode as we always do:
> > airmon-ng
> # Start airbase-ng and setup AP with a catch SSID name
> > airbase-ng -c <channel> -e "Free Internet" <interface>
>
> # Create a network bridge interface with the following commands:
> > sudo apt-get install bridge-utils
> > brctl addbr br0
> > brctl addif br0 eth0
> > brctl addif br0 at0
> # br0: is the name of the bridge interface we are creating
> # eth0: wired interface
> # at0: virtual interface created by airbase-ng
>
> # Now that we have created a bridge between the airbase-ng virtual interface
> # and our wired interface, we need to assign an IP address to it:
> > ipconfig br0 <ip_address> up
>
> # enable IP packet forwarding:
> > echo 1 > /proc/sys/net/ipv4/ip_forward
> ```
>
> All of the Internet directed traffic from the victim client is already being forwarded through the attack machine. To confirm everything is working fine, you can try to browse a website from your victim client. You should be able to load it correctly.&#x20;
>
> Now that you are controlling all of the traffic flowing from and towards the client, you can try to steal confidential data such as website credentials as in the classic MitM attack.&#x20;
>
> Fire up your favorite sniffing tool and start listening on your virtual wireless interface
>
> ```bash
> > tcpdump -nvi <interface> tcp port 80 -A
> ```
>
> From the victim client, we now ingenuously browse to a sample website. In such website a form where the user is required to insert login and password in order to access a secured area
>
> Shortly after the POST HTTP request, `tcpdump` output will dump the user credentials on your screen.
>
> More possibilities:
>
> * Redirect DNS Requests
> * Change web page content
> * Harvest user information
> * Inject browser-specific payloads to exploit browser vulnerabilities

> **Rogue AP: an alternative definition**
>
> The following is an alternative definition of rogue AP: it's an unmanaged and unauthorized wireless AP attached to an enterprise wireless network.&#x20;
>
> A rogue AP could have been installed by a legitimate user unaware of the security implications or on purpose for an insider attack. An outsider can also install the AP inside the enterprise premises if insufficient physical security measures are used.
>
> Rogue APs represent a major security threat as they create a wireless backdoor on the internal wired network that bypasses all of the perimeter defenses like firewalls and IDS.
>
> In many occasions they are installed by security-illiterate employees and configured with poor encryption scheme as WEP or worst, left completely open.
>
> A typical example of Rogue AP is the one set up by an employee willing to share the company's Internet connection with mobile devices or the same employee could bring an AP to connect from its laptop and bypass internal security policies just to be able to surf social network sites.
>
> In all cases, wireless signal could reach past the company premises.
>
> A pentester with good wireless equipment will then be able to try a series of attacks.
>
> In the simplest case, the attacker can passively scan the wireless medium to collect information about network configuration, hostnames and IPs.
>
> Sensitive information could also be disclosed such as usernames, passwords or emails (especially if the wireless network uses no encryption).

> **Rogue AP: Evil Twin Attack**
>
> We setup a rogue access point to mimic an existing known access point, we can use an Evil Twin attack combined with a bit of social engineering to obtain a WPA2 networks' Pre-shared key without the need to conduct a cryptographic attack against the WPA2 protocol itself.
>
> A typical flow for an Evil Twin attack is as follows:
>
> 1. Replicate a known Access Point ESSID via creation of an access point with hostapd.
> 2. De-authenticate a station that is associated to the "real" AP.
> 3. Station reconnects to "Evil Twin" AP.
> 4. The user, upon launching a browser is presented with a web page over HTTP requesting SSID for an "Important Firmware Upgrade"
> 5. We receive the SSID in plain-text via the HTTP page.
>
> As "all-in-one" toolkit we can use to conduct this type of attack is known as **Mana**.
>
> Mana allows us to quickly spin up a rogue access point, configure the necessary DHCP settings and with some modifications to the default configuration, we can host our own web page to be served to a connected station.
>
> **Important!** The attacker AP should be in close proximity to a station already connected to the legitimate AP. This way, upon de-authentication of the client, the client should auto-reconnect to the AP with the stronger signal (the attacker-controlled AP).

> **Attacks against WPA2-Enterprise**
>
> WPA2-Enterprise introduced several improvements to the WPA2-PSK model in regards to security, primarily with the support of 802.1x authentication, and therefore, requires its own set of tools and changes to the way we traditionally perform attacks against wireless networks.
>
> In the traditional WPA2-PSK model, we typically have a client (supplicant) that connects to an access point (authenticator), the usual "two-party" scenario.
>
> With WPA2-Enterprise, we introduce a third-party "Authentication Server", which is usually a system that supports the RADIUS and Extensible Authentication (EAP) protocols.
>
> * **Initialization:** On detection of a new supplicant, the port on the switch (authenticator) is enabled and set to the "unauthorized" state. In this state, only 802.1X traffic is allowed; other traffic such as the Internet Protocol (and with that TCP and UDP) is dropped.
> * **Initiation:** to initiate authentication will periodically transmit EAP-Request Identity frames to a special Layer 2 address (01:80:C2:00:00:03) on the local network segment. The supplicant listens on this address and on receipt of the EAP-Request Identity frame it responds with an EAP-Response Identity frame containing an identifier for the supplicant such as a User ID. The authenticator then encapsulates this Identity response in RADIUS Access-Request packet and forwards it on to the authentication server. The supplicant may also initiate or restart authentication by sending an EAPOL-Start frame to the authenticator, which will then reply with an EAP-Request Identity frame.
> * **Negotiation:** technically EAP negotiation, the authentication server sends a reply (encapsulated in a RADIUS Access-Challenge packet) to the authenticator, containing an EAP Request specifying the EAP Method (the type of EAP based authentication it wishes the supplicant to perform). The authenticator encapsulates the EAP Request in an EAPOL frame and transmits it to the  supplicant. At this point, the supplicant can start using the requested EAP method, or do a NAK (negative acknowledgement) and respond with the EAP methods it is willing to perform.
> * **Authentication:** if the authentication server and supplicant agree on an EAP Method, EAP Requests and Responses are sent between the supplicant and the authentication server (translated by the authenticator) until the authentication server responds with either an EAP-Success message (encapsulated in a RADIUS Access-Accept packet), or an EAP-Failure message (encapsulated in a RADIUS Access-Reject packet). If authentication is successful, the authenticator sets the port to the "authorized" state and normal traffic is allowed; if it's unsuccessful, the port remains in the "unauthorized" state. When the supplicant logs off, it sends an EAPOL-logoff message to the authenticator, the authenticator then sets the port to the "unauthorized" state, once again blocking all non-EAP traffic.

> **Eaphammer: Attacking WPA2-Enterprise neworks**
>
> Aside from being able to automate Evil Twin attacks similar to the previously mentioned Mana toolkit, eaphammer allows us to steal RADIUS credentials, conduct hostile portal attacks to steal Active Directory credentials (through Responder-type attacks), and includes a host of other features we'll find useful during Wireless Penetration testing engagements:
>
> * Built-in Responder Integration
> * Support for Open networks and WPA-EAP/WPA2-EAP
> * No manual configuration necessary for most attacks
> * No manual configuration necessary for installation and setup process
> * Leverages latest version of hostapd (2.6)
> * Support for evil twin and karma attacks
> * Generate timed Powershell payloads for indirect wireless pivots
> * Integrated HTTP server for Hostile Portal attacks
> * Support for SSID cloaking

#### Wardriving

It's the act of searching for Wi-Fi networks by a person on a moving vehicle (like a car) using a portable computer, a smartphone or any other Wi-Fi enabled device.

The word originated from WarDialing, a practice that consisted of using a modem to automatically scan a list of telephone numbers searching for equipment as fax machines, modems or other systems.

The main objective of wardriving is creating a map of Wi-Fi access points in a specific area.

The map can then be used to observe AP distribution and characteristics like SSID names or encryption type (if used).

To start wardriving, all you need is a good GPS receiver, a Wi-Fi enabled device and, obviously a vehicle.

These days, most smartphones available on the market have decent Wi-Fi capabilities and an integrated GPS sensor so they can actually be used as wardriving devices, however you can not expect the same level of signal range than a good wireless adapter with a proper antenna could get.

WiGLE.net is a website that collects all of the user uploaded information and build constantly updated maps of Wi-Fi Access Points around the world.

After the drive, we use an app functionality which allows one to export the database of found APs in KML.

{% embed url="https://github.com/sensepost/mana" %}

{% embed url="https://en.wikipedia.org/wiki/IEEE_802.1X" %}

{% embed url="https://en.wikipedia.org/wiki/Evil_twin_(wireless_networks)" %}

{% embed url="https://wigle.net/" %}

{% embed url="https://en.wikipedia.org/wiki/RADIUS" %}

{% embed url="https://github.com/s0lst1c3/eaphammer" %}

### :arrow\_forward: Rogue Access Point

### :arrow\_forward: Evil Twin Attack with Mana Toolkit Pt. 1

### :arrow\_forward: Evil Twin Attack with Mana Toolkit Pt. 2

# Wireless Standards and Networks

### IEEE 802.11 Standards

IEEE (Institute of Electrical and Electronic Engineers) is a worldwide association counting over 435000 members dedicated to advancing technological innovation.

IEEE 802.11 is the IEEE Working Group that develops and enhances standards related to Wi-Fi technologies.

| Protocol        | Release Date | Radio Band (GHz) |
| --------------- | ------------ | ---------------- |
| 802.11 (legacy) | 1997         | 2.4              |
| 802.11a         | 1999         | 5                |
| 802.11b         | 1999         | 2.4              |
| 802.11g         | 2003         | 2.4              |
| 802.11n         | 2009         | 2.4 / 5          |

IEEE 802.11g is the most widely deployed version of the protocol. In the last few years, 'n' version APs have become quite frequent but most of them still use the 2.4GHz band for backward compatibility with older adapters.

The 2.4GHz band (2.4GHz - 2.495Ghz) is divided into _14 overlapping channels_ with a 22MHz bandwidth around the central frequency. Each channel is simply referred to by its number.

Channel availability is not the same in every part of the world. In fact, most countries have special requirements. For example in USA you can only use channels from 1 to 11 while in Japan the whole spectrum is available (1 to 14).

The 802.11n protocol introduced the 5GHz band as the 2.4GHz was getting more and more crowded with so many devices interfering with Wi-Fi communications (bluetooth, cordless phones, microwave ovens). 5GHz band is far less crowded and guarantees a higher number of non-overlapping channels.

802.111n also defines specifications for 40MHz channel bandwidth that double the maximum theoretical throughput.

### Types of Wireless Networks

There are two main types of wireless network architectures described by Wi-Fi standards:

1. Infrastructure Network
2. Ad-hoc Network

#### Infrastructure Network

A Basic Service Set (BBS) contains an Access Point (AP) and a set of wireless client stations (STAs).

Every BSS has a unique formal identifier called BSSID (the MAC address of the AP).

An informal name is also assigned to a BSS. It is called SSID (Service Set Identifier) and it's easier to remember.

It's composed by a 32-byte (maximum) character string and is the one you usually see when connecting to a wireless network.

Single BSS's can be combined to form groups of Access Points connected together. This configuration takes the name of Extended Service Set (ESS). In such a configuration, multiple BSS's exist with a common SSID (now called ESSID) but unique BSSID.

Access Points can be linked together using a backbone Distribution System (DS) which is usually a wired Ethernet network; this enables communications between STAs associated in different BSSs and other segments of the network.

This configuration is used by corporations and it enables larger coverage areas.

#### Ad-hoc network

This type of network does not need an existing infrastructure. All of the STAs directly communicate to each other, as there is not a central base. A set of stations connected like this is called IBSS (Independent Basic Service Set).

A simple schema of an Ad-hoc network can be a simple IBSS with 3 stations communicating each other.

### Wireless frames

Now we'll take a look at the fundamental datagram units of the Wi-Fi protocols.

In the context of 802.11 specifications, datagrams are called _frames_.

Each frame consists of

* a header
* an optional payload (data)
* a Frame Check Sequence (FCS)

This is the 802.11MPDU (MAC Protocol Data Unit) format. Each field is annotated with its byte length:

![Second row decomposes the Frame Control Field!](<../../.gitbook/assets/image (81).png>)

We will not dive into the details of every single field as this is not the purpose of this course. Instead, we will focus or attention on the most interesting ones.

The _Frame Control_ field contains control information defining the type of 802.11 MAC frame and how to process the frame itself.

The frame function is defined by the _Type_ and _Subtype_ fields.&#x20;

Note that there are multiple subtype fields for each frame type. Each subtype determines the specific function to perform for its associates frame type.

Currently the standard describes three types of frames:

* Management Frames
* Control Frames
* Data Frames

The frames _To DS_ and _From DS_ indicate whether the frame is going to or existing from the DS (Distributed System).

The WEP field (also called Privacy Bit) is a Boolean flag and indicates whether or not the WEP algorithm has been used to encrypt the packet. The Privacy Bit can be set only for data frames and management frames for authentication purposes.

Addresses can be a combination of the following:

* BSSID: identifies an AP
* Destination Address (AD): final destination to receive the frame
* Source Address (SA): original source that created the frame
* Receiver Address (RA): receiving STA
* Transmitter Address (TA): transmitting STA

The presence of both a SA and a TA might be confusing at first but if you think about how Wi-Fi works, you will understand that all of these fields are necessary as frames are relayed.

For example, in an Infrastructure network, all of the frames between two stations still to pass from the base AP. In that case, packet coming from the stations will have the 1st tation's MAC as Source Address but AP's MAC as Transmitter Address.

Frame Body is an optional field (from 0 to 2312 bytes) that contains the payload of the transmission. It is used in either data and management frames. When the WEB bit is set, the body is extended by 8 bit (so its maximum length becomes 2320 bytes).

And finally the Frame Check Sequence (a simple Cyclic Redundancy Check - CRC - of the entire frame) is used for transmission errors and detection.

This table lists Management frames subtypes:

| Type (Management) | Subtype   | Description            |
| ----------------- | --------- | ---------------------- |
| 00                | 0000=0x00 | Association Request    |
| 00                | 0001=0x01 | Association Response   |
| 00                | 0010=0x02 | Reassociation Request  |
| 00                | 0011=0x03 | Reassociation Response |
| 00                | 0100=0x04 | Probe Request          |
| 00                | 0101=0x05 | Probe Response         |
| 00                | 1000=0x08 | Beacon                 |
| 00                | 1010=0x0A | Disassociation         |
| 00                | 1011=0x0B | Authentication         |
| 00                | 1100=0x0C | Deauthentication       |

Beacon frames are periodically transmitted by an AP. Their purpose is to advertise the availability of a wireless network. They contain information about network parameters and AP capabilities such as supported throughput rates.

Beacons also contain the SSID but that value can be stripped from the frame for security reasons (Hidden SSID configuration).

Probe Requests are sent by a wireless client in order to determine the network availability status. It contains the SSID name of the network and is sent over all the wireless channels. A special "null" (0x00) SSID can be used if the client does not want to search for a specific network.

Client can also query the AP for specific information in the request.

Probe Responses are sent by an AP upon the reception of a Probe Request.

They are very similar to beacons but they can also contain addition information (as specified by the communicating client inside the corresponding Probe Request frame).

Authentication frames are used to perform the authentication process.

Unlike association or probing, all authentication frames share the same subtype.

After the authentication, a station needs to associate to an AP. This is the purpose of the Association Request frames. These frames carry information about the STA capabilities (e.g. supported data rates) and the SSID of the network to which it wishes to associate.

After receiving the association request, the AP considers associating with the STA, and (if positive) establishes an Association ID (AID) for the newly associated STA reserving necessary memory resources.

The Association Response frame contains an acceptance or rejection notice to the requesting STA. If the AP accepts the STA, the frame includes information regarding the association such as Association ID and supported data rates.

The wireless STA can now start to communicate with other peers in the network through the AP.

A station sends a Disassociation Frame to another station if it wishes to terminate the association but the same frame can be used by either party of the communication.

Disassociation is often used when the station is roaming from a BSS to another in order to keep the authentication status.

The frame also contains a reason code field.

Deauthentication frames are used when all of the communication is terminated. For example, a STA that is shut down gracefully can send a deauthentication frame to alert the access point that it is powering off. The access point can then free memory allocations and remove corresponding records in its internal structures.

As for disassociation, the AP can also use these frames.

Reassociation Requests and Responses are used by a STA and AP respectively and they enable roaming stations the capability to move from one AP to another without losing the authenticated status.&#x20;

### Security features

We'll explore two main aspects of Wi-Fi security:

* Traffic encryption
* Station authentication

#### Wired Equivalent Privacy

The first attempt to secure the communications on Wi-Fi netowrks was WEP which was introduced in the 802.11 standard - ratified on September 1999. The acronym stands for Wired Equivalent Privacy.

Despite this, WEP has been proven to suffer from a number of flaws that made it completely ineffective. Due to these findings it has been deprecates in subsequent versions of the standard.

WEP uses the RC4 algorithm for encryption with a 40 (WEP-40) or 104 bits (WEP-104) long key.

RC4 is a stream cipher that uses a pseudo-random generation algorithm (also called PRGA) coupled with an internal state and a key to generate a byte keystream.

This keystream is then XORed to the plaintext to obtain the final encrypted cyphertext.

In WEP implementation, the RC4 internal state is reset on every frame.

Normally, this will completely break security as every plaintext would be encrypted with the exact same keystream; despite its name, the PRGA algorithm is completely deterministic and would produce the same results over and over if the same key is applied.

To alleviate this problem, the RC4 implementation designed for WEP makes use of a 24 bits Initialization Vector (IV) as a concatenated prefix of the key. This IV is then sent unencrypted with the cyphertext to enable decryption on the receiver side. The receiver must still know the key to be able to recover the original plaintext.

![](<../../.gitbook/assets/image (82).png>)

Key Index is a number from 0 to 3 that stands as a key identifier. In fact most wireless routers optionally let you specify 4 WEP keys and then choose which one to use. This mechanism was introduced to facilitate key changes in large organizations.

![](<../../.gitbook/assets/image (83).png>)

The last part of the WEP frame is the Integrity Check Value. This is a 4-byte CRC code of the original unencrypted frame. It is appended to the plaintext data and subsequently sent encrypted along with the actual payload. Its purpose is to detect frame tampering by an attacker. WEP vulnerabilities make this integrtiy protection much less effective.

#### WEP Flaws

The first and simplest WEP flaw descends from the short length of the IVs. As this is only 24 bits, there is a 50% probability that the same IV will be repeated after only 5000 packets and so also the related keystream would be reused.

In a semi-busy network, the packet rate is large enough to assure repetitions will happen quite often.

Keystream reuse is a critical vulnerability.

If the attacker can get two cyphertexts that were encrypted with the same keystream and has knowledge about on of the two plaintexts, he can recover the other messages with a simple operation.

{% hint style="info" %}
To see why IV repetition is a problem, we need to understand some formulas. Let us indicate:

* P1 and P2 as two plaintext messages
* C1 and C2 as the corresponding cyphertexts.
* RC4(IV1, K) and RC4(IV2, K) as the generated keystreams used to encrypt P1 and P2.

Both messages are encrypted with the same key K but different IVs, so:

&#x20;C1 = P1⊕RC4(IV1, K) ; C2 = P2⊕RC4(IV2, K)

IF the used IVs are the same we get:

C1⊕ C2 = P1⊕RC4(IV1, K) ⊕ C2

Substituting C2 on the right we obtain:

C1⊕ C2 = P1⊕RC4(IV1, K) ⊕ P2⊕RC4(IV1, K)&#x20;

This can be further simplified resulting in:

C1⊕ C2 = P1⊕ P2

> Therefore, w can see that a known plaintext attack can be applied.

In fact, if an attacker knows the content of P2 or P2 message (or a prefix of it) the other message can be obtained with a simple XOR operation as the attacker knows all the other values of the equation (C1 and C2 are sent on the wireless medium and can be sniffed by any wireless-enabled device).
{% endhint %}

WEP cannot guarantee the confidentiality of the encrypted data. Another thing to note about this theoretical exposure is that it is possible to abuse the vulnerability for the inverse task of recovering the keystream.

While data confidentiality is one of the security requirements of an encryption algorithm, it is not he only one. A good algorithm must also provide data integrity protection.

An attacker that has obtained an encrypted frame should not be able to modify that frame and re-inject it into the network successfully.

WEP uses CRC-32 to calculate a checksum (the ICV/CRC) of the payload before encryption; the ICV is then sent encrypted along with the message.

When decrypting, the receiver calculates the ICV in the same different way.

If the calculated version and the received one do not match, the frame is discarded so tampering the encrypted data should not be easy for an attacker who lacks knowledge of the key.

The main problem of this approach is that CRC-32 was not intended as a security measure when it was designed. Its main purpose is to provide error checking during transmissions or storage of data, using statistical methodologies. This leads to another major WEP flaw: it is actually possible to make controlled changes to the frame payload and re-inject it without the receiver noticing.

{% hint style="info" %}
**Linearity of the CRC algorithm **_**and bit-flipping attack**_

Given two messages M1 and M2, the following is valid:

CRC(M1) ⊕ CRC(M2) = CRC(M1⊕ M2)&#x20;

The above equation simply states the linearity of the CRC function with the XOR operation.

Suppose now that a message M was sent encrypted on the network with cyphertext C:

C=RC4(IV,K) ⊕ (M,CRC(M))

The attacker can now build a new cyphertext C' that is decrypted to (M', CRM(M')) without knowing the network key.

He chooses an arbitrary message Δ and uses it to compute C':

C' = C ⊕ (Δ,CRC(Δ))&#x20;

\= RC4(IV, K) ⊕ (M,CRC(M)) ⊕  (Δ,CRC(Δ))&#x20;

\= RC4(IV, K) ⊕ ( M ⊕ Δ, CRC(M) ⊕ CRC(Δ))&#x20;

\= RC4(IV, K) ⊕ ( M ⊕ Δ, CRC(M ⊕ Δ))

\= RC4(IV, K) ⊕ ( M', CRC(M'))  # here we apply CRC linearity!

The major limitation of the attack derives from the fact that the attacker does not know the plaintext content of the original message but he does know is that a "1" in a specified position in the Δ message produce a "flipped" bit in the corresponding position in M'.

This is called a _bit-flipping attack_. Using this technique, an attacker can make controlled changes that will not be detected by the receiver.
{% endhint %}

While these design flaws surfaced almost after WEP specification release, further analysis of plain RC4 cypher and particularly of its usage mode in WEP showed that the encryption schme could be broken by a series of statistical attacks.

Back in 2001, Scott Fluhrer, Itsik Mantin and Adi Shamir published a paper exposing a methodology to take advantage of the way RC4 cipher and IV are used in WEP in order to recover the network key.

The authors discovered correlations between the first bytes of the keystream and the key itself resulting in a passive attack that can recover the RC4 key after eavesdropping on the network long enough to collect a sufficient amount of packets (typically around 4 million).

The attack was named **FMS** after the names of the original discoverers.

The **FMS Attack** relies particularly on a subset of the possible IVs, named "weak IVs". While the other IVs are simply discarded during the attack, these weak IVs can "leak" portions of the key; statistical attacks can be performed in order to fully recover the network key.

Implementation of the attack are available in the `aireplay-ng` tool.

Most vendors implemented countermeasures against the FMS attack. An obvious one was detecting and avoiding the use of "weak IVs". This countermeasure proved to be insufficient; in 2004 a person using the pseudonym **KoreK** posted a series of 17 statistical attacks that do not need weak IVs and reduced the number of frames to recover the key to less than 500000.

Andreas Klein, in 2005, discovered even more correlations between the key and the RC4 generated keystream.

In 2007, Andrei Pychkine, Erik Tews and Ralf-Philipp Weinmann were able to optimize Klein's attack and apply to the WEP scenario in a powerful new methodology.

With this new attack, named **PTW** from the initials of the original authors, it is possible to recover a 104-bit WEP key with 50% probability using only 40000 captured packets.

{% embed url="https://eprint.iacr.org/2007/120.pdf" %}

#### WPA: Wi-Fi Protected Access

It became available in 2003 as part of the IEEE 802.11i standard draft. It was intended as a replacement for WEP while WPA2 was in development stage. WPA specification solves the flaws that plagued WEP.

The main addition was the use of a per-packet 128 bit key, generated using the Temporal Key Integrity Protocol (TKIP), a feature that prevents the types of attacks that compromised WEP. This means that for each packet, a new key is dynamically generated.

Another feature of WPA is the addition of a message integrity check (MIC). This is designed to prevent an attacker from capturing, altering and/or resending data packets; this replaces the Cyclic Redundancy Check (CRC) used by WEP that could not provide any security guarantee.

WPA2 is the result of the final IEEE 802.11i specifications and was published in 2004. The new standard deprecates the use of TKIP in favor of CCMP, a new AES-based encryption scheme with strong security properties.

While WPA was a huge improvement over WEP with regard to security guarantees, TKIP was still based on the RC4 cypher. This was a design requirement as it allowed adoption of the new algorithm implementation through a simpler software/firmware upgrade.

However, researchers were able to demonstrate attacks on WPA when TKIP encryption was in use by exploiting some of the known flaws existing in RC4.

These attacks, developed by Martin Beck, Eric Tews, Toshihiro Ohigashi and Masakatu Morii, circumvent WPA anti-replay protection and enable the attacker to decrypt one packet and use the obtained keystream to forge and inject up to 7 new frames. While the discovered attacks are interesting on theoretical bases, they are not practical and can not be used to recover the network key.

{% embed url="https://dl.aircrack-ng.org/breakingwepandwpa.pdf" %}

Since TKIP is completely secure and has been deprecated, to guarantee the best security of a Wi-Fi network, WPA2 with CCMP/AES encryption must be used.

#### Authentication

In order to exchange messages, client stations must be associated with an AP. Before this can happen, stations need to authenticate themselves, proving they have the rights to access the wireless network.

802.11 specifications describes three possible connection states for a client that model this process:

1. Not authenticated
2. Authenticated but not associated
3. Authenticated and associated

The 802.11 original standard specified two different station's authentication modes:

* Open Authentication
* Sharked Key Authentication (SKA)

{% hint style="info" %}
**Open Authentication**

If Open Authentication is enabled on the AP, the client station simply sends an Authentication Request frame specifying the target SSID, and receives an Authentication Response with a successful result.

In this case, the client does not need to provide any proof beside the SSID; given that this information is broadcasted by the AP in beacon frames, it cannot be considered a secret.

Open Authentication messages flow:

1. STA → AP: Open System Authentication Request
2. STA ← AP: Open System Authentication Response
3. STA → AP: Association Request
4. STA ← AP: Association Response

Truth is that something could go wrong along the way due to transmission errors, stations incompatibilities but also to MAC filtering features enabled on the AP. When one of these happens, the AP will report a failure status code in its Authentication Response.

It's also useful to note that all of the messages exchanged during the process are sent unencrypted; WEP encryption is used only for data frames sent immediately after a successful authentication.
{% endhint %}

{% hint style="info" %}
**Shared Key Authentication**

SKA is available only when WEP is enabled. Different from the Open mode, when receiving an Authentication Request, the AP responds with a challenge text (128 bytes). The client needs to encrypt the challenge with the shared WEP key and return it to the AP in the next frame. Then the AP compares the decrypted challenge to the known plaintext and successfully authenticates the client if they are equal.

1. STA → AP: Authentication Request
2. STA ← AP: Challenge Text
3. STA → AP: Encrypted Challenge&#x20;
4. STA ← AP: Authentication Response

The Association messages are removed but are obviously necessary.&#x20;

While it may seem like SKA is an improvement over simple open authentication, _it can simplify certain attacks against WEP_. In fact, an attacker is able to recover 128 bytes of keystream given that the challenge text that was sent unencrypted. **This can be used to forge encrypted packets without actually knowing the key!**

An attacker will be able to authenticate to the AP once he has snooped over at least one authentication message flow.&#x20;

This clearly makes the Shared Key Authentication completely broken so you should not rely on it for any security requirements.&#x20;
{% endhint %}

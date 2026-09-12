

# HARDENING AND ENCRYPTION

By: Sumeet Chand

Date: FEBRUARY 2025

# TABLE OF CONTENTS
- [1. Terminologies](#terminologies)
- [2. Common Commands](#common-commands)
- [3. Encryptions](#encryptions)
- [3.1 End-to-End Encryptions (E2EE)](#end-to-end-encryptions-(E2EE))
- [3.2 VPNs](#VPNs)
- [3.3 Encrypting Text Messaging](#encrypting-text-messaging)
- [3.4 Encrypting Calls](#encrypting-calls)
- [4. Kill Switches](#kill-switches)
- [5. Hardening](#hardening)
- [5.1 Desktop Hardening](#desktop-hardening)
- [5.2 Cloud Instance Hardening](#cloud-instance-hardening)
- [5.3 Mobile Hardening](#mobile-hardening)
- [5.4 Email Hardening](#email-hardening)
- [5.5 Network Hardening](#network-hardening)

# TERMINOLOGIES


# COMMON COMMANDS


# ENCRYPTIONS
Encryptions are protecting data traffic to and from a network.

## END 2 END ECRYPTIONS (EE2E)
Is the definition of encrypting all the information from point to point.

## VPNs
A VPN (Virtual Private Network) encrypts your internet traffic and routes it through a remote server, hiding your IP address and protecting your data from being intercepted by hackers, ISPs, or other third parties. They achieve this by providing a server for you to remotely connect to from wherever you are, encrypting all the traffic to and from that server. This allows you to be in one country and "tunnel" into another country, enabling you to browse the internet as if you were a local resident of that country.

### Example Use Cases:
Remote Work Security: You are working from home and need to connect to your company's network. A VPN ensures that no one can intercept sensitive work data.

* Regional Testing: You are testing how a website appears in different countries. By using a VPN to connect to a server in another country, you can see regional differences, such as content variations, pricing, and more.

* Bypassing Geo-Restrictions: Access streaming services, websites, or content that is restricted in your region by connecting to a VPN server in a country where the content is available.

* Public Wi-Fi Security: When using public Wi-Fi networks (e.g., in cafes or airports), a VPN encrypts your connection, protecting your data from hackers or snoopers.

* Online Privacy: A VPN masks your IP address, making it harder for advertisers, websites, or third parties to track your online activities.

* Avoiding Censorship: In countries with strict internet censorship, a VPN can help you access blocked websites and services.

### Example VPN's

* Cisco SecureConnect is a popular enterprise VPN used for secure remote work.

* ProtonVPN is a widely used consumer VPN for privacy and security on phones and computers.

* Other popular VPNs include NordVPN, ExpressVPN, and Surfshark.


### Encrypting Text Messaging (e.g., SMS over Cellular/Wi-Fi)

Traditional SMS (Short Message Service) is not encrypted by default, making it vulnerable to interception. 
Non-text messaging includes multimedia messages, voice messages, and file sharing.
To encrypt any message, you should avoid using standard SMS and or default inbuilt messaging apps and switch to secure messaging apps that offer end-to-end encryption (E2EE). Here’s how:

Do NOT use: Standard SMS or messaging apps without encryption (e.g., default SMS apps on most phones).

Use instead: Apps like Signal or Threema, which encrypt all messages by default, ensuring only the sender and recipient can read them.

Example: Install Signal, enable it as your default messaging app, and use it to send encrypted texts over Wi-Fi or cellular networks.

### Encrypting Calls (e.g., Calling a Friend over 4G/5G)

Standard voice calls over cellular networks (4G/5G) are encrypted by the network itself (e.g., LTE/5G encryption), but this encryption is not end-to-end. For fully private calls, you should use apps that offer end-to-end encryption for voice calls. Here’s how:

Do NOT rely on: Standard cellular calls e.g, calling over a phone's carrier network, as they are only encrypted between your device and the cell tower, not end-to-end.

Use instead: Apps like Signal, WhatsApp, too make calls using the internet traffic not the cellular phone network, which encrypt voice calls end-to-end.

Example: Install Signal and use it to make encrypted voice calls over Wi-Fi or cellular data. This ensures that only you and the recipient can hear the conversation.

# KILL SWITCHES

Kill switches block internet access, or trigger tasks like kill apps, or wipe OS systems whenever unexpected behaviour occurs such as VPN connection drops, or the OS password was entered incorrectly too many time,, or an Anti Virus aleet is triggered stopping further propogation of malicious virus such as ransomware.

# HARDENING
Hardening a system refers to the process of securing it against potential threats. There are multiple approaches to hardening, but the ultimate goal is to protect the system from malicious activities.

#### DESKTOP HARDENING
* Bitlocker: protect the physical data storage (drive) by encrypting the contents so that anyone that steals or access the physical drive cannot
access the digital data inside
* Least Privilege Principle: Install and run applications with the minimum necessary permissions.
* Least Privilege Access: Restrict user access to only what is required for their role.
* Anti-Virus Software: Utilize tools that employ virus database definition matching against run executable hashes and behavior analysis to detect and prevent malware.
* Firewalls: Protect network traffic by controlling and locking down unnecessary ports.
* DNS Filtering: Filter websites browsed by matching against known malicious databases
* VPNs: Use Virtual Private Networks to secure and encrypt internet connections.
* Kill Switches: Implement kill switches to drop network traffic when Tunnels (e.g, VPN's are dropped) or on too many incorrect
attempts to login to the device

#### CLOUD INSTANCE HARDENING
* Security Rules: Configure and enforce strict access control policies.
* Firewalls: Implement cloud-based firewalls to monitor and filter incoming and outgoing traffic.

#### MOBILE HARDENING

* OS Hardening: Android is an popular phone optimised OS made by Google, and the Pixel is an example of a Google Android phone, however Android is open source and
can be installed on any hardware from Samsung, Huawei or more.
iOS the OS for Apple phones comes pre-hardended as its a propriety system its already strengthened.
For any Android phone e.g, Samsung, Huawei, CalyxOS (for select devices) or LineageOS (for broader compatibility, but with fewer security enhancements)
are the best OS for hardening the phone.
For Google Pixel devices use a custom hardened operating system like GrapheneOS instead of the traditional Android OS. GrapheneOS is a privacy-focused, open-source operating system based on Android. It removes Google services, adds advanced security features, and provides regular updates.
Visit the official GrapheneOS website: https://grapheneos.org/install/

* Browser Hardening: Use a secure browser like Vanadium (included in GrapheneOS) or Bromite for enhanced privacy and security.
Vanadium is a privacy-focused browser based on Chromium, with additional security features like stricter sandboxing and tracking protection.
Download Bromite from its official website: https://www.bromite.org

* Data Encryption: Enable full-disk encryption on your device (most modern smartphones have this enabled by default).

* Data Encryption: Use encrypted communication apps like Signal or Threema for secure data transmission.
Avoid installing Signal or Threema from the Google Play Store to reduce reliance on Google services.
Download the Signal APK free from the official website: https://signal.org/android/apk or
Download the Threema APK from the official website: https://shop.threema.ch/.

* VPNs: Employ reliable VPN services such as ProtonVPN or Mullvad to protect data in transit and mask your IP address.
Download the ProtonVPN APK from the official website: https://protonvpn.com/support/android-vpn-setup.
Download the Mullvad APK from the official website: https://mullvad.net/en/download/android.

* Kill Switches: Implement features like auto-incrementing lockout timers (e.g., iOS) or remote wipe capabilities to protect data in case of theft or unauthorized access.
Use strong passphrases or biometric authentication (e.g., fingerprint or face unlock) to secure your device.
Enable Find My Device (Android) or Find My iPhone (iOS) for remote tracking and wiping.

#### EMAIL HARDENING
* SPAM Filters: Deploy advanced spam filters like Avanan, which analyze email headers and content to detect and block malicious emails.

#### NETWORK HARDENING
Network Security Appliances: Use devices and software from trusted vendors like Cisco to secure network infrastructure.
* Regular Audits: Conduct frequent security assessments to identify and address vulnerabilities.


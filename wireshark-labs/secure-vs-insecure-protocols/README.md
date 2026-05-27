# Application Layer Security Analysis: Cleartext vs. Encrypted Protocols (FTP, Telnet, SSH & NTP)

## 📊 Project Presentation & Lab Captures
* 📂 **[Click here to view the full Project Walkthrough Slides (PDF)](./project-walkthrough.pdf)**
* 📦 **[Browse Raw Wireshark Capture Files (.pcapng)](./captures/)**

---

## 📌 Project Brief & Objectives
The final phase of this Wireshark packet analysis series evaluates Layer 7 (Application Layer) security architectures and core network infrastructure support. By contrasting legacy cleartext communication protocols against modern cryptographic implementations, this lab demonstrates the operational risk of plain-text data exposure on the wire and underscores the necessity of encryption in transit.

<p align="center">
  <img src="screenshots/01_project_brief_RM.png" width="90%" alt="Project Brief and Objectives">
</p>

---

## 🛠️ Diagnostics & Methodology
* **Network Analyzer:** Wireshark v4.6.5
* **Execution Utility:** Windows Command Prompt (CMD), native administrative utilities (`pkgmgr`), and protocol-specific client connections.
* **Core Concepts Covered:** Application layer encapsulation, cleartext credential exposure, TCP stream reconstruction, asymmetric-to-symmetric key exchange, and network time synchronization.

---

## 🔍 Protocol Deep Dive & Packet Analysis

### 1. Insecure File Transfers: FTP Analysis
* **Wireshark Filter:** `ftp`
* **Target Server:** `test.rebex.net`
* **Captured File:** `01_ftp_cleartext_transfer.pcapng`

An anonymous file transfer session was established over TCP Port 21 to request and fetch a text payload. Because FTP uses distinct control and data channels without underlying TLS encapsulation, all application parameter commands and arguments are broadcast openly.

**Wireshark Analysis:**
Inspecting the network stream exposes the fundamental structural flaw of legacy file delivery protocols. Authentication strings, response codes, and system parameters are captured entirely in plain text.

<p align="center">
  <img src="screenshots/02_ftp_wireshark_cleartext_RM.jpg" width="95%" alt="FTP Cleartext Leakage in Wireshark">
  <br>
  <em>Figure 1: Complete transparency of FTP parameters, commands, and client configuration on the wire.</em>
</p>

---

### 2. Plaintext Terminal Emulation: Telnet Analysis
* **Wireshark Filter:** `telnet`
* **Target Server:** `towel.blinkenlights.nl`
* **Captured File:** `03_telnet_cleartext_session.pcapng`

Telnet enables remote terminal-to-terminal emulation via TCP Port 23. To safely benchmark its architectural layout, a connection was instantiated to a public text-rendering server displaying an ASCII animation sequence.

<p align="center">
  <img src="screenshots/03_telnet_starwars_cmd_RM.png" width="90%" alt="Telnet Command Execution">
  <br>
  <em>Figure 2: Establishing a plain-text Telnet session via Command Prompt.</em>
</p>

**Wireshark Analysis:**
Reconstructing the transport stream highlights Telnet's severe vulnerabilities. Because every character typed or returned is serialized inside independent, unencrypted TCP segments, an adversary running a packet sniffer can extract the entire transaction seamlessly.

<p align="center">
  <img src="screenshots/04_telnet_cleartext_bytes_RM.jpg" width="95%" alt="Telnet Stream Decoding in Wireshark">
  <br>
  <em>Figure 3: Intercepted TCP data tracking showing character-by-character transmission of the session payload.</em>
</p>

---

### 3. Cryptographically Protected Remote Access: SSH Analysis
* **Wireshark Filter:** `ssh`
* **Captured Files:** `02_ssh_sftp_handshake.pcapng` & `04_ssh_encrypted_traffic.pcapng`

To validate modern protective design patterns, a secure alternative was audited using the Secure Shell (SSH) protocol on TCP Port 22. SSH counters sniffing vulnerabilities by using a cryptographic handshake (such as Diffie-Hellman) to establish symmetric session keys.

**Wireshark Analysis:**
Following the initialization phase, all subsequent upper-layer payload segments are completely masked. The raw packet data is wrapped into generic encrypted payload definitions, ensuring data confidentiality and preventing session hijacking or intermediate manipulation.

<p align="center">
  <img src="screenshots/05_ssh_encrypted_bytes_RM.jpg" width="95%" alt="SSH Encrypted Segment Verification">
  <br>
  <em>Figure 4: Secure encapsulation showing that upper-layer payloads are fully obfuscated on the network layer.</em>
</p>

---

### 4. Infrastructure Synchronization: NTP (Network Time Protocol)
* **Wireshark Filter:** `ntp`
* **Port / Protocol:** Port 123 / UDP
* **Captured File:** `05_ntp_clock_synchronization.pcapng`

Network time synchronization is critical for log validation, timestamping cryptographic certificates, and managing stateful operations. A manual synchronization event was triggered using the Windows Time configuration architecture
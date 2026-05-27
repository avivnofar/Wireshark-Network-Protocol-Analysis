# Application Layer Security & Infrastructure Analysis: Secure vs. Insecure Protocols (FTP, SSH, Telnet & NTP)

## 📊 Project Presentation & Quick Links
* 📂 **[Click here to view the full Presentation Slides (PDF) directly in your browser](./secure-protocols-walkthrough.pdf)**
* 📦 **[Browse Raw Wireshark Capture Files (.pcapng)](./captures/)**

---

## 📌 Project Brief & Objectives
The final phase of this Wireshark packet analysis series focuses heavily on Layer 7 (Application Layer) security architectures and core network infrastructure management. By evaluating legacy cleartext communication protocols against modern cryptographic variants, this lab demonstrates the absolute necessity of structural network defense and payload encryption. Additionally, it explores network time synchronization mechanics via UDP.

<p align="center">
  <img src="screenshots/RM_resources/01_project_brief_RM.png" width="90%" alt="Project Brief and Objectives">
</p>

---

## 🛠️ Diagnostics & Methodology
* **Network Analyzer:** Wireshark v4.6.5
* **Execution Utility:** Windows Package Manager (`pkgmgr`), Windows Command Prompt (CMD), and native FTP/Telnet/SSH clients.
* **Target Scenarios:** Automated feature deployment, anonymous file transfers, cleartext terminal parsing, asymmetric-to-symmetric key exchange, and network clock synchronization.

---

## 🔍 Step-by-Step Technical Deep Dive

### 0. Environment Setup & Feature Provisioning
To capture raw legacy traffic, the native Windows Telnet client and FTP utilities were verified and initialized via administrative CLI commands, ensuring the local network subsystem was fully equipped to handle transport-layer emulation.

<p align="center">
  <img src="screenshots/RM_resources/02_telnet_ftp_setup_cmd.png" width="90%" alt="Windows Feature Provisioning">
</p>

---

### 1. Insecure File Ingestion: FTP Analysis
* **Wireshark Filter:** `ftp`
* **Target Server:** `test.rebex.net` (Public Testing Server)
* **Captured File:** `01_ftp_cleartext_transfer.pcapng`

An anonymous FTP session was initiated via the command line interface to request and pull a sample payload (`readme.txt`). 

<p align="center">
  <img src="screenshots/RM_
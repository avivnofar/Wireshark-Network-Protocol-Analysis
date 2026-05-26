# Network Protocol Analysis: DHCP, DNS, and ARP Lifecycle

## 📊 Project Presentation & Quick Links
* 📂 **[Click here to view the full Presentation Slides (PDF) directly in your browser](./project-walkthrough.pdf)**
* 📦 **[Raw Wireshark Capture File (.pcapng)](./captures/restart%20dns%20flow%20capture.pcapng)**

---

## 📌 Project Brief
This hands-on lab demonstrates practical network analysis and troubleshooting techniques by capturing and dissecting core network protocols (**DHCP, DNS, and ARP**). By systematically manipulating the Windows network stack via the command line, I generated real-world traffic scenarios to analyze packet structures, handshakes, and protocol mechanics under Wireshark.

<p align="center">
  <img src="screenshots/RM_resources/01 project_brief_RM.png" width="90%" alt="Project Brief Overview">
</p>

---

## 🛠️ Tools & Methodology
* **Network Analyzer:** Wireshark v4.6.5
* **Environment:** Windows 11 Environment
* **Diagnostic CLI Tools:** Windows Command Prompt (CMD)

### Test Execution Strategy:
To cleanly capture the protocol lifecycle without background network noise, the local adapter state was altered using targeted commands:
1. `ipconfig /release` - Forces the client to surrender its current IP lease to the DHCP server.
2. `ipconfig /flushdns` - Clears the local DNS cache to guarantee fresh query generation.
3. `ipconfig /renew` - Triggers a fresh lease allocation sequence, initiating the protocol flow.

---

## 🔍 Detailed Protocol Analysis

### 1. DHCP & The 4-Stage DORA Process
* **Wireshark Filter:** `dhcp` or `bootp`

During normal execution, a standard `ipconfig /renew` on an active interface often triggers an optimized **2-stage handshake** (Request -> ACK) because the lease configuration is remembered. 

To force and observe the classic, complete **4-stage DORA process**, a cold connection sequence was simulated by cycling the interface completely. 

<p align="center">
  <img src="screenshots/RM_resources/04 wireshark_dora_RM.png" width="95%" alt="DHCP DORA 4-Stage Capture">
  <br>
  <em>Figure 1: Full DHCP DORA negotiation sequence (Discover, Offer, Request, ACK).</em>
</p>

**Analysis Breakdown:**
* **Discover (0.0.0.0 $\rightarrow$ 255.255.255.255):** The client broadcasts to locate any available DHCP server.
* **Offer:** The server proposes an available IP address configuration.
* **Request:** The client formally requests the offered configuration.
* **ACK:** The server acknowledges and commits the network configuration lease.

---

### 2. DNS Resolution Sequence
* **Wireshark Filter:** `dns`

Following the cache purge, an `nslookup google.com` command was executed to capture the raw name-to-IP resolution sequence.

<p align="center">
  <img src="screenshots/RM_resources/03 wireshark_dns_RM.jpg" width="95%" alt="DNS Wireshark Dissection">
  <br>
  <em>Figure 2: DNS Query and Response lifecycle over UDP Port 53.</em>
</p>

**Key Observations:**
* **Layer 4 Transport:** Resolution relies strictly on **UDP** to eliminate connection setup overhead and maximize speed.
* **Port Destination:** Traffic targets standard destination **Port 53**.
* **Transaction ID Validation:** The individual Query and Response packet pairs were validated and matched using their unique hexadecimal Transaction IDs.

---

### 3. ARP (Address Resolution Protocol) Mapping
* **Wireshark Filter:** `arp`

Simultaneously, ARP packets were captured as the client device broadcasted queries to discover the physical hardware addresses (MAC) corresponding to local destination IPs (such as the default gateway).

<p align="center">
  <img src="screenshots/RM_resources/02 wireshark_arp_RM.jpg" width="95%" alt="ARP Broadcast Capture">
  <br>
  <em>Figure 3: ARP Request broadcast and Point-to-Point Reply sequence.</em>
</p>

**Key Observations:**
* **Hardware Binding:** Documented the mapping sequence translating Layer 3 IP addresses to Layer 2 MAC addresses.
* **Broadcast Mechanism:** The Request packet targets the broadcast MAC (`ff:ff:ff:ff:ff:ff`), forcing all nodes to listen, while the Reply is an efficient unicast direct to the requester.

---

## 📂 Repository Structure Reference
* `/captures/` - Contains the live `.pcapng` packet capture stream.
* `/screenshots/RM_resources/` - Holds documentation assets and structural figures.
* `project-walkthrough.pdf` - Full executive slide presentation.
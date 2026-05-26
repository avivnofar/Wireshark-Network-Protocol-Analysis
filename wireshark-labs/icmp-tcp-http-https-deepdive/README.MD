# Network Diagnostics & Secure Transport Analysis: ICMP, TCP, HTTP/HTTPS & TLS

## 📊 Project Presentation & Quick Links
* 📂 **[Click here to view the full Presentation Slides (PDF) directly in your browser](./project-walkthrough.pdf)**
* 📦 **[Browse Raw Wireshark Capture Files (.pcapng)](./captures/)**

---

## 📌 Project Brief & Objectives
This comprehensive hands-on lab demonstrates technical competency in network diagnostics, transport layer reliability, and application layer security. By capturing live traffic streams across multiple layers of the OSI model, I isolated and evaluated core routing mechanisms, cryptographic handshakes, and the structural differences between cleartext and encrypted web protocols.

<p align="center">
  <img src="screenshots/RM_resources/01_project_brief_RM.png" width="90%" alt="Project Brief and Objectives">
</p>

---

## 🛠️ Diagnostics & Methodology
* **Network Analyzer:** Wireshark v4.6.5
* **Execution Utility:** Windows Command Prompt (CMD) & `curl` CLI
* **Target Scenarios:** Diagnostic probing, L4 connection lifecycles, unencrypted Layer 7 fetching vs. cryptographic secure sessions.

---

## 🔍 Layer-by-Layer Protocol Deep Dive

### 1. Network Layer Diagnostics: ICMP (Ping & Traceroute)
* **Wireshark Filters:** `icmp`
* **CLI Execution:** `ping google.com` / `tracert google.com`

This section isolates the Behavior of the Internet Control Message Protocol (ICMP). By evaluating Echo Requests/Replies and tracking Time-to-Live (TTL) decrements, I analyzed hop-by-hop traversal across the WAN.

<p align="center">
  <img src="screenshots/RM_resources/02_icmp_tracert_wireshark_RM.jpg" width="95%" alt="ICMP Traceroute Stream Analysis">
  <br>
  <em>Figure 1: ICMP packet stream mapping out TTL progression and router response mapping.</em>
</p>

**Key Insights:**
* **TTL Architecture:** Documented standard Windows TTL initiation ($TTL=128$) versus intermediate router decrements, verifying exactly 13 logical routing hops to destination.
* **Sequence Verification:** Validated strict 1:1 Transaction matching via internal ICMP Sequence numbers.

---

### 2. Application Layer Contrast: HTTP Cleartext Analysis
* **Wireshark Filters:** `http`
* **CLI Execution:** `curl http://example.com`

To analyze an unencrypted session, `curl` was utilized to generate a standard HTTP GET request. The stream exposes how plain-text protocols transmit payload architecture natively without encryption overhead.

<p align="center">
  <img src="screenshots/RM_resources/03_http_curl_wireshark_RM.jpg" width="95%" alt="HTTP Cleartext Protocol Inspection">
  <br>
  <em>Figure 2: Inspection of plain text HTTP GET and 200 OK Response string.</em>
</p>

**Key Insights:**
* **Total Exposure:** Application payload strings, server identities, and content architecture are completely readable within the packet byte-stream, demonstrating high vulnerability to sniffing attacks.

---

### 3. Cryptographic Security & Transport Layer: TCP, TLS, and OCSP
* **Wireshark Filters:** `tcp` / `tls` / `ocsp`
* **CLI Execution:** `curl https://google.com`

This section tracks the complex transition from transport layer reliability to high-assurance data privacy via HTTPS on TCP Port 443.

<p align="center">
  <img src="screenshots/RM_resources/04_tcp_tls_handshake_RM.jpg" width="95%" alt="TCP and TLS Handshake Capture Sequence">
  <br>
  <em>Figure 3: Combined capture capturing the Layer 4 3-Way Handshake immediately followed by the TLS Cryptographic Handshake.</em>
</p>

**Key Insights:**
* **Layer 4 Reliability:** Isolated the standard TCP 3-Way Handshake sequence: `SYN` $\rightarrow$ `SYN-ACK` $\rightarrow$ `ACK`.
* **Asymmetric to Symmetric Transition:** Dissected the TLS negotiation sequence including `Client Hello` (cipher suite proposals) and `Server Hello` (certificate provisioning).
* **Identity Assurance (OCSP):** Captured Online Certificate Status Protocol (OCSP) queries verifying real-time certificate validation and non-revocation status with the Certificate Authority (CA).

---

## 📊 Comprehensive Protocol Comparison (HTTP vs. HTTPS)

The core architectural differences between a standard web connection and a secure deployment are outlined in the evaluation matrix below:

<p align="center">
  <img src="screenshots/RM_resources/05_http_https_comparison_RM.png" width="85%" alt="HTTP vs HTTPS Protocol Matrix">
</p>

---

## 📂 Repository Structure Reference
* `/captures/` - Raw packet captures (`.pcapng`) for ICMP, Traceroute, and Curl sessions.
* `/screenshots/RM_resources/` - Granular visual step documentation.
* `project-walkthrough.pdf` - Consolidated slide presentation for executive review.
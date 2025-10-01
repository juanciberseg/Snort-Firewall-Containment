# Technical Analysis Report: Snort IDS and Iptables Contention

## 1. Executive Summary

This report details a simulated security incident involving the successful detection and immediate containment of traffic related to a known malicious server. **Snort IDS** identified the signature, providing the necessary **5-Tuple** information (Destination IP: 209.165.202.133, Port: 6666) to implement a **Linux iptables firewall rule**. The rule was successfully applied to the perimeter router (R1), blocking the traffic and demonstrating effective network-level control.

## 2. Detection and Identification

The detection phase focused on monitoring network perimeter traffic passing through the **Router/Firewall/IDS (R1)** device.

### 2.1 Network Topology
The lab environment utilizes a Mininet topology where the perimeter defense (R1) sits between the internal networks and the ISP Router.
* [View Network Topology Diagram](Evidence/Capture_01.png)

### 2.2 Initial Traffic and Detection
The analyst, simulating a user on **H5** (DMZ Server), executes a malicious file download.
* **Initial Access:** The `wget` command successfully downloads the malicious file (`W32.Nimda.Amm.exe`), confirming the communication path before any blocking action. [View Wget Successful Download](Evidence/Capture_02.png)
* **Real-Time Monitoring:** **Snort IDS** runs on the perimeter router (R1), monitoring traffic. The download event immediately triggers an alert.
    * [View Snort Real-Time Alert Log (Tail Output)](Evidence/Capture_03.png)
* **Packet Capture:** To preserve the evidence, a packet capture was initiated on H5 *before* the second download attempt. [View Tcpdump Command Initiation](Evidence/Capture_04.png)
* **Capture Verification:** The saved capture file (`nimda.download.pcap`) was verified on the host. [View Packet Capture File Verification (ls -l)](Evidence/Capture_05.png)

## 3. Containment and Verification

The containment phase involved using the data from the Snort alert to implement a precise packet filtering rule on the **iptables** firewall running on R1.

### 3.1 Containment Action
Traffic originating from H5 and destined for the malicious server **traverses** the firewall R1, making the **FORWARD** chain the correct target for the blocking rule.

* **Rule Implemented:** The rule was added to **DROP** TCP packets destined for `209.165.202.133` on port `6666`.
    ```bash
    iptables -I FORWARD -p tcp -d 209.165.202.133 --dport 6666 -j DROP
    ```
* **Verification:** The `iptables -L -v` command confirms the rule is in place, successfully blocking traffic to the malicious server. [View Iptables Rule Implementation and Verification](Evidence/Capture_06.png)

### 3.2 Post-Containment Test
A final `wget` test from H5 after the rule was implemented would confirm a **`Connection timed out`** error, validating the firewall's effectiveness (as shown in the lab instructions).

## 4. Remediation and Recommendations

### 4.1 Eradication and Remediation
1.  **Rule Persistence:** The implemented `iptables` rule must be saved to ensure persistence across reboots.
2.  **Host Cleanup:** The downloaded malware file (`W32.Nimda.Amm.exe`) must be purged from the host H5.
3.  **User Education:** The user on H5 should be re-educated regarding safe file download practices.

### 4.2 Recommendations (Preventive Measures)
1.  **Default Deny Policy:** Implement a "Deny by Default" policy on the firewall's `FORWARD` chain, allowing only explicitly approved traffic.
2.  **IPS Implementation:** Consider transitioning Snort from pure IDS (Detection) mode to an IPS (Prevention) mode to automatically drop packets matching malicious signatures.

# IDS/IPS Rule Implementation: Snort and Iptables Firewall Hardening

## Overview
This project simulates a live security incident to demonstrate perimeter defense and hardening skills. The process involved monitoring network traffic with **Snort IDS** to detect a malware download event. Based on the alert data, a **granular firewall rule using iptables** was immediately implemented on the perimeter router (R1) to block the malicious server's IP and port, effectively containing the threat and preventing future access.

## Key Findings

* **Positive Detection:** Snort IDS successfully triggered a `Malicious Server Hit!` alert on the perimeter router (R1).
* **Traffic Identification:** The download attempt was identified, detailing the malicious server IP (`209.165.202.133`) and port (`6666`).
* **Successful Containment:** A single `iptables` DROP rule was implemented in the `FORWARD` chain, successfully preventing any host from connecting to the malicious server.

## Key Technologies

* **Snort:** Intrusion Detection System (IDS) for pattern matching and real-time alerting.
* **Iptables:** Linux-based firewall used to implement granular packet filtering rules.
* **Linux/Mininet:** Virtualized network environment and host operating systems (Prioritizing Linux Mint).
* **Wget, Tcpdump:** Command-line tools used for simulation and packet capture.
* **Network Protocols:** TCP, HTTP.

## Documentation
* [Technical Analysis Report](ANALYSIS_REPORT.md)
* [Evidence Screenshots](Evidence/)

## License
This project is licensed under the [MIT License](LICENSE).

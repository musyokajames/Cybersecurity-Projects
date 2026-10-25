# MITRE ATT&CK Mapping

The following techniques were identified during the packet capture analysis.

| Technique                                  | ID    | Evidence in PCAP                                                                                      |
| :----------------------------------------- | :---- | :---------------------------------------------------------------------------------------------------- |
| **Network Service Discovery**              | T1046 | High volume of SYN packets sent to multiple ports in rapid succession.                                |
| **Application Layer Protocol**             | T1071 | Use of HTTP `GET` request (`/malware.exe`) over port 4444 to blend in.                                |
| **Exfiltration Over Alternative Protocol** | T1048 | Transmission of credentials (`USER=admin...`) over an unencrypted TCP channel.                        |
| **Network Sniffing**                       | T1040 | (Defender perspective) The ability to read the plaintext credentials proves the channel was insecure. |

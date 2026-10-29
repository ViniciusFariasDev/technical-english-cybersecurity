# Technical English 02 — Network Security, Firewalls and IDS/IPS

**Topic:** Network Security, Intrusion Detection, Intrusion Prevention, and Nmap Scanning Techniques  
**Level:** Intermediate (B1–B2)  
**Field:** Cybersecurity and Penetration Testing

---

## 1. Understanding Network Security

Network security is the practice of protecting computer networks, devices, and services against unauthorized access, malicious activities, and cyberattacks.

Organizations use different security technologies to monitor network traffic, detect suspicious behavior, and prevent potential threats.

Three important technologies are **firewalls, Intrusion Detection Systems (IDS), and Intrusion Prevention Systems (IPS)**.

Although these technologies have different responsibilities, they often work together to protect corporate networks.

For cybersecurity professionals, understanding how these systems operate is essential for network administration, security assessments, and penetration testing.

### Key Concepts

- **Unauthorized access:** Access to a system without permission.
- **Network traffic:** Data transmitted between devices.
- **Security threat:** A potential danger to a system or network.
- **Suspicious behavior:** Activity that may indicate a security incident.

---

## 2. What Is a Firewall?

A firewall is a security system that monitors and controls incoming and outgoing network traffic.

Its main purpose is to allow or block connections according to predefined security rules.

For example, a company may allow employees to access a web server through port 443 while blocking unnecessary connections to other ports.

Firewalls can be implemented as software, hardware, or cloud-based security services.

When a firewall receives a network packet, it evaluates the packet against its configured rules.

Depending on those rules, the firewall may allow, reject, or silently drop the packet.

### DROP vs. REJECT

**DROP:** The firewall silently discards the packet without sending a response.

**REJECT:** The firewall blocks the packet and sends an explicit response indicating that the communication was refused.

This difference is important during network enumeration because it affects how scanning tools interpret network responses.

For example, a scanner may classify a port as filtered when it receives no response.

### Example

A security administrator configures a firewall to allow HTTPS traffic on port 443 and block unauthorized access to an internal database.

The firewall acts as a barrier between trusted and untrusted networks.

---

## 3. What Is an Intrusion Detection System (IDS)?

An **Intrusion Detection System (IDS)** monitors network traffic or system activity to identify suspicious behavior and potential security threats.

An IDS analyzes information using detection rules, signatures, and other techniques.

When suspicious activity is detected, the IDS generates an alert for the security team.

However, an IDS does not normally block the suspicious connection by itself.

Its primary responsibility is detection and notification.

### Example

An attacker performs repeated connection attempts against several network services.

The IDS identifies unusual scanning behavior and sends an alert to the security administrator.

The administrator can then investigate the event and decide whether additional security measures are necessary.

**Key takeaway: An IDS detects and alerts.**

---

## 4. What Is an Intrusion Prevention System (IPS)?

An **Intrusion Prevention System (IPS)** is designed to detect suspicious activity and automatically take preventive action.

Unlike an IDS, an IPS can actively interfere with malicious traffic.

Depending on its configuration, an IPS may:

- Block suspicious connections.
- Drop malicious packets.
- Prevent access from specific IP addresses.
- Stop communication associated with known attack patterns.

For example, if an IPS detects repeated malicious requests against a web server, it may block the source IP address.

This automated response helps organizations reduce the impact of potential attacks.

However, incorrect detection rules may also block legitimate traffic.

**Key takeaway: An IPS detects and prevents.**

---

## 5. IDS vs. IPS — Understanding the Difference

Although IDS and IPS are closely related, their primary functions are different.

| Feature | IDS | IPS |
|---|---|---|
| Full Name | Intrusion Detection System | Intrusion Prevention System |
| Main Purpose | Detect threats | Detect and prevent threats |
| Generates Alerts | Yes | Yes |
| Automatically Blocks Traffic | Usually no | Yes |
| Typical Operation | Monitoring | Active prevention |

### Technical English Example

"An IDS monitors network traffic and generates alerts, while an IPS can automatically block suspicious connections."

**Portuguese:** Um IDS monitora o tráfego de rede e gera alertas, enquanto um IPS pode bloquear automaticamente conexões suspeitas.

---

## 6. Understanding TCP SYN and ACK Scanning

Nmap supports different scanning techniques to analyze TCP ports and network filtering behavior.

Two important techniques are **SYN scanning** and **ACK scanning**.

### TCP SYN Scan

A SYN scan sends a TCP packet with the SYN flag enabled.

The response helps determine whether a TCP port is open, closed, or filtered.

Typical responses include:

- **SYN-ACK:** The port is open.
- **RST:** The port is closed.
- **No response or certain ICMP errors:** The port may be filtered.

Example:

```bash
sudo nmap -sS -p 80 <target>
```

The `-sS` option performs a TCP SYN scan.

### TCP ACK Scan

An ACK scan sends TCP packets with the ACK flag enabled.

Unlike SYN scanning, ACK scanning is primarily used to investigate firewall filtering rules.

It does not reliably distinguish open ports from closed ports.

Instead, it helps determine whether traffic can pass through a filtering device.

Example:

```bash
sudo nmap -sA -p 80 <target>
```

The `-sA` option performs a TCP ACK scan.

**Important distinction: SYN scanning helps identify port states, while ACK scanning helps analyze filtering behavior.**

---

## 7. Understanding Network Scanning Performance

Network scanning performance is an important consideration during penetration testing.

Large networks may contain hundreds or thousands of devices.

Scanning every host and port can require significant time and network resources.

Nmap provides several options to control scanning speed, packet transmission, timeouts, and retries.

### Timing Templates

Nmap offers six timing templates:

| Option | Name | Description |
|---|---|---|
| `-T0` | Paranoid | Extremely slow |
| `-T1` | Sneaky | Very slow |
| `-T2` | Polite | Reduced scanning speed |
| `-T3` | Normal | Default timing |
| `-T4` | Aggressive | Faster scanning |
| `-T5` | Insane | Very aggressive scanning |

Example:

```bash
sudo nmap -p- -T4 <target>
```

This command scans all TCP ports using an aggressive timing template.

### Packet Transmission Rate

The `--min-rate` option specifies the minimum packet transmission rate that Nmap attempts to maintain.

Example:

```bash
sudo nmap -p- --min-rate 1000 <target>
```

Nmap attempts to send at least 1,000 packets per second.

However, increasing scanning speed can lead to packet loss or incomplete results.

### Retries and Timeouts

Nmap can retransmit packets when no response is received.

The `--max-retries` option limits the number of retransmissions.

The `--max-rtt-timeout` option controls the maximum amount of time Nmap waits for a response.

Reducing these values may accelerate scanning, but it can also cause important information to be missed.

**Key takeaway: Faster scanning does not always mean better scanning.**

---

## 8. Understanding Firewall Evasion Techniques

Firewall evasion refers to techniques used to test how network security controls handle different types of traffic.

During an authorized penetration test, security professionals may evaluate whether firewall rules correctly restrict access to protected services.

Nmap provides options for analyzing how filtering devices respond to different packet characteristics.

### Decoy Scanning

Decoy scanning uses additional source IP addresses in scan traffic.

The purpose is to make the original scanning source more difficult to distinguish from other apparent sources.

In Nmap, the `-D` option configures decoy addresses.

However, decoy scanning does not guarantee anonymity or successful firewall evasion.

### Source IP Address

The `-S` option allows Nmap to specify a source IP address for crafted packets.

This can be useful when evaluating network filtering behavior in a controlled environment.

### Source Port

Some firewall configurations treat traffic differently depending on its source port.

For example, DNS commonly uses port 53.

The `--source-port` option allows Nmap to specify the source port used during certain scans.

A properly configured firewall should not trust traffic simply because it originates from a commonly used port.

These techniques demonstrate why network security should rely on carefully designed rules rather than assumptions about packet characteristics.

---

## 9. Why Are These Concepts Important?

Firewalls, IDS, and IPS are essential components of modern network security.

They help organizations control access, identify suspicious behavior, and prevent unauthorized activities.

However, security technologies must be properly configured and continuously monitored.

A poorly configured firewall may expose sensitive services.

An IDS may generate alerts without stopping an attack.

An IPS may block malicious traffic but also interrupt legitimate communication if its detection rules are inaccurate.

Cybersecurity professionals must understand these limitations to evaluate security controls effectively.

**Strong network security requires multiple layers of protection, accurate configuration, and continuous monitoring.**

---

# 10. Technical Vocabulary — English to Portuguese

| English | Portuguese |
|---|---|
| Firewall | Firewall |
| Intrusion Detection System | Sistema de Detecção de Intrusão |
| Intrusion Prevention System | Sistema de Prevenção de Intrusão |
| Network Traffic | Tráfego de rede |
| Incoming Traffic | Tráfego de entrada |
| Outgoing Traffic | Tráfego de saída |
| Unauthorized Access | Acesso não autorizado |
| Suspicious Activity | Atividade suspeita |
| Security Threat | Ameaça de segurança |
| Malicious Packet | Pacote malicioso |
| Drop a Packet | Descartar um pacote |
| Reject a Connection | Rejeitar uma conexão |
| Allow Traffic | Permitir tráfego |
| Block Traffic | Bloquear tráfego |
| Detection Rule | Regra de detecção |
| Security Alert | Alerta de segurança |
| Source IP Address | Endereço IP de origem |
| Destination Port | Porta de destino |
| Packet Filtering | Filtragem de pacotes |
| Network Enumeration | Enumeração de rede |
| Port Scanning | Varredura de portas |
| SYN Scan | Varredura SYN |
| ACK Scan | Varredura ACK |
| Packet Loss | Perda de pacotes |
| Timeout | Tempo limite de espera |
| Retry | Nova tentativa |
| Round-Trip Time | Tempo de ida e volta |
| Scanning Performance | Desempenho da varredura |
| Source Port | Porta de origem |
| Decoy Scanning | Varredura com endereços de distração |
| Firewall Evasion | Evasão de firewall |
| Security Assessment | Avaliação de segurança |
| Misconfiguration | Configuração incorreta |
| Legitimate Traffic | Tráfego legítimo |
| Attack Pattern | Padrão de ataque |

---

# 11. Useful Technical English Sentences

**1.** "The firewall blocked the incoming connection."

O firewall bloqueou a conexão de entrada.

**2.** "The IDS detected suspicious network activity."

O IDS detectou atividade suspeita na rede.

**3.** "The IPS automatically dropped the malicious packets."

O IPS descartou automaticamente os pacotes maliciosos.

**4.** "The port appears to be filtered by a firewall."

A porta parece estar sendo filtrada por um firewall.

**5.** "The server did not respond to the TCP SYN packet."

O servidor não respondeu ao pacote TCP SYN.

**6.** "Increasing the scanning speed may cause packet loss."

Aumentar a velocidade da varredura pode causar perda de pacotes.

**7.** "The ACK scan helps identify firewall filtering behavior."

A varredura ACK ajuda a identificar o comportamento de filtragem do firewall.

**8.** "The security administrator received an intrusion alert."

O administrador de segurança recebeu um alerta de intrusão.

**9.** "The firewall rules must be properly configured."

As regras do firewall devem ser configuradas corretamente.

**10.** "An intrusion prevention system can automatically block malicious traffic."

Um sistema de prevenção de intrusão pode bloquear automaticamente tráfego malicioso.

---

# 12. Reading Comprehension — Practice Questions

Answer the following questions in English.

**Question 1:** What is the main purpose of a firewall?

**Question 2:** What is the difference between an IDS and an IPS?

**Question 3:** What happens when a firewall drops a packet?

**Question 4:** Why is an ACK scan useful during a network security assessment?

**Question 5:** What is the difference between `-T3` and `-T4` in Nmap?

**Question 6:** Why can aggressive scanning produce incomplete results?

**Question 7:** What is the purpose of the `--min-rate` option?

**Question 8:** Why should security professionals understand firewall filtering behavior?

---

# 13. Final Review — Key Takeaways

- **Firewall:** Controls incoming and outgoing network traffic.
- **IDS:** Detects suspicious activity and generates alerts.
- **IPS:** Detects suspicious activity and can automatically block it.
- **DROP:** Silently discards packets.
- **REJECT:** Blocks traffic and returns an explicit response.
- **SYN Scan:** Helps identify open, closed, and filtered TCP ports.
- **ACK Scan:** Helps investigate firewall filtering behavior.
- **Nmap Timing:** Controls scanning speed and related timing parameters.
- **Packet Rate:** Determines how quickly Nmap attempts to send packets.
- **Firewall Evasion:** Examines how security controls respond to different traffic characteristics.

### Final Thought

Understanding network security is not only about learning how to use tools.

It is about understanding how networks communicate, how security systems detect threats, and how attackers may attempt to exploit weaknesses.

**A skilled cybersecurity professional understands both offensive techniques and defensive security mechanisms.**

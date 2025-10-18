# Full VoIP Stack on VMware ESXi (SIP/TLS, SRTP, WebRTC)

## Objective
Design a secure, scalable VoIP platform using **ESXi VMs**. SIP over **TLS**, media via **RTPengine** (SRTP), and WebRTC support.

## Architecture
![Architecture](voip_stack.png)

**Components**
- **Kamailio** – SIP proxy/registrar/LB (SBC role)
- **Asterisk (Call)** – Dialplan, IVR, routing
- **Asterisk (Voicemail)** – Storage, MWI, mailbox
- **RTPengine** – Media relay/transcoding, SRTP, NAT traversal
- **MariaDB/Postgres** – Subscribers, auth, CDR
- **Redis** – Pub/Sub events, rate limiting
- **pfSense/OPNsense** – Edge firewall/NAT, optional HAProxy

## ESXi Topology
- Port groups: **VOICE**, **DATA**, **MGMT**
- VLANs: VOICE=20, DATA=10, MGMT=99
- vNIC mapping per VM; MGMT isolated from WAN

## Ports
- SIP TLS **5061**, SIP int **5060**
- RTP/SRTP **10000–20000/UDP**
- WSS **443** (WebRTC)
- DB **3306/5432**
- Redis **6379**

## Security
- SIP over TLS, SRTP via RTPengine
- Kamailio anti-scan & rate limits, Fail2ban
- Least-privilege firewall rules per VLAN
- Central NTP/syslog, regular snapshots & DB backups

## Future Work (AI)
- **FastAPI** service for AI-assisted call routing/anomaly detection
- Scoring calls by quality (jitter/packet loss) for auto-reroute

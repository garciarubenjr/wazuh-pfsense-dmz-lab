# wazuh-pfsense-dmz-lab
pfSense + Wazuh SOC Lab (LAN / DMZ Segmentation)
# pfSense + Wazuh SOC Lab (LAN / DMZ Segmentation)

## Goal
Build a small SOC-style lab that:
- Segments LAN and DMZ networks with pfSense
- Sends pfSense syslog to a Wazuh manager
- Verifies visibility using packet capture + firewall logs
- Supports safe pentesting tests against a DMZ host (Metasploitable)

## Environment
- Host: Windows 11 + VMware Workstation
- pfSense CE 2.8.1 (WAN/LAN/DMZ)
- Wazuh (Manager + Web UI)
- Ubuntu Wazuh Agent (enrolled)
- Metasploitable (DMZ target)

## Network Design
- WAN: VMware NAT network (internet access for pfSense)
- LAN: Internal LAN segment for Wazuh + agents
- DMZ: 10.10.10.0/24 with Metasploitable

  <img width="466" height="467" alt="{495B228B-481A-4CAC-AD42-A254594C131C}" src="https://github.com/user-attachments/assets/f640e42f-1fda-4348-9dda-84ee4155a0e2" />


## What Works (Validated)
- pfSense routing between LAN and DMZ
- Ubuntu agent enrolled and reporting to Wazuh manager
- pfSense syslog forwarding to Wazuh over UDP/514 (validated with tcpdump)
- DMZ isolation: DMZ host can reach pfSense gateway but not internet by default
- Controlled access between DMZ and LAN via pfSense rules
- Firewall blocking confirmed via pfSense firewall logs

## Validation Evidence
See `docs/screenshots/` for:
- pfSense interfaces (WAN/LAN/DMZ)
- Firewall rules (LAN + DMZ)
- Remote syslog settings
- tcpdump confirming syslog arrival at Wazuh
- Wazuh agent status (ID 003 active)
- Metasploitable DMZ isolation tests and scan results

## Tests Performed
- DMZ -> pfSense gateway ping: PASS
- DMZ -> Internet ping: FAIL (expected)
- DMZ -> LAN target scan (nmap): FILTERED (expected)
- pfSense -> Wazuh syslog: PASS (tcpdump proof)

## Notes / Next Improvements
- Add custom parsing/decoding for pfSense firewall logs in Wazuh (pf decoder tuning)
- Optionally add a monitored Linux server in DMZ with Wazuh agent (more realistic than Metasploitable)
- Add detection rules for scan activity (nmap, brute force, web attacks)

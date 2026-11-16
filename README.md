# pfSense Firewall Homelab

A small VirtualBox lab I use to practise firewall administration, network troubleshooting and traffic filtering with pfSense, Kali Linux and Ubuntu.

## Lab overview

The lab has a pfSense firewall with a WAN connection and two internal interfaces. Ubuntu is used as an internal test machine, and Kali is used as an authorised test client. The Windows host has also been used for connectivity checks.

```text
                 VirtualBox NAT
                       |
                    pfSense
                  /         \
                LAN         OPT1
                 |            |
              Ubuntu         Kali
```

This is a logical view of the lab. The exact packet path depends on the VirtualBox adapter settings and the routes on each machine. Two systems on the same Layer-2 network may communicate directly without their traffic passing through pfSense.

## Lab devices

| Device | Role |
|---|---|
| pfSense WAN | Outside connection through VirtualBox NAT |
| pfSense LAN | Internal network for Ubuntu |
| pfSense OPT1 | Kali test network |
| Ubuntu | Internal test machine / HTTP test server |
| Kali Linux | Authorised test client |
| Windows host | Additional connectivity test source |

Private addresses are intentionally left out of this public repository. Placeholders such as `<KALI_IP>`, `<UBUNTU_IP>` and `<OPT1_GATEWAY>` are used in the notes.

## Progress so far

The status below separates what was observed in the lab from exercises that are documented but still need a complete test record.

| Day | Topic | Current status |
|---|---|---|
| 01 | Interface setup and basic connectivity | Initial setup completed. Kali reached the OPT1 gateway after a temporary pass rule was added; Kali and Ubuntu ping connectivity was reported. Windows-to-Kali connectivity is still unresolved, and screenshots/log evidence are pending. |
| 02 | PASS/BLOCK rules and rule order | Concepts and test procedure documented. A complete targeted rule-order test with client output and matching firewall logs is still pending. |
| 03 | TCP/UDP ports and HTTP testing | Test procedure documented. A successful HTTP baseline and matching TCP/80 rule/log evidence have not yet been recorded. |
| 04 | IP aliases | Configuration steps documented. An alias configured and validated in this pfSense lab has not yet been confirmed. |
| 05 | NAT and WAN port forwarding | A port-forward test was attempted. An HTTP 301 response and an Ubuntu request log were observed, but the external port, redirect port and Ubuntu listening port did not yet match clearly enough to call the mapping fully verified. |
| 06 | Firewall log analysis | Log analysis practical documented: reviewed Pass/Block events and correlated packet details with rules and client/server results. Redacted event screenshots and a representative evidence set are still pending. |
| 07 | pfBlockerNG DNSBL | pfBlockerNG DNSBL configured on pfSense with a custom `youtube.com` entry and Unbound. `nslookup` returned the DNSBL virtual IP `10.10.10.1`; browser-level blocking was not fully verified because the site could still open. |

## Documentation

- [Day 1 — Setup and connectivity](docs/day-01/README.md)
- [Day 1 — Network architecture](docs/day-01/network-architecture.md)
- [Day 2 — PASS/BLOCK rules and rule order](docs/day-02/README.md)
- [Day 3 — Protocol, port and HTTP testing](docs/day-03/README.md)
- [Day 4 — IP aliases](docs/day-04/README.md)
- [Day 5 — NAT and port forwarding](docs/day-05/README.md)
- [Day 6 — Firewall log analysis](docs/day-06/README.md)
- [Day 7 — pfBlockerNG DNSBL](docs/day-07/README.md)
- [Configuration register](docs/configuration/README.md)

## Useful pfSense pages

- Firewall rules: **Firewall → Rules**
- NAT / port forwarding: **Firewall → NAT → Port Forward**
- Aliases: **Firewall → Aliases**
- Firewall logs: **Status → System Logs → Firewall**
- Active connections: **Diagnostics → States**

## How I record a test

For each practical, I want to keep the configuration, the command used from the client, the actual output and the matching pfSense log together. Saving a rule is not enough to prove that the traffic behaved as expected. A timeout on its own is not proof that pfSense blocked a packet.

## Safety

This is an isolated learning lab. I do not expose the test services or pfSense administration interface to the public internet. Before publishing screenshots or configuration details, I remove private addresses, credentials, keys and other personal information.

## Next things to finish

1. Resolve the Windows-to-Kali connectivity issue and record the result.
2. Complete the Day 2 rule-order test with a matching firewall log.
3. Get a successful HTTP baseline, then test a specific TCP/80 Block and Pass rule.
4. Configure and validate the Day 4 IP alias in pfSense.
5. Confirm the exact external-to-internal port mapping for Day 5 and add the remaining evidence.
6. Add redacted firewall-log evidence and a representative correlated test record for Day 6.
7. Finish the Day 7 browser-level DNSBL test, including checks for DNS-over-HTTPS, cached records and alternate domains.

I will mark an exercise as verified only after the test result and the relevant evidence agree.

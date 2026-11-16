# Day 6 — Firewall log analysis

## Objective

Review pfSense firewall logs to understand which traffic is being passed or blocked and use the log details to troubleshoot connectivity.

## What I reviewed

I used the pfSense firewall log to follow traffic generated during the lab tests. The useful fields to inspect are:

- **Action:** Pass or Block
- **Interface:** where pfSense received the packet
- **Source:** the client address and source port
- **Destination:** the target address and destination port
- **Protocol:** for example, TCP, UDP or ICMP
- **Rule / tracker / description:** the policy associated with the event, when available
- **Timestamp:** when the event occurred

## Analysis workflow

1. Open **Status → System Logs → Firewall**.
2. Reproduce one known test from Kali or Ubuntu.
3. Filter or locate the event using the source address, destination address, protocol and time.
4. Check whether the action is Pass or Block and identify the interface where the traffic entered pfSense.
5. Compare the event with the relevant rule under **Firewall → Rules**.
6. If the result is unexpected, check routing, rule order, existing states and the destination service before changing the policy.

## Important observations

- A Block entry indicates that pfSense blocked a matching packet; check its interface and rule details before deciding which rule needs changing.
- A Pass entry shows that the firewall allowed that packet. It does not guarantee that the destination application responded successfully.
- ICMP ping logs cannot be used as proof that a TCP service such as HTTP on port 80 is working.
- A timeout by itself is not enough to identify pfSense as the cause. Correlate the client result with the firewall log and, where possible, the server-side log.
- Existing connection states can affect repeat tests. Inspect **Diagnostics → States** when a rule change does not appear to affect an existing connection.

## Current status

The log-analysis exercise has been carried out. I still need to add a redacted screenshot and one example event alongside the client test that generated it.


## Key lesson

Firewall logs help explain what pfSense did with a packet. Good troubleshooting correlates the log with the packet path, firewall rule, client output and destination service instead of relying on a timeout alone.

# Day 7 — pfBlockerNG DNSBL

## Objective

Configure pfBlockerNG DNSBL on pfSense and test whether a domain query is redirected to the DNSBL response address.

## What pfBlockerNG DNSBL does

DNSBL uses DNS responses to prevent clients using the configured DNS resolver from resolving selected domains normally. It is a DNS-layer control; it is not the same as blocking every possible connection to a website at the firewall layer.

## Lab work recorded

- pfBlockerNG was used on the **pfSense** firewall in this lab.
- DNSBL was enabled and a custom entry for `youtube.com` was added to the block list.
- Unbound DNS was used as the resolver.
- The DNSBL virtual IP used in the lab was `10.10.10.1`.
- A DNS lookup sent to the pfSense resolver at `192.168.20.1` returned the DNSBL virtual IP `10.10.10.1` for `youtube.com`.

These are lab-specific values. They are included here as the recorded test configuration; replace or redact them if the lab addressing changes before publishing screenshots.

## Verification performed

From a client, the lookup was tested with:

```bash
nslookup youtube.com 192.168.20.1
```

The observed DNS answer was `10.10.10.1`, the configured DNSBL virtual IP. This verifies that the DNS query was handled by the DNSBL policy for that test.

## Browser test limitation

The browser could still open YouTube during the test. Therefore, the DNS lookup result must not be described as proof that browser access was completely blocked. Possible reasons to investigate include browser DNS-over-HTTPS, cached DNS records, additional domains used by the site, or a client using a different DNS resolver.

## Follow-up checks

1. Confirm the client is using the pfSense Unbound resolver.
2. Check whether the browser has Secure DNS / DNS-over-HTTPS enabled and whether it bypasses the local resolver.
3. Clear the client and browser DNS caches before retesting.
4. Review the pfBlockerNG DNSBL log and Unbound configuration.
5. Identify any additional domains required for the intended test, then add only the necessary entries.
6. Retest with `nslookup` and a browser, recording the two outcomes separately.

## Security note

DNSBL is a useful DNS-layer control, but it should not be treated as a complete web-filtering or application-control solution. Clients that bypass the configured resolver may not be affected. Use it as one layer in a broader network policy.

## Key lesson

The DNS lookup showed that the configured DNSBL policy returned its virtual IP for the test domain. Browser-level blocking was not fully verified and remains a follow-up task.

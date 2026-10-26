# Pcap-Investigation

**Finding – Data hidden in DNS lookups (DNS tunnelling)**

1. **What I found:** Partway through normal browsing, the machine 10.0.2.15 suddenly made 40 DNS lookups in about 13 seconds. Each one asked for a different, random-looking name under the domain `sync-update-cdn.info`.
2. **Evidence:** Frames 87–177, from 10.0.2.15 to the DNS server 10.0.2.3 (UDP port 53). Example: `b72017d8484017b51497070cf388.14dc7fa6ea53.sync-update-cdn.info`. Every reply returned 10.10.10.10. _(Screenshot: filter `dns.qry.name contains "sync-update-cdn"`.)_
3. **Why it’s suspicious:** Normal lookups repeat the same real sites and get public IP addresses back. These names are long strings of hex, every one is unique, and they all point to one private address. That means the lookups aren’t being used to find a website at all. The domain name is made to sound like a harmless software update service so it blends in.
4. **Attacker’s likely goal:** Malware on 10.0.2.15 is chopping stolen data into small pieces and hiding them in DNS names, so the attacker’s DNS server can collect them. This is called **DNS tunnelling** or **DNS exfiltration**. Firewalls usually allow DNS out, so it slips past controls that would block a direct upload.
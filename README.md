# Pcap-Investigation

**Finding 2 – Data hidden in DNS lookups (DNS tunnelling)**

1. **What I found:** Partway through normal browsing, the machine 10.0.2.15 suddenly made 40 DNS lookups in about 13 seconds. Each one asked for a different, random-looking name under the domain `sync-update-cdn.info`.
2. **Evidence:** Frames 87–177, from 10.0.2.15 to the DNS server 10.0.2.3 (UDP port 53). Example: `b72017d8484017b51497070cf388.14dc7fa6ea53.sync-update-cdn.info`. Every reply returned 10.10.10.10. _(Screenshot: filter `dns.qry.name contains "sync-update-cdn"`.)_
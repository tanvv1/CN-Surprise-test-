# BGP Routing Lab – Completion Writeup

This document records the completion of the two-AS eBGP lab (AS 100 / AS 200) in Cisco Packet Tracer, following the topology and configuration steps from the BGP tutorial.

---

## 1. Topology

Two routers, each representing a separate organization, connected by a single eBGP serial link. Each router has its own switch and two PCs.

![Topology](images/Screenshot_2026-09-24_123341.png)

- **Router0** — AS 100 — LAN `172.16.1.0/24`
- **Router1** — AS 200 — LAN `172.16.2.0/24`
- eBGP link — `203.0.113.0/30`

---

## 2. PC IP Configuration

**PC0** (AS 100 LAN)

![PC0 IP config](images/Screenshot_2026-09-24_123935.png)

**PC1** (AS 100 LAN)

![PC1 IP config](images/Screenshot_2026-09-24_123941.png)

**PC2** (AS 200 LAN)

![PC2 IP config](images/Screenshot_2026-09-24_123947.png)

**PC3** (AS 200 LAN)

![PC3 IP config](images/Screenshot_2026-09-24_123953.png)

---

## 3. Router1 (AS 200) Initial Boot and Hostname

Router1 boots with no initial configuration dialog, then is given its hostname and FastEthernet address.

![Router1 initial boot and hostname/fa0/0 config](images/Screenshot_2026-09-24_124331.png)

---

## 4. Router1 Serial Interface and BGP Configuration

The serial interface is brought up, then the BGP process is started for AS 200, the eBGP neighbor in AS 100 is defined, and the local LAN is advertised into BGP.

![Router1 serial interface config](images/Screenshot_2026-09-24_124359.png)

![Router1 BGP neighbor and network statement](images/Screenshot_2026-09-24_124556.png)

The adjacency comes up immediately after the `neighbor` command:

```
%BGP-5-ADJCHANGE: neighbor 203.0.113.1 Up
```

---

## 5. Verification — BGP Neighbor State

`show ip bgp summary` confirms the eBGP peering with Router0 (`203.0.113.1`, AS 100) is established, with 4 prefixes received.

![show ip bgp summary](images/Screenshot_2026-09-24_124604.png)

---

## 6. Verification — Routing Table

`show ip route` on Router1 shows `172.16.1.0/24` learned as a **B** (BGP) route via `203.0.113.1`, alongside the directly connected `172.16.2.0/24` and `203.0.113.0/30` networks.

![show ip route bgp - part 1](images/Screenshot_2026-09-24_124611.png)

![show ip route bgp - part 2](images/Screenshot_2026-09-24_124641.png)

---

## 7. End-to-End Connectivity Test

**PC0 (172.16.1.10) → PC2/PC3 LAN (172.16.2.10):** one packet lost on the first attempt (ARP resolution), then successful replies.

![PC0 ping to 172.16.2.10](images/Screenshot_2026-09-24_124650.png)

**PC2 (172.16.2.10) → PC0/PC1 LAN (172.16.1.10):** 100% success, 0% loss.

![PC2 ping to 172.16.1.10](images/Screenshot_2026-09-24_124650.png)

---

## Result

The eBGP peering between AS 100 and AS 200 came up successfully, both LAN prefixes were exchanged via BGP, and PCs on either side of the two autonomous systems could reach each other end to end — confirming the lab was completed correctly.

# Open Source SOC — Concept & Architecture

![Overall SOC topology](assets/Solution%20Layout-black.png)

> ⚠️ **This is not a step-by-step tutorial.** This repository documents the *concept* and *logical architecture* of a fully functional Security Operations Center built entirely with open-source tools. The goal is to show what's possible to build, and the reasoning behind each decision — not to hand you a script to copy-paste. For implementation, go straight to each tool's official docs.

---

## 1. About

A SOC (Security Operations Center) is usually framed as something only enterprises with six-figure budgets can afford. This project is proof that's not entirely true.

Every component here — detection, correlation, response, and threat intelligence — is open-source, self-hosted, and integrated end-to-end. No commercial licenses, no vendor lock-in. Just a deliberate architecture built on tools the security community already trusts.

This repo isn't "here's how to install X." It's "here's how the pieces think together" — so you can build your own version, adapted to your context.

**To be upfront about intent:** this isn't pitched as an enterprise-ready deployment, and it's not meant to be dropped into a company as-is. The goal is educational — to understand, end to end, how a SOC *should* think and function, so the concepts aren't a black box the day you actually work with a real environment, open source or commercial.

### 1.1 What this architecture actually does

| Capability | Concept |
|---|---|
| **Proactive Detection** | Identify threats and anomalous behavior as they happen, not after |
| **Centralized Visibility** | One place to see everything, instead of ten dashboards |
| **Structured Response** | Incidents follow a repeatable process, not tribal knowledge |
| **Forensic Analysis** | Every case leaves a trail you can investigate later |
| **Threat Intelligence** | The SOC gets smarter with every incident it handles |
| **Low Operational Cost** | Zero licensing — the only cost is your time and infrastructure |
| **Native Integration** | Tools talk to each other automatically, not through manual exports |

---

## 2. The Big Picture — Thinking in Layers, Not Boxes

The mistake most people make when designing a SOC is thinking in terms of *products* first. The right way to think about it is in terms of **logical functions** — then you pick the tool that fills each function.

There are three logical planes here:

1. **Visibility Plane** — collects raw signal from endpoints. If you can't see it, you can't detect it.
2. **Enforcement Plane** — this one isn't a single box. Traffic actually crosses three checkpoints before it ever reaches a workstation or server: the **Border Firewall** (automated inspection — IPS engines and antivirus), the **Internal Firewall** (manual allow/deny policy — the calls that need a human, not a signature), and the **L3 Switch** (the device that actually enforces segmentation between zones). Only what survives all three gets through.
3. **Intelligence & Response Plane** — correlates what's left, turns it into a case, and feeds what's learned back into the system.

![Logical layers: Visibility, Enforcement, Intelligence & Response](/assets/Solution%20Architecture-black.png)

The key idea: **data gets progressively refined and challenged as it moves through each checkpoint.** Raw logs become filtered events, filtered events become correlated alerts, and correlated alerts become documented cases with shareable intelligence. Nothing gets to an analyst's screen without already being processed by multiple layers first.

### 2.1 Component Map

| Layer | Component | Logical Role |
|---|---|---|
| Visibility | Windows Defender | Native OS-level malware detection on the endpoint |
| Visibility | Sysmon | Deep visibility into Windows process/network behavior |
| Visibility | Wazuh Agent | Ships endpoint logs and telemetry to the SIEM |
| Enforcement — Border | OPNsense | Perimeter firewall and router |
| Enforcement — Border | Suricata (IPS) | Inspects packets against known attack signatures |
| Enforcement — Border | CrowdSec (IPS) | Detects behavioral attack patterns, blocks malicious IPs |
| Enforcement — Border | ClamAV | Catches known malware before it spreads |
| Enforcement | Internal Firewall | Manual inbound/outbound allow-deny policy |
| Enforcement | L3 Switch | Routes and segments traffic between VLANs |
| Intelligence & Response | Wazuh SIEM | Correlates everything, decides what's an alert |
| Intelligence & Response | DFIR-IRIS | Turns alerts into structured investigations |
| Intelligence & Response | MISP | Stores and shares what was learned |
| Resilience | Google Drive Backup | Off-site backup for both endpoints and SOC data (see note in section 3) |

---

## 3. Why Each Tool Earns Its Place

Instead of a feature dump, here's the *reasoning* behind each choice:

**OPNsense** exists because segmentation is the first line of defense — a flat network means one compromised device can see everything else.

**CrowdSec and Suricata run on the same border firewall on purpose — they're not redundant, they cover two different detection philosophies.** Suricata is signature-based: it looks for packet patterns that match known exploits, so it's fast and precise against threats that are already cataloged. CrowdSec is behavior-based: it doesn't need to recognize the exact attack, it just needs to notice a host doing something abnormal — repeated failed logins, scanning patterns — which is exactly what catches the things Suricata's signature list hasn't seen yet. Running only one of them means choosing between "fast against known threats" and "adaptive against new ones." Running both means not having to choose.

**ClamAV** is the last perimeter gate — cheap to run, catches known malware before it ever reaches an endpoint.

**The Internal Firewall is a second, deliberate checkpoint after the border firewall — not a duplicate of it.** The border firewall's IPS engines make automated decisions at machine speed, based on signatures and behavior patterns. The internal firewall is where a human's own allow/deny policy applies — the traffic decisions that need judgment, not just pattern matching.

**The L3 Switch is what actually makes segmentation real, not just documented.** A firewall rule that says "these zones shouldn't talk to each other" is a policy; the L3 switch is the device enforcing it packet by packet. Without it, segmentation is a diagram — with it, it's a property of the network itself.

**Wazuh Agent, Sysmon, and Windows Defender** exist because detection at the endpoint is non-negotiable. The perimeter can miss things; the endpoint is where the truth usually lives.

**Wazuh SIEM** is the brain — it's the only place with enough context to say "these five separate events are actually one attack."

**DFIR-IRIS** exists because an alert without a documented investigation is just noise nobody learns from.

**MISP** exists because a SOC that doesn't share or consume threat intelligence is reinventing the wheel with every incident.

**Google Drive Backup** was chosen specifically because it's free — for a lab or a small-scale deployment, that's a meaningful advantage, even with its limitations (storage caps, no enterprise-grade retention controls, no immutability guarantees). In a real production environment, this is the one piece of the architecture you'd want to swap out first — for dedicated backup infrastructure with offline or immutable copies. It's a reasonable starting point, not an end state.

---

## 4. How an Incident Actually Flows

Data doesn't jump straight to a workstation — it earns its way there. Here's the actual path, in both directions:

**Inbound (network → workstation):**

```
① Traffic enters through the internet link
        ↓
② Border Firewall inspects it (Suricata + CrowdSec as IPS, ClamAV as antivirus)
        ↓
     Malicious? ──→ blocked right here, never goes further
        ↓ (clean)
③ Internal Firewall applies manual allow/deny policy
        ↓
④ L3 Switch routes it into the correct, segmented VLAN
        ↓
⑤ Traffic finally reaches the workstation
```

**If something still activates locally (workstation → response):**

```
⑥ Malicious activity is caught on the endpoint (Windows Defender or Sysmon)
        ↓
⑦ An alert is sent to Wazuh SIEM and classified
        ↓
⑧ An analyst opens an investigation in DFIR-IRIS and takes the necessary action
        ↓
⑨ Once the case is closed, indicators are exported to MISP
        ↓
⑩ That intelligence enriches the company's own future detection —
   so the next occurrence of the same threat is caught faster, with more context
```

This is the part most architectures get wrong: **the loop has to close.** A SOC that detects, responds, and never feeds intelligence back is just doing the same work over and over with no compounding value. The whole point of exporting to MISP after closing a case isn't paperwork — it's making sure the *next* analyst (or the *same* one, six months from now) inherits the answer instead of starting from zero.

---

## 5. A Concrete Walkthrough — Following One Attack Through the Whole System

Diagrams are useful, but nothing makes an architecture click like tracing a real scenario through it end to end. Here's a phishing attempt, followed layer by layer:

**00:00** — An employee opens an email attachment. It quietly drops a small executable and tries to establish a connection to an external command-and-control server.

**00:01** — **Sysmon**, running on that workstation, logs the process creation and the outbound network connection as soon as they happen. **Wazuh Agent** ships that log off the machine in near real time.

**00:02** — The outbound connection also crosses the **Border Firewall** (OPNsense). Suricata flags the destination against a known-bad signature; CrowdSec independently notices the same host is now behaving very differently from its baseline. Neither the Internal Firewall's manual rules nor the L3 Switch's segmentation were built to catch this — this kind of anomaly is exactly what the IPS layer exists for.

**00:03** — **Wazuh SIEM** receives all three signals — the endpoint log, the Suricata alert, and the CrowdSec flag — almost simultaneously. Individually, none of them would be conclusive. Correlated together, in a short time window, on the same host, they cross the threshold for a real alert.

**00:04** — That alert automatically opens a case in **DFIR-IRIS** via webhook, pre-filled with the host, the timeline, and the raw evidence. No analyst had to manually connect the dots — they start already looking at a structured case, not a wall of logs.

**00:15** — An analyst confirms it's malicious, documents the investigation in DFIR-IRIS, and isolates the host.

**00:20** — Once closed, the indicators from the case — the malicious domain, the file hash — are exported to **MISP**.

**Next time** — if that same domain or file hash shows up anywhere else in the environment, Wazuh already knows to flag it immediately, because it's now enriched with what MISP learned from this exact incident.

This is the entire point of the architecture: **no single tool "catches" the attack alone.** It's the layered correlation — endpoint, perimeter, and SIEM agreeing with each other — that turns a quiet, easy-to-miss event into a fast, confident, documented response.

---

## 6. Segmentation as a Design Principle, Not a Network Diagram

The topology isn't about which switch model or how much RAM a server has — it's about **isolating blast radius**. A few logical decisions drive the whole design:

| Design Decision | Why It Matters |
|---|---|
| Dedicated segment for SOC tooling | If a workstation is compromised, it should never have a direct path to the SIEM or case management system |
| Separate segments per business function (e.g. HR, IT) | Lateral movement between departments is one of the most common ways small incidents become big ones |
| Resilient internet connectivity | Detection and response are useless if the SOC itself goes offline during an incident |
| Segmentation enforced at the L3 Switch | Policy on paper isn't enough — the switch is what actually keeps zones apart, packet by packet |
| Off-site backup (Google Drive, in this build) | Ransomware scenarios assume local backups are also compromised. Google Drive was chosen here for being free — a fine starting point for a lab, but a real production environment should move to dedicated, immutable backup infrastructure |

> 🖼️ *See the topology diagram at the top of this document — the segmentation logic above is what it's meant to show, independent of any hardware detail.*

---

## 7. The Concepts You Actually Need to Build This

If you want to build your own version of this, the tools matter far less than understanding these concepts first:

- **Networking fundamentals** — VLANs, routing between segments, firewall rule logic
- **Log management** — what's worth collecting vs. what's just noise
- **Correlation logic** — how a SIEM turns ten unrelated events into one meaningful alert
- **Incident response workflow** — case lifecycle, evidence handling, documentation discipline
- **Threat intelligence basics** — what an IOC is, how sharing/taxonomies work
- **Automation via APIs/webhooks** — this is what makes tools act as *one system* instead of five separate dashboards

Once these concepts click, the specific tool choices become almost interchangeable — you could swap Wazuh for another SIEM, or MISP for another TI platform, and the architecture still holds.

---

## 8. Official Tools & Docs

| Category | Tool | Link |
|---|---|---|
| Firewall/Router | OPNsense | https://opnsense.org |
| Behavioral Detection | CrowdSec | https://www.crowdsec.net |
| IDS/IPS | Suricata | https://suricata.io |
| Antivirus | ClamAV | https://www.clamav.net |
| SIEM/XDR | Wazuh | https://wazuh.com |
| Endpoint Logging | Sysmon | https://learn.microsoft.com/pt-br/sysinternals/downloads/sysmon |
| Incident Response | DFIR-IRIS | https://dfir-iris.org |
| Threat Intelligence | MISP | https://www.misp-project.org |

---

## 9. What This Architecture Enables

| Scenario | Layers Involved |
|---|---|
| Malware investigation | Wazuh + ClamAV + DFIR-IRIS + MISP |
| Phishing response | Wazuh + DFIR-IRIS + MISP |
| Data exfiltration investigation | Wazuh + Suricata + DFIR-IRIS |
| Insider threat analysis | Wazuh + Sysmon + DFIR-IRIS |

**Metrics that matter here:** MTTD (Mean Time to Detect), MTTR (Mean Time to Respond), false positive rate, and endpoint coverage. A SOC's maturity is measured by how these trend over time — not by how many tools it runs.

---

## 10. Final Notes

This project is a living reference for learning, not a finished product or a deployment blueprint. If it helps you understand how a SOC actually works, well enough to design your own — for a lab, a study project, or as groundwork before touching a real environment — that's the goal. If you use it in academic or personal study work, a credit is appreciated.

Built with ❤️ for the cybersecurity community.

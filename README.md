<h1 align="center">🛡️ Home SOC Detection Lab</h1>
<h3 align="center">Building a Blue-Team Lab to Simulate, Detect, and Document Real-World Attack Techniques</h3>

<p align="center">
  <img src="https://img.shields.io/badge/Wazuh-3F6EC2?style=for-the-badge&logo=wazuh&logoColor=white" />
  <img src="https://img.shields.io/badge/Sysmon-0078D6?style=for-the-badge&logo=windows&logoColor=white" />
  <img src="https://img.shields.io/badge/Atomic%20Red%20Team-D32029?style=for-the-badge&logo=redhat&logoColor=white" />
  <img src="https://img.shields.io/badge/MITRE%20ATT%26CK-BB1F36?style=for-the-badge&logo=mitre&logoColor=white" />
  <img src="https://img.shields.io/badge/Linux-FCC624?style=for-the-badge&logo=linux&logoColor=black" />
  <img src="https://img.shields.io/badge/License-MIT-2ea44f?style=for-the-badge" />
</p>

---

## 📌 Overview

This repository documents the design, build, and operation of a **home Security Operations Center (SOC) detection lab**. The lab pairs a Wazuh SIEM with Sysmon-instrumented Windows and Linux endpoints, then uses **Atomic Red Team** to safely execute MITRE ATT&CK techniques against those endpoints so detection rules and analyst workflows can be validated end-to-end.

The goal is to practice the full detection-engineering loop: **simulate → collect telemetry → detect → investigate → document → tune**.

---

## 🎯 Objectives

- Simulate real adversary behavior with Atomic Red Team test plans
- Centralize Windows + Linux telemetry into Wazuh for analysis
- Author and tune custom detection rules mapped to MITRE ATT&CK
- Produce analyst-grade incident reports and evidence artifacts
- Build muscle for SOC analyst, detection engineer, and IR roles

---

## 🧱 Lab Architecture

```
  +-------------------+        +-------------------+
  | Windows Endpoint  |        |  Linux Endpoint   |
  |  + Sysmon         |        |  + auditd / syslog|
  |  + Wazuh Agent    |        |  + Wazuh Agent    |
  +---------+---------+        +---------+---------+
            |                            |
            |   Telemetry (events,       |
            |   process, network, file)  |
            +-------------+--------------+
                          |
                          v
                +---------+----------+
                |   Wazuh Manager    |
                |  (SIEM + rules)    |
                +---------+----------+
                          |
                          v
               Detections · Alerts · Dashboards
```

**Attack Simulation Layer:** Atomic Red Team test cases executed on the endpoints to generate the malicious telemetry the rules must catch.

---

## 🛠️ Tech Stack

| Layer | Tooling |
|---|---|
| SIEM / Manager | **Wazuh** |
| Windows Telemetry | **Sysmon**, Windows Event Logs |
| Linux Telemetry | **auditd**, syslog, Wazuh agent |
| Adversary Emulation | **Atomic Red Team** (Invoke-AtomicTest) |
| Threat Model | **MITRE ATT&CK** Enterprise matrix |
| Documentation | Markdown, incident reports, lessons learned |

---

## 📁 Repository Structure

```
home-soc-detection-lab/
├── setup/             # Deployment notes for Wazuh, Sysmon, agents, endpoints
├── detections/        # Custom Wazuh rules, decoders, and ATT&CK mappings
├── logs/              # Sample telemetry from simulated attacks
├── evidence/          # Incident investigation artifacts and reports
├── screenshots/       # Dashboards, alerts, and walkthrough captures
├── lessons-learned.md # Reflections and detection-engineering takeaways
├── README.md
└── LICENSE
```

---

## 🔬 Detection Workflow

1. **Plan** — pick a MITRE ATT&CK technique (e.g., T1059.001 — PowerShell)
2. **Simulate** — run the matching Atomic Red Team test on the endpoint
3. **Collect** — capture Sysmon / auditd / Wazuh agent telemetry
4. **Detect** — author or tune a Wazuh rule that fires on the activity
5. **Investigate** — triage the alert, pivot through logs, build a timeline
6. **Document** — write the incident note and lessons-learned entry
7. **Tune** — reduce false positives, broaden coverage, re-test

---

## 🧪 Example MITRE ATT&CK Coverage

| Tactic | Technique | Status |
|---|---|---|
| Execution | T1059.001 — PowerShell | 🔍 In progress |
| Persistence | T1547.001 — Registry Run Keys | 🔍 In progress |
| Defense Evasion | T1070.004 — File Deletion | 🔍 In progress |
| Credential Access | T1003 — OS Credential Dumping | 🔍 In progress |
| Discovery | T1082 — System Information Discovery | 🔍 In progress |

> Coverage will expand as new detections are committed under `detections/`.

---

## 🚀 Getting Started

> The lab is intended to run in an **isolated virtual environment** (e.g., VirtualBox / VMware / Proxmox). Do **not** run Atomic Red Team payloads against production systems.

1. Provision a Wazuh Manager VM — see `setup/` for notes.
2. Deploy Wazuh agents on a Windows VM and a Linux VM.
3. Install **Sysmon** on Windows with a tuned configuration.
4. Install **Atomic Red Team** on the test endpoints.
5. Execute selected Atomic tests and review alerts in the Wazuh dashboard.
6. Iterate on detections in `detections/` and document findings in `evidence/`.

---

## 📓 Lessons Learned

Ongoing reflections — detection gaps, tuning trade-offs, false-positive analysis, and analyst-workflow takeaways — are tracked in [`lessons-learned.md`](./lessons-learned.md).

---

## ⚠️ Safety & Ethics

This lab is for **defensive research and education**. All adversary emulation runs on isolated VMs that I own. Atomic Red Team tests can produce real malicious-looking behavior — only execute them in environments where you have explicit authorization.

---

## 👤 Author

**Shalem Raju Maddirala** · M.S. Digital Forensics and Cybersecurity, University at Albany, SUNY

[Portfolio](https://shalem.site) · [GitHub](https://github.com/Shalem2020)

---

## 📄 License

Released under the [MIT License](./LICENSE).

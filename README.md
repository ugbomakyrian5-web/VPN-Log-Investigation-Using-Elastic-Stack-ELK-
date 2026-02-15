# 🔎 SOC Lab Project: VPN Log Investigation & Threat Hunting Using Elastic Stack (ELK)

---

## 🧪 Lab Overview

This lab demonstrates practical Security Operations Center (SOC) investigation using the Elastic Stack (Elasticsearch, Logstash, Kibana) for log analysis and threat hunting.

While ELK is not a traditional SIEM, it is widely deployed in SOC environments for log aggregation, anomaly identification, and dashboard-driven monitoring.

The objective of this investigation was to analyze VPN connection logs, identify anomalous behavior, and surface potential security control gaps.

---

## 🎯 Objectives

- Analyze VPN authentication logs for abnormal activity  
- Identify high-frequency source IP addresses  
- Detect anomalous user behavior  
- Investigate post-termination account activity  
- Build operational dashboards for monitoring  

---

## 🏗 Platform Architecture

| Component     | Function                        | SOC Impact                              |
|---------------|---------------------------------|----------------------------------------|
| Elasticsearch | Log storage and indexing        | Enables fast search and correlation    |
| Logstash      | Log parsing and normalization   | Structured field extraction             |
| Kibana        | Analyst interface               | Search, filtering, visualization, and investigation |

All analysis was performed in Kibana using indexed VPN logs.

---

## 📂 Dataset Scope

- **Index Pattern:** `vpn_connections`  
- **Time Range:** 31 December 2021 – 2 February 2022  
- **Total Events:** 2861 VPN connection logs

---

## 🌐 Source IP Analysis

### Highest Frequency Source IP
`238.163.231.224`  
This IP generated the largest volume of VPN connections during the observation period.

### Geographic Refinement
Connections from `238.163.231.224` excluding New York state: **48 connections**  

Repeated connections outside expected regions may indicate automated attempts or credential misuse.

---

## 👤 User Activity Analysis

### Highest Traffic User
`James`  

Traffic reviewed for operational consistency; no anomalous deviation detected beyond thresholds.

---

## 🔎 Targeted Account Investigation

Applied filter: `UserName: Emanda`  

### Highest Associated Source IP
`107.14.1.247`  

Consistent access from a single external IP warrants monitoring and correlation with authentication logs.

---

## 📈 Timeline Anomaly Detection

Observed spike in VPN connections on **11 January 2022**  

### Responsible IP
`172.201.60.191`  

Spikes in connection volume may indicate scripted or automated login attempts.

---

## 🔍 Advanced Query Investigation (KQL)

### Multi-Condition Query
```
Source_Country: "United States" AND (UserName: James OR UserName: Albert)
```
**Records Returned:** 161

---

### Post-Termination Account Validation

User **Johny Brown** was terminated on 1 January 2022.  

Query:
```
UserName: "Johny Brown" AND @timestamp > "2022-01-01"
```
**Post-Termination VPN Connections:** 1  

Indicates a potential gap in deprovisioning controls.

---

## 📊 Visualization & Dashboard Development

Created a custom VPN Security Dashboard including:

- Top Source IPs (Bar Chart)  
- VPN Connections Over Time (Time Series)  
- Connections by Country (Pie Chart)  
- User Activity Table  

The dashboard consolidates high-risk indicators and improves monitoring efficiency.

---

## 🧠 MITRE ATT&CK Mapping

| Tactic               | Technique                   | ID    |
|----------------------|----------------------------|-------|
| Initial Access       | Valid Accounts             | T1078 |
| Credential Access    | Brute Force (Potential)    | T1110 |
| Discovery            | Account Discovery           | T1087 |

---

## 📸 Optional Evidence

### VPN Connections Overview
![VPN Connections Overview](https://i.imgur.com/IA6KyVh.png)

### Source IP Analysis
![Source IP Analysis](https://i.imgur.com/ciEliQo.png)

### High-Traffic User Analysis
![High-Traffic User Analysis](https://i.imgur.com/1f7hgrK.png)

### Filtered User Johny Brown Source IP
![Filtered User Johny Brown Source IP](https://i.imgur.com/wjofMO2.png)

### Timeline Spike on 11th Jan
![Timeline Spike on 11th Jan](https://i.imgur.com/6KtO70N.png)

### Post-Termination VPN Activity
![Post-Termination VPN Activity](https://i.imgur.com/vIEYJMV.png)

### KQL Search Query Results
![KQL Search Query Results](https://i.imgur.com/AS2pj5C.png)

### VPN Dashboard Visualization
![VPN Dashboard Visualization](https://i.imgur.com/vMsUigU.png)

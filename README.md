# SIEM-Sentinel- &nbsp;&nbsp;<img width="300" height="198" alt="Microsoft_Sentinel_logo1" src="https://github.com/user-attachments/assets/0330bec9-89fc-473d-b656-62bd751ce1bf" />

Deployed a Microsoft Sentinel environment to emulate enterprise-grade SIEM ingestion and monitoring workflows. A dedicated Log Analytics workspace (Cyber-Sentinel-Workspace) was provisioned within the SOC-Project-RG resource group. The workspace was then onboarded to Sentinel, enabling centralized log collection and analysis.

**Workspace and Sentinel Deployment**

A dedicated Log Analytics workspace (Cyber-Sentinel-Workspace) was created within the SOC-Project-RG resource group and successfully onboarded to Microsoft Sentinel, establishing the foundation for centralized log collection and analysis (Figures 1–3, 5).

This deployment enabled:
- Centralized log ingestion
- KQL-based querying
- Custom table creation for non-native data sources

  <img width="1362" height="669" alt="1" src="https://github.com/user-attachments/assets/baadb8bf-cc5c-437d-94f7-8bef5ef30d7a" />
  
  *Figure 1-1: Dashboard Alerts*
  
  <img width="1908" height="685" alt="2" src="https://github.com/user-attachments/assets/e517df6a-75dc-4bf4-95b7-fcfd18dfc4ab" />

  *Figure 2-2: Dashboard Alerts*

   <img width="1317" height="431" alt="3" src="https://github.com/user-attachments/assets/75e9c92c-a456-40dd-a42c-b69ace2e1654" />

  *Figure 3-3: Dashboard Alerts*

  <img width="1917" height="681" alt="5" src="https://github.com/user-attachments/assets/af742b46-46dc-4f04-9a1f-aa769c2e3263" />

  *Figure 4-5: Dashboard Alerts*

**Custom Log Ingestion Architecture**

To simulate ingestion of external telemetry, a full Azure Monitor ingestion pipeline was configured, including:
- Data Collection Endpoint (DCE) (SOC-DCE)
- Data Collection Rule (DCR) (DCR-Cisco-Upload)
- Custom Tables for structured log storage
  
This architecture reflects a real-world ingestion pipeline used to onboard third-party data sources into Sentinel (Figures 8–10, 18).

<img width="1324" height="684" alt="8 creating data collection end point" src="https://github.com/user-attachments/assets/bfdbec03-e651-45ca-b9ef-d7a6f44b8863" />

*Figure 5-8: Dashboard Alerts*

<img width="852" height="886" alt="9" src="https://github.com/user-attachments/assets/c5720b35-daea-4f1d-9784-331c3611a32e" />

*Figure 6-9: Dashboard Alerts*

<img width="989" height="898" alt="10" src="https://github.com/user-attachments/assets/5e66becd-afc0-4e7f-a69b-6d4e565cbe1b" />

*Figure 7-10: Dashboard Alerts*

The resource group confirms successful deployment of all required components:
- Log Analytics Workspace
- Microsoft Sentinel solution
- Data Collection Rule
- Data Collection Endpoint (Figure 8)

<img width="1919" height="590" alt="18 both logs ingested Resource Group" src="https://github.com/user-attachments/assets/05d77b5c-5601-4733-8738-bfca75be5928" />

*Figure 8-18: Resource Group Deployments*

**Cisco Stealthwatch Integration (CiscoStealthwatch_CL)**

A custom table (CiscoStealthwatch_CL) was created to simulate ingestion of Cisco Stealthwatch network telemetry (Figures 7, 10, 14).

<img width="1886" height="895" alt="7" src="https://github.com/user-attachments/assets/e5effa28-5ac9-45c0-91ec-9700d5c0d725" />

*Figure 9 - 7: Dashboard Alerts*

<img width="623" height="475" alt="14" src="https://github.com/user-attachments/assets/eb725d96-c99e-4a33-b68e-e71c1ace8bce" />

*Figure 10-14: Dashboard Alerts*

During ingestion configuration:

- A JSON sample file (CiscoStealthwatch.json) was uploaded to define schema

- A transformation rule was implemented to normalize timestamps:

```kusto
source
| extend TimeGenerated = todatetime(EventTime)
```
(Figures 11)

<img width="1878" height="894" alt="12" src="https://github.com/user-attachments/assets/b751d8ff-ec00-40f7-adbe-4118c5aeded8" />

*Figure 11-12: Normalizing timestamps*

Initial ingestion errors highlighted a missing TimeGenerated field. This was resolved by mapping the existing EventTime field to TimeGenerated, ensuring compatibility with Sentinel’s time-based query engine (Figures 12).

<img width="1917" height="434" alt="13 time repaired" src="https://github.com/user-attachments/assets/811fee7b-d305-40b6-a726-acd416c7f4cd" />

*Figure 12 - 13:  Sentinel’s time-based query resolved*

Final schema validation using KQL confirmed the table structure was correctly defined, including fields such as: 

- EventTime
- HostIP / HostName
- SeverityLevel
- SyslogMessage
- ProcessName (Figure13)
  
<img width="1919" height="815" alt="21 prrof" src="https://github.com/user-attachments/assets/a9b54921-dca6-4718-b95d-15361cdf47ee" />

*Figure 13 -21:  Validating schema using KQL*

**Windows Security Event Integration (WindowsRegistry_CL)**
A second custom table was created to simulate ingestion of Windows Security Event logs (Event ID 4663), representing file and registry access activity.

During ingestion:

- A JSON dataset was uploaded
  
-Sentinel generated a warning indicating no timestamp field was present (Figure 16)

To resolve this:

The timestamp field (TimeGenerated_UTC) was normalized using transformation logic:

```kusto
source
| extend TimeGenerated = todatetime(TimeGenerated_UTC)
| project-away TimeGenerated_UTC
```


*Figure 7: Dashboard Alerts*

*Figure 8: Dashboard Alerts*

*Figure 9: Dashboard Alerts*

*Figure 10: Dashboard Alerts*

*Figure 11: Dashboard Alerts*

*Figure 12: Dashboard Alerts*

*Figure 13: Dashboard Alerts*

*Figure 14: Dashboard Alerts*

# SIEM-Sentinel- &nbsp;&nbsp;<img width="300" height="198" alt="Microsoft_Sentinel_logo1" src="https://github.com/user-attachments/assets/0330bec9-89fc-473d-b656-62bd751ce1bf" />

Deployed a Microsoft Sentinel environment to emulate enterprise-grade SIEM ingestion and monitoring workflows. A dedicated Log Analytics workspace (Cyber-Sentinel-Workspace) was provisioned within the SOC-Project-RG resource group. The workspace was then onboarded to Sentinel, enabling centralized log collection and analysis.

**Workspace and Sentinel Deployment**

A dedicated Log Analytics workspace (Cyber-Sentinel-Workspace) was created within the SOC-Project-RG resource group and successfully onboarded to Microsoft Sentinel, establishing the foundation for centralized log collection and analysis (Figures 1–3).

This deployment enabled:
- Centralized log ingestion
- KQL-based querying
- Custom table creation for non-native data sources

  <img width="1362" height="669" alt="1" src="https://github.com/user-attachments/assets/baadb8bf-cc5c-437d-94f7-8bef5ef30d7a" />
  
  *Figure 1: Log Analytics workspace*
  
  <img width="1317" height="431" alt="3" src="https://github.com/user-attachments/assets/75e9c92c-a456-40dd-a42c-b69ace2e1654" />

  *Figure 2: Cyber-Sentinel-Workspace Deployment*

  <img width="1917" height="681" alt="5" src="https://github.com/user-attachments/assets/407bbee6-b855-460c-a111-d4ac76a6a6c1" />

  *Figure 3: Deployment complete*

**Custom Log Ingestion Architecture**

To simulate ingestion of external telemetry, a full Azure Monitor ingestion pipeline was configured, including:
- Data Collection Endpoint (DCE) (SOC-DCE)
- Data Collection Rule (DCR) (DCR-Cisco-Upload)
- Custom Tables for structured log storage
  
This architecture reflects a real-world ingestion pipeline used to onboard third-party data sources into Sentinel (Figures 4-6).

<img width="852" height="886" alt="9" src="https://github.com/user-attachments/assets/c5720b35-daea-4f1d-9784-331c3611a32e" />

*Figure 4: Data Collection Endpoint*

<img width="989" height="898" alt="10" src="https://github.com/user-attachments/assets/5e66becd-afc0-4e7f-a69b-6d4e565cbe1b" />

*Figure 5: Custom Table*

The resource group confirms successful deployment of all required components:
- Log Analytics Workspace
- Microsoft Sentinel solution
- Data Collection Rule
- Data Collection Endpoint (Figure 8)

<img width="1919" height="590" alt="18 both logs ingested Resource Group" src="https://github.com/user-attachments/assets/05d77b5c-5601-4733-8738-bfca75be5928" />

*Figure 6: Resource Group Deployments*

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
  
- Sentinel generated a warning indicating no timestamp field was present (Figure 14)
  
<img width="1918" height="548" alt="16 ingesting windows logs time error" src="https://github.com/user-attachments/assets/68f54c05-c5cc-4f7c-9294-809727d83c6d" />

*Figure 14-16: Ingesting windows logs time error*

To resolve this:

The timestamp field (TimeGenerated_UTC) was normalized using transformation logic:

```kusto
source
| extend TimeGenerated = todatetime(TimeGenerated_UTC)
| project-away TimeGenerated_UTC
```
(Figure 15-17)

<img width="1910" height="898" alt="17 fixing time" src="https://github.com/user-attachments/assets/fd89eb88-dde6-4416-96af-ab4ad893d24b" />

*Figure 15: Resolving timestamp field*

This ensured:

- Accurate historical timestamp representation
- Proper alignment with Sentinel’s query and detection engine

**Ingestion Challenges and Validation**

Several real-world ingestion challenges were encountered and resolved, including:

- Missing or improperly formatted timestamp fields
- Schema conflicts with reserved field names
- Data Collection Rules not initially connected to sources (Figure 19)
- API-based ingestion requiring workspace authentication (Figure 20)
  
These issues were systematically debugged and corrected, demonstrating practical experience with Azure Monitor ingestion pipelines.

<img width="1912" height="394" alt="19  data sources not connected" src="https://github.com/user-attachments/assets/d427dd9b-458f-40e1-b68e-c2e2114bacbc" />

*Figure 16-19: Data sources not connected*

<img width="1126" height="286" alt="20 shared key" src="https://github.com/user-attachments/assets/4129db27-80cf-4b15-82fa-9a242237a7d9" />

*Figure 17-20: API-based ingestion*

**Schema Validation and Detection Readiness**

Although continuous data streaming was not established (due to lack of a live data source), both custom tables were successfully:

- Created and deployed
- Parsed and normalized
- Validated using KQL (getschema)
  
This confirms the environment is fully prepared for real-time ingestion and detection engineering.

Example validation query:

```kusto
CiscoStealthwatch_CL
| getschema
```
(Figure 18)

<img width="1919" height="815" alt="21 prrof" src="https://github.com/user-attachments/assets/0aaac1e7-8dae-4bde-9452-40457908d341" />

*Figure 18-21: CiscoStealthwatch_CL Schema*

**Summary**

The Sentinel environment successfully replicates a production-style SIEM ingestion pipeline by:

- Deploying core Sentinel infrastructure
- Building custom ingestion pipelines (DCR + DCE)
- Normalizing and parsing external log formats
- Validating schema for detection use
  
This demonstrates hands-on experience with:

- Azure Sentinel architecture
- Log ingestion engineering
- KQL-based data normalization
- Troubleshooting real-world SIEM ingestion issues


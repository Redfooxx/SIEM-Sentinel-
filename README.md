# SIEM-Sentinel- &nbsp;&nbsp;<img width="300" height="198" alt="Microsoft_Sentinel_logo1" src="https://github.com/user-attachments/assets/0330bec9-89fc-473d-b656-62bd751ce1bf" />

Deployed a Microsoft Sentinel environment to emulate enterprise-grade SIEM ingestion and monitoring workflows. A dedicated Log Analytics workspace (Cyber-Sentinel-Workspace) was provisioned within the SOC-Project-RG resource group. The workspace was then onboarded to Sentinel, enabling centralized log collection and analysis.

**Workspace and Sentinel Deployment**

A dedicated Log Analytics workspace (Cyber-Sentinel-Workspace) was created within the SOC-Project-RG resource group and successfully onboarded to Microsoft Sentinel, establishing the foundation for centralized log collection and analysis (Figures 1–3, 5).

This deployment enabled:
- Centralized log ingestion
- KQL-based querying
- Custom table creation for non-native data sources

  <img width="1362" height="669" alt="1" src="https://github.com/user-attachments/assets/baadb8bf-cc5c-437d-94f7-8bef5ef30d7a" />
  
  *Figure 1: Dashboard Alerts*
  
  <img width="1908" height="685" alt="2" src="https://github.com/user-attachments/assets/e517df6a-75dc-4bf4-95b7-fcfd18dfc4ab" />

  *Figure 2: Dashboard Alerts*

   <img width="1317" height="431" alt="3" src="https://github.com/user-attachments/assets/75e9c92c-a456-40dd-a42c-b69ace2e1654" />

  *Figure 3: Dashboard Alerts*

  <img width="1917" height="681" alt="5" src="https://github.com/user-attachments/assets/af742b46-46dc-4f04-9a1f-aa769c2e3263" />

  *Figure 4: Dashboard Alerts*

**Custom Log Ingestion Architecture**

To simulate ingestion of external telemetry, a full Azure Monitor ingestion pipeline was configured, including:
- Data Collection Endpoint (DCE) (SOC-DCE)
- Data Collection Rule (DCR) (DCR-Cisco-Upload)
- Custom Tables for structured log storage
  
This architecture reflects a real-world ingestion pipeline used to onboard third-party data sources into Sentinel (Figures 8–10, 18).

The resource group confirms successful deployment of all required components:
- Log Analytics Workspace
- Microsoft Sentinel solution
- Data Collection Rule
- Data Collection Endpoint
(Figure 18)



*Figure 5: Dashboard Alerts*

*Figure 6: Dashboard Alerts*

*Figure 7: Dashboard Alerts*

*Figure 8: Dashboard Alerts*

*Figure 9: Dashboard Alerts*

*Figure 10: Dashboard Alerts*

*Figure 11: Dashboard Alerts*

*Figure 12: Dashboard Alerts*

*Figure 13: Dashboard Alerts*

*Figure 14: Dashboard Alerts*

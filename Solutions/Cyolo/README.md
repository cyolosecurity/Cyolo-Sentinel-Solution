# Cyolo Solution for Microsoft Sentinel

Collect, monitor, and investigate **Cyolo Zero-Trust Access** events directly in Microsoft Sentinel.  
This solution ships a codeless data connector, sample analytics, and a starter workbook to give you
instant insights into application-access activity across your Cyolo environment.

[![Deploy to Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2F\<ORG\>/\<REPO\>/main/Solutions/Cyolo/CreateUiDefinition.json)

---

## Contents

| Folder / file | Description |
| ------------- | ----------- |
| **Data Connectors/CyoloDataConnector.json** | ARM template for the **RestApiPoller** connector that ingests Cyolo logs. |
| **IncidentScenarios/CyoloSuspiciousAccess.json** | Scheduled analytics rule detecting anonymous access to sensitive apps. |
| **Workbooks/CyoloAccessInsights.json** | Workbook showing top accessed applications and activity trends. |
| **Metadata/SolutionMetadata.json** | Content-hub metadata (name, publisher, version, dependencies). |
| **Package/package.template.json** | Master manifest listing all artefacts (required for Marketplace). |
| **CreateUiDefinition.json** | “Deploy to Azure” UI definition (parameters wizard). |

---

## Prerequisites

| Requirement | Notes |
|-------------|-------|
| **Log Analytics Workspace** | Sentinel-enabled workspace where data will be stored. |
| **Data Collection Endpoint (DCE)** | Public-enabled DCE in the same region as the workspace. |
| **Data Collection Rule (DCR)** | Must map `timestamp ➜ TimeGenerated` and route to the workspace. |
| **Cyolo API credentials** | Client ID & Secret with access to the `/v1/activity_log` endpoint. |

---

## Quick Deployment (CLI)

```bash
az deployment group create \
  -g <resource-group> \
  -f Solutions/Cyolo/Data\ Connectors/CyoloDataConnector.json \
  -p workspace="<workspace-name>" \
    workspace-location="<region>" \
    username="<cyolo-client-id>" \
    password="<cyolo-client-secret>" \
    dataCollectionEndpoint="<https://<dce>.ingest.monitor.azure.com>" \
    dataCollectionRuleImmutableId="<dcr-immutable-id>"

# Rubrik Security Cloud Google SecOps Playbooks

This repository contains sample playbooks that uses the **Rubrik Security Cloud** integration in **Google SecOps SOAR** platform. These playbooks automate threat detection, ransomware analysis, and file context enrichment using Rubrik Security Cloud data, enabling rapid and comprehensive insights for security events and backup operations.

These automated playbooks can be used as a reference to generate a custom workflow to fulfill your needs.


## Import Playbooks

Importing a playbook into any SecOps instance is straightforward:

- Download the `.zip` file of the playbook that needs to be imported. This file would also contain all the blocks that are used in the playbook.

![Download ZIP](<./screenshots/Download ZIP.png>)

- Open the SecOps instance. From the sidebar, navigate to **Response > Playbooks** section.

![Playbook Section](<./screenshots/Playbook Section.png>)

- Click on the **three dots** icon at the top of the Playbooks, and then click on **Import**.

![Import Playbook](<./screenshots/Import Playbook.png>)

- Select the `.zip` file downloaded earlier.

The playbooks stored in the file would be imported. We can access these under the **Imported Playbooks** folder. These playbooks and blocks can be moved to any other folder of our choice.

You can also download the [**All Playbook Bundle**](<./All Rubrik Security Cloud Playbooks.zip>) which includes all the Rubrik Security Cloud playbooks and blocks.

**Important:** Before using any playbook with an alert, review its default settings (triggers, action parameters, etc.) to ensure they align with your needs.

---

## Install other utilities for playbooks

- Go to **Content Hub**.

![Content Hub](<./screenshots/Content Hub.png>)

- Click on **Power-Ups**.

![Power Ups](<./screenshots/Power Up.png>)

- Search for **Tools** and click on **Install**. 

![Tools](<./screenshots/Tools.png>)

- Configure **Tools** by clicking on the **Configure** button.

![Configure Tools](<./screenshots/Configure Tools.png>)

- Go to **Response Integrations** and Search for **Siemplify** and click on **Install**.

![Siemplify](<./screenshots/Siemplify.png>)

- Configure **Siemplify**.

![Configure Siemplify](<./screenshots/Configure Siemplify.png>)

---
## Documentation

- [Playbooks](#playbooks)
  - [Rubrik Advanced IOC Scan](#rubrik-advanced-ioc-scan)
  - [Rubrik Turbo IOC Scan](#rubrik-turbo-ioc-scan)
  - [Rubrik Anomaly Analysis](#rubrik-anomaly-analysis)
  - [Rubrik File Context Analysis](#rubrik-file-context-analysis)
- [Blocks](#blocks)
  - [Rubrik Advanced IOC Scan Input Block](#rubrik-advanced-ioc-scan-input-block)
  - [Rubrik Turbo IOC Scan Input Block](#rubrik-turbo-ioc-scan-input-block)
  - [Rubrik Anomaly Analysis Input Block](#rubrik-anomaly-analysis-input-block)
  - [Rubrik File Context Analysis Input Block](#rubrik-file-context-analysis-input-block)
  - [Rubrik Sensitive Data Information Block](#rubrik-sensitive-data-information-block)
  - [Async Polling Interval](#async-polling-interval)
  - [Async Action Timeout](#async-action-timeout)
- [Known Behaviours](#known-behaviours)
- [Troubleshooting](#troubleshooting)

---
## Playbooks

Playbooks are a feature in Google Security Operations (SecOps) that can be used to automatically perform actions on alerts/cases when triggered. Playbooks can be used to: Retrieve information about alerts, Remediate threats, Create tickets, and Manage toxic combinations and IAM recommendations.

**Points to Note:**

- `.zip` file for every playbook contains the playbook along with all the blocks used in that playbook.
- Each playbook requires a trigger, which decides when the playbook would be run. Users can update the Trigger conditions based on the requirements.
- The imported playbooks are disabled by default. User can easily enable any playbook by simply clicking on the toggle button beside the playbook name. ([Reference](https://cloud.google.com/chronicle/docs/soar/respond/working-with-playbooks/whats-on-the-playbooks-screen#:~:text=At%20the%20top%20segment%20of%20the%20playbook%20designer%20pane%2C%20you%20can%20use%20the%20horizontal%20toggling%20button%20to%20enable%20or%20disable%20the%20playbook.))
  

### Rubrik Advanced IOC Scan

This playbook is used to perform an Advanced IOC scan using manually entered IOCs or other IOCs from Google SecOps. [Download](<./playbooks/Rubrik Advanced IOC Scan.zip>)

![Playbook - Rubrik Advanced IOC Scan](<./screenshots/playbooks/Rubrik Advanced IOC Scan.png>)

**Actions in Use:**

1. Advanced IOC Scan
2. IOC Scan Results

**Flow:**

- **Reset Context Advanced IOC**
  - Clears previous context data for Advanced IOC input to ensure clean execution. 

- **Rubrik Advanced IOC Scan Input Block**

  - Collects inputs from the user or automation and combines input fields to json.
  - The playbook starts by receiving inputs:
    - **Object ID**: The Object ID of the system on which to perform the scan. Supports comma separated values.
    - **Start Date**: Filter the snapshots from the provided date. Format supported:  yyyy-mm-dd, yyyy-mm-ddTHH:MM:SSZ.
    - **End Date**: Filter the snapshots from the provided date. Format supported:  yyyy-mm-dd, yyyy-mm-ddTHH:MM:SSZ.
    - **Advanced IOC**: Json encoded Indicators Of Compromise to scan. Json keys signify the type of IOC and the corresponding list of values are the values of the IOC's.
      ```json
      format:
      {
        "<ioc_type1>": ["<ioc_value1>", "<ioc_value2>"],
        "<ioc_type2>": "<ioc_value3>"
      }
      ```

- **Get Context for Advanced IOC**

  - Gets context for input - Advanced IOC (json).

- **Advanced IOC Scan (action)**

  - This action will start a new advanced threat hunt on the given IOC hash and it returns hunt id as output.

- **Get Result**

  - This action retrieves the scan results using the Hunt ID generated by the Advanced IOC Scan action. Since IOC Scan Results is an asynchronous action, it waits for the scan to complete and then fetches the results for the hunt created by the Advanced IOC Scan action.

**Widgets:**

The following widgets will be displayed on successful playbook completion:

1. **Advanced IOC Scan Overview:** The following values are shown:

   - Scan Start time
   - Scan End Time
   - IOC Values
   - Scanned Objects
   - Affected Objects
   - Unique File Matches
   - Scanned Snapshots
   - Matched Snapshots

2. **Advanced IOC Scan Result (Json Result)**

> **Note:** All the response from the actions will be saved to the widgets. Will be shown to the Alert Overview page inside Google SecOps Case for later use.

---

### Rubrik Turbo IOC Scan

This playbook is used to perform an Turbo IOC scan using manually entered IOCs or other IOCs from Google SecOps. [Download](<./playbooks/Rubrik Turbo IOC Scan.zip>)

![Playbook - Rubrik Turbo IOC Scan](<./screenshots/playbooks/Rubrik Turbo IOC Scan.png>)

**Actions in Use:**

1. Turbo IOC Scan
2. IOC Scan Results

**Flow:**

- **Rubrik Turbo IOC Scan Input Block**

  - Collects inputs from the user or automation and combines input fields to json.
  - The playbook starts by receiving inputs:
    - **IOC Hash**: the value of IOC Hash, the user needs to add the MD5, SHA1 or SHA256 hash values.
    - **Cluster ID**: ID of the cluster on which to perform a scan. Supports comma separated values.
    - **Start Time:** Filter the snapshots from the provided date. Format supported:  yyyy-mm-dd, yyyy-mm-ddTHH:MM:SSZ.
    - **End Time:** Filter the snapshots until the provided date. Format supported:  yyyy-mm-dd, yyyy-mm-ddTHH:MM:SSZ.

- **Turbo IOC Scan (action)**

  - This action will start a new turbo threat hunt on the given IOC hash and it returns hunt id as output.

- **Get Result**

  - This action retrieves the scan results using the Hunt ID generated by the Turbo IOC Scan action. Since IOC Scan Results is an asynchronous action, it waits for the scan to complete and then fetches the results for the hunt created by the Turbo IOC Scan action.

**Widgets:**

The following widgets will be displayed on successful playbook completion:

1. **Turbo IOC Scan Overview:** The following values are shown:

   - Scan Start time
   - Scan End Time
   - IOC Hash
   - Scanned Objects
   - Affected Objects
   - Unique File Matches
   - Scanned Snapshots
   - Matched Snapshots

2. **Turbo IOC Scan Result (Json Result)**

> **Note:** All the response from the actions will be saved to the widgets. Will be shown to the Alert Overview page inside Google SecOps Case for later use.

---

### Rubrik Anomaly Analysis

This playbook automatically retrieves anomaly events, sensitive data scan results, and associated cluster information from Rubrik Security Cloud for a specified object. [Download](<./playbooks/Rubrik Anomaly Analysis.zip>)

![Rubrik Anomaly Analysis](<./screenshots/playbooks/Rubrik Anomaly Analysis.png>)

**Actions in Use:**

1. List Events
2. Get Sonar Sensitive Hits
3. Get CDM Cluster Connection
4. Get CDM Cluster Location

**Flow:**

- **Rubrik Anomaly Analysis Input Block**
  - The playbook starts by receiving inputs:
    - **Object Name:** The Object Name to fetch Anomaly Events for.
    - **Cluster ID:** The Rubrik cluster ID associated with the object.

- **List Events**

  Fetches the list of most recent "Anomaly" events with last activity status as "Success" for the specified object and cluster.

- **Rubrik Sensitive Data Information Block**

  This block enriches the ransomware analysis context by retrieving sensitive data hits and detailed Rubrik CDM cluster information—including location and connection state—for the specified object.

**Widgets**

The following widgets will be displayed on successful playbook execution:

1. **Rubrik Anomaly Analysis Overview:** The following values are shown:
   - Object Name
   - Cluster ID
   - Sonar Sensitive HITS
   - Location
   - Connection State

2. **Anomaly Analysis (JSON Result)**
3. **Sonar Sensitive HITS (JSON Result)**
4. **Cluster Location (JSON Result)**
5. **Cluster Connection State (JSON Result)**

> **Note:** All the response from the actions will be saved to the widgets. Will be shown to the Alert Overview page inside Google SecOps Case for later use.

---

### Rubrik File Context Analysis

This playbook is used to retrieve file context information from Rubrik Security Cloud, to enrich the alert with file details, policy hits, and security analysis data. [Download](<./playbooks/Rubrik File Context Analysis.zip>)

![Rubrik File Context Analysis](<./screenshots/playbooks/Rubrik File Context Analysis.png>)

**Actions in Use:**

1. List Object Snapshots
2. List Sonar File Contexts

**Flow:**

- **Rubrik File Context Analysis Input Block**
  - Collects inputs from the user or automation:
  - The playbook starts by receiving inputs:
    - **Object Name**: The Object Name to fetch Sensitive HITS and Sonar File Context for.
    - **File Name**: Specify the name of the file, folder, or file share object.
    - **File Path**: Specify the standard file path to filter with.
    - **User ID**: Specify the user ID to filter with.

- **Get Sonar Sensitive HITS (action)**
  - Find the most recent data classification hits on an object.

- **List Sonar File Contexts (action)**
  - This action fetches file context information from Rubrik Security Cloud for the Snapshot ID returned from Get Sonar Sensitive HITS action and the File Path, File Name, Object ID given as input.

**Output**

The following widgets will be displayed on successful playbook execution:

1. **Rubrik File Context Analysis Overview:** The following values are shown:
   - Object Name
   - Snapshot ID
   - File Name
   - File Path

2. **Sonar File Context (JSON Result)**

> **Note:** All the response from the actions will be saved to the widgets. Will be shown to the Alert Overview page inside Google SecOps Case for later use.

---

## Blocks

A block is a re-usable set of actions and conditions that can be used in multiple playbooks. This acts as a wrapper for performing some set of actions, that are often performed in multiple playbooks.


### Rubrik Advanced IOC Scan Input Block

This block is used to collect inputs for Advanced IOC scan Playbook. [Download](<./blocks/Rubrik Advanced IOC Scan Input Block.zip>)

![Playbook - Rubrik Advanced IOC Scan Input Block](<./screenshots/blocks/Rubrik Advanced IOC Scan Input Block.png>)

**Input:**

- **Object ID:** Object ID of the system on which to perform the scan. Supports comma separated values.
- **Start Date:** Filter the snapshots from the provided date. Format supported:  yyyy-mm-dd, yyyy-mm-ddTHH:MM:SSZ.
- **End Date:** Filter the snapshots until the provided date. Format supported:  yyyy-mm-dd, yyyy-mm-ddTHH:MM:SSZ.
- **Advanced IOC:** Json encoded Indicators Of Compromise to scan. Json keys signify the type of IOC and the corresponding list of values are the values of the IOC's.
  ```json
  format:
  {
    "<ioc_type1>": ["<ioc_value1>", "<ioc_value2>"],
    "<ioc_type2>": "<ioc_value3>"
  }
  ```

**Flow:**

- **Set Context for Advanced IOC**
  - Saves the context in json format for input - Advanced IOC.

- **Extract Input Params (action)**
  - This action collects the input values and convert it to JSON, which is then used in other actions.

**Output:**

- Returns the generated `JSON output` containing all the input values.

---

### Rubrik Turbo IOC Scan Input Block

This block is used to collect inputs for Turbo IOC scan Playbook. [Download](<./blocks/Rubrik Turbo IOC Scan Input Block.zip>)

![Playbook - Rubrik Turbo IOC Scan Input Block](<./screenshots/blocks/Rubrik Turbo IOC Scan Input Block.png>)

**Input:**

- **Cluster ID:** ID of the cluster on which to perform a scan. Supports comma separated values.
- **Start Time:** Filter the snapshots from the provided date. Format supported:  yyyy-mm-dd, yyyy-mm-ddTHH:MM:SSZ.
- **End Time:** Filter the snapshots until the provided date. Format supported:  yyyy-mm-dd, yyyy-mm-ddTHH:MM:SSZ.
- **IOC Hash:** the value of IOC Hash, the user needs to add the MD5, SHA1 or SHA256 hash values.


**Flow:**

- **Extract Input Params (action)**
  - This action collects the input values and convert it to JSON, which is then used in other actions.

**Output:**

- Returns the generated `JSON output` containing all the input values.

---

### Rubrik Anomaly Analysis Input Block

This block is used to collect inputs for List Events and Rubrik Sensitive Data Information Blocks used in the Rubrik Anomaly Analysis Playbook. [Download](<./blocks/Rubrik Anomaly Analysis Input Block.zip>)

![Playbook - Rubrik Anomaly Analysis Input Block](<./screenshots/blocks/Rubrik Anomaly Analysis Input Block.png>)

**Input:**
  - **Object Name:** The Object ID or the Snappable ID (can be received from case if present)
  - **Cluster ID:** The Rubrik cluster ID associated with the object.

**Flow:**

- **Extract Input Params (action)**
  - This action collects the input values and convert it to JSON, which is then used in other actions.

**Output:**

- Returns the generated `JSON output` containing all the input values.

---

### Rubrik File Context Analysis Input Block

This block is used to collect inputs for File Context Analysis Playbook. [Download](<./blocks/Rubrik File Context Analysis Input Block.zip>)

![Playbook - Rubrik File Context Analysis Input Block](<./screenshots/blocks/Rubrik File Context Analysis Input Block.png>)

**Input:**
  - **Object Name**: The Object Name to fetch Sensitive HITS and Sonar File Context for.
  - **File Name**: Specify the name of the file, folder, or file share object.
  - **File Path**: Specify the standard file path to filter with.
  - **User ID**: Specify the user ID to filter with.

**Flow:**

- **Extract Input Params (action)**
  - This action collects the input values and convert it to JSON, which is then used in other actions.

**Output:**

- Returns the generated `JSON output` containing all the input values.

---

### Rubrik Sensitive Data Information Block

This block enriches the ransomware analysis context by retrieving sensitive data hits and detailed Rubrik CDM cluster information—including location and connection state—for the specified object. [Download](<./blocks/Rubrik Sensitive Data Information Block.zip>)

![Rubrik Sensitive Data Information Block](<./screenshots/blocks/Rubrik Sensitive Data Information Block.png>)

**Input:**

- **Object Name:** The Object ID or the Snappable ID.
- **Cluster ID:** Name of the file, folder, or file share object.

**Flow:**

- **Get Sonar Sensitive Hits (action)**
  - Retrieves most recent sensitive data hits from Rubrik Security Cloud.

- **Get CDM Cluster Location (action)**
  - Fetches the location information of the CDM cluster.

- **Get CDM Cluster Connection State (action)**
  - Retrieves the connection state of the CDM cluster.

**Output**

- Returns sensitive data hits, cluster location, and cluster connection state information.

---

## Get Results Action Block

The Get Results Action Block uses the IOC Scan Results action internally. This is an asynchronous action, meaning it continuously polls for the hunt results until they are ready. To avoid long-running executions, configure the Async Action Timeout and Async Polling Interval according to your requirements.

---

## Async Polling Interval
Defines how frequently the action checks whether the hunt results are ready. To update this, click Get Results Block → Settings.

![Async Polling Interval](<./screenshots/Async Polling Interval.png>)

---

## Async Action Timeout
Defines the maximum duration the async retry action is allowed to run before it automatically times out and the playbook stops. To update this, click Get Results Block → Settings.

### Example
If **Async Action Timeout = 2 days**:

If the retry action continues polling and the hunt results are not ready, the action will automatically time out and stop execution after 2 days.

![Async Action Timeout](<./screenshots/Async Action Timeout.png>)

---
## Known Behaviours
- Currently, only the view from the 1st attached playbook will be set for the **Alert Overview** ([Reference](https://www.googlecloudcommunity.com/gc/SecOps-SOAR/Playbook-Widget-Behavior-in-Alert-Overview/td-p/888644)). For example, if a playbook is already attached to an alert, then the alert overview page will only reflect the view generated by that playbook. Hence, it is advised to enable only the required playbooks. Make sure that the trigger for the playbook is not set to All. Instead, the trigger should be set such that the playbook must only run of relevant set of alerts.

---
## Troubleshooting

In case of any failures when running any playbook, we can debug the same by running the playbook in the Simulator Mode. For more details, please refer to this guide: [Google SecOps playbooks - Simulator](https://cloud.google.com/chronicle/docs/soar/respond/working-with-playbooks/working-with-playbook-simulator).

---
## References

- [Rubrik Security Cloud Documentation](https://github.com/rubrikinc/google-secops-content/tree/main/SOAR)
- [Google SecOps Playbooks Documentation](https://cloud.google.com/chronicle/docs/secops/google-secops-soar-toc#work-with-playbooks)

---

For questions or support, please contact your Rubrik or Google SecOps administrator.
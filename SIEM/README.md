# Rubrik Security Cloud Resources for Google SecOps SIEM Integration

## Dashboards

You can visualize and analyze ingested data by creating dashboards in Google SecOps SIEM.

### Import a Native Dashboard into Google SecOps SIEM

Complete the following steps to import a dashboard:

1. Download the dashboard `.json` file for Rubrik Security Cloud from the following [GitHub](https://github.com/rubrikinc/google-secops-content/tree/main/SIEM/Dashboards) repository.
2. From Google SecOps SIEM, Go to **Dashboards & Reports** > **Dashboards**, Select **New Dashboard** > **Import from JSON**.
3. Click on **Upload Dashboard files** dialog, browse and select the appropriate JSON file.
4. Click **Edit** to update the name, description, and the dashboard access you're importing.
5. Click **Import** to import the dashboard.

See [Import Dashboards into Google SecOps](https://cloud.google.com/chronicle/docs/reports/manage-native-dashboards#import-dashboards) for more information.


## Create Correlation Rules for Detections and Alerts

Correlation rules scan events ingested into Google SecOps SIEM to generate detections and alerts for specified anomalies. Rubrik Security Cloud provides seven rules that you can modify or copy to get started. You can find the Rubrik Security Cloud correlation rules in the following [GitHub](https://github.com/rubrikinc/google-secops-content/tree/main/SIEM/Correlation%20Rules) repository.

### Create a New Correlation Rule

1. From Google SecOps SIEM, navigate to **Detections > Rules & Detections**.
2. From the **Rules Editor** tab, click **New**.
3. In the rule editor, clear all the contents, and then copy and paste the code from the [GitHub](https://github.com/rubrikinc/google-secops-content/tree/main/SIEM/Correlation%20Rules) repository.
4. Click **Save New Rule**.
5. To generate alerts from the correlation rule, click the three dots next to the rule name and enable the **Alerting** option.

See [Manage rules using Rules Editor](https://cloud.google.com/chronicle/docs/detection/manage-all-rules#:~:text=Click%20New%20in,click%20DISCARD.) for more information.

## Create Search Queries

After the data is ingested into Google SecOps SIEM as events, you can use predefined search queries to visualize and analyze the data. Rubrik Security Cloud provides ten search queries that you can modify or copy to get started. You can find the Rubrik Security Cloud search queries in the following [GitHub](https://github.com/rubrikinc/google-secops-content/tree/main/SIEM/Search%20Queries) repository or in the SIEM User Guide.

### Create a New Search Query

1. From Google SecOps SIEM, navigate to **Investigation** > **SIEM Search**.
2. Go to **Search Manager**, click on **+** icon.
3. Copy and paste the Search Query in **UDM SEARCH**, Title in **Title** and Description in **Description** from the Search Queries listed in [GitHub](https://github.com/rubrikinc/google-secops-content/tree/main/SIEM/Search%20Queries) repository or from the SIEM User guide.
4. Click on **SAVE EDITS**.

See [Google SecOps: Saved Searches](https://security.googlecloudcommunity.com/community-blog-42/new-to-google-secops-saved-searches-4047) for more information.

---

For more information, please refer the User guide.
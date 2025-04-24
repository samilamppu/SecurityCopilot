# Weekly Incident Report - Enhanced Version

Required Plugin(s): *Sentinel, MDTI, MDCAttackPaths*

The ultimate solution would be to automate the flow through Logic Apps (coming soon). Use the following prompts to demonstrate how the flow would work in your environment.

1. **Get Sentinel incidents from the past 7 days**
    ```
    Get Sentinel Incidents from the past 7 days. Use the securityincidents data table. Do not set any provider to the KQL query. Include the following details: IncidentNumber, Title, AdditionalData.tactics, AdditionalData.techniques, Status, Severity, TimeGenerated, ClosedTime
    ```

2. **Get Sentinel alerts from the past 7 days**
    ```
    Get recent alerts from the past 7 days with full entity context, to support triage and investigation by the SOC team, presented in a table with alert title, severity, status, timestamp, alertId, alertLink and associated entities like user, IP, and device, using data from Microsoft Defender XDR and Microsoft Sentinel.
    ```

3. **Analyze Defender for Cloud Attack Paths and Correlate them to alerts**
    ```
    /MDCAttackPaths - Analyze the correlation between attack paths and alert data (make sure to include all alerts from the previous prompt) by comparing entities involved in each. Identify which entities found in attack paths are also present in the alerts. The table should include the following columns: - Title (of the alert) - Severity - Timestamp - Entity - AlertId - AlertLink - AttackPath (mark as true if an attack path exists to the entity, otherwise false) . Present the results in a table for better visualization.
    ```

4. **Analyze Defender for Cloud Attack Paths and Correlate them to alerts**
    ```
    /askGPT based on the responses above give me a breakdown of status of the incidents and breakdown of incident severity (low, medium, high) including MITRE ATT&CK tactics. Make sure to include all incidents in the data from the fist prompt. If possible, include combined table that contains all data for better visualization.
    ```
   
5. **MDTI Recommendations for incidents based on MITRE ATT&CK tactics:**
    ```
    /AskGPT - Using all previously retrieved incident data (not alerts), generate a single, environment-specific table that highlights recommendations to improve security posture. Use Defender XDR recommended actions and Microsoft threat intelligence as sources. For incidents with the same title, combine them into one row. Include the recommendation and source (Defender XDR, Threat Intelligence, or Both), MITRE ATT&CK tactic (only if available in the incident data), Justification for priority. Priority Level (Critical, Medium, Low). Format the output as a clean, structured table for executive reporting.
    ```
   
6. **Create weekly report**:
    ```
    /AskGPT Generate a Weekly Executive Security Incident Report based on previously retrieved incidents and mapped MITRE ATT&CK data. The report should include: 1. Executive Summary: Brief overview of key findings, trends, and notable risks. Focus on potential business impact and areas requiring attention. 2. Incident Summary Table: Total incident count, grouped by status (Open, In Progress, Resolved), with severity breakdown if available. 3. MITRE Tactics Heatmap: Table showing the number of incidents aligned to each MITRE tactic to highlight common attacker objectives. 4. Trend & Risk Analysis: - Detect significant changes or anomalies compared to previous weeks. - Analyze resolution rates and average time-to-resolve. - Highlight any unresolved incidents older than 7 days, especially high severity or recurring ones. 5. Combined Incident Overview: Provide a ‘Combined Incident Details' table from previous prompt showing key attributes per incident (e.g., ID, title, severity, status, affected assets, detection time). 6. Presentation Format: Use concise tables and bulleted insights tailored for executive consumption. 7. Emphasize clarity, relevance, and strategic insights over technical detail.
    ```

7. **Create weekly report**:
    ```
    /AskGPT - Based on incident trends and alert data: 1. Recommend actions to reduce Mean Time to Triage (MTTT) and Mean Time to Respond (MTTR) based on observed inefficiencies. 2. propose actionable steps to reduce Mean Time to Triage (MTTT) and Mean Time to Respond (MTTR) based on observed inefficiencies. - Provide recommendations based on trends in severity and tactics - Provide additional insights for improving operational effectiveness, such as optimizing resources, leveraging automation, or enhancing monitoring.
    ```

The initial logic and flow is based on the Microsoft template. It has been further developed to enrich the data by correlating MDTI data with security posture enhancements.

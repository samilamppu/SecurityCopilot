# Weekly Incident Report

Required Plugin(s): *Sentinel, MDTI*

The ultimate solution would be to automate the flow through Logic Apps (coming soon). Use the following prompts to demonstrate how the flow would work in your environment.

1. **Get Sentinel incidents from the past xx days**
   ```
   Get Sentinel incidents from the past 7 days by running the following query: SecurityIncident | where CreatedTime >= ago(7d) | project IncidentNumber, Title, AdditionalData.tactics, Status, Severity, TimeGenerated, ClosedTime.

If multiple incidents are on the same incident number, select the one with the latest TimeGenerated timestamp.
   ```

2. **Grouping and analyzing the incidents:**
   ```
    /askGPt based on the response above give me a breakdown of status of the incidents and breakdown of incident severity (low, medium, high) including MITRE ATT&CK tactics. If possible, include combined table that contains all date for better visualization.
   ```
   
3. **MDTI Recommendations of incidents:**
   ```
   Based on the threat categories identified above in previous responses, use threat intelligence provides recommendations on how to protect my organization against these threats in an optimized, concise manner. Ensure that each threat category includes relevant for the identified threats. If possible, include a table for better visualization.
   ```
   
5. **Create weekly report**:
   ```
   /AskGpt - Generate a Weekly Incident Report based on previous responses. 

Take into account the following requirements: 
- Use tables whenever possible for better visualization. 
- Start with the incident and tactics summary tables by providing the total number of incidents reported during the period, highlighting any patterns or anomalies observed in the volume, and including key parameters from the grouping and analyzing incidents: incident statuses (open, resolved, and in progress) and MITRE tactics. 
- Highlight the resolution rate and any incidents that have remained unresolved for extended durations.
- After the incident summary, use the combined table (Combined Incident Details for Better Visualization) to summarize incidents in detail.

Based on MDTI recommendations and MDTI analyze impact of incidents, provide recommendations with a focus on the following key topics: 
- Provide recommendations based on trends in severity and tactics, suggesting measures such as enhanced detection systems, training programs, or specific preventive controls. 
- Related threat mitigation recommendations include a table that contains bullet point list about the recommendations.

In addition, propose actionable steps to reduce Mean Time to Triage (MTTT) and Mean Time to Respond (MTTR) based on observed inefficiencies. 
- Provide additional insights for improving operational effectiveness, such as optimizing resources, leveraging automation, or enhancing monitoring. 
- Ensure the report is concise, data-driven, and structured to support decision-making and proactive responses.
   ```

This flow is based on the Microsoft template and is further developed to enable automated incident reporting through Az Logic Apps with the correlation of MDTI data and security posture enhancements. 

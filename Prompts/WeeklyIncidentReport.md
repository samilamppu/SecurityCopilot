# Weekly Incident Report

Required Plugin(s): *Sentinel, MDTI*

The ultimate solution would be to automate the flow through Logic Apps (coming soon). Use the following prompts to demonstrate how the flow would work in your environment.

1. **Get Sentinel incidents from the past xx days**
   ```
   Get Sentinel incidents from the past 7 days by running the following query: SecurityIncident | where CreatedTime >= ago(7d) | project IncidentNumber, Title, AdditionalData.tactics, Status, Severity, TimeGenerated, ClosedTime. If there are multiple incidents on the same incident number select the one with the latest TimeGenerated timestamp.
   ```

2. **Grouping and analyzing the incidents**
   ```
    /askGPt based on the response above, give me a breakdown of status of the incidents and breakdown of incident severity (low, medium, high) including MITRE ATT&CK tactics. If possible, include combined table that contains all date for better visualization.
   ```
   
3. **MDTI Recommendations for incidents based on MITRE ATT&CK tactics:**
   ```
   Based on the incidents on the previous prompts, show me the impact of the threats for my organization. Give me recommendations on how to enhance environment security posture to prevent similar incident occurring in the future in my environment. Use both, Defender XDR incidents 'recommended actions' and threat intelligence data and combine recommendations into the table for better visualization. Divide the output between security posture recommendations from Defender XDR and recommendations from threat intelligence based on mitre tactics. Add an additional column to the table that highlights the recommendation source.
   ```
   
4. **Create weekly report**:
   ```
   /AskGPT - Generate a Weekly Incident Report based on previous responses. 

Take into account the following requirements: 
- Use tables whenever possible for better visualization. 
- Start with the incident and tactics summary tables by providing the total number of incidents reported during the period, highlighting any patterns or anomalies observed in the volume, and including key parameters from the grouping and analyzing incidents: incident statuses (open, resolved, and in progress) and MITRE tactics. 
- Highlight the resolution rate and any incidents that have remained unresolved for extended durations.
- After the incident summary, use the combined table (Combined Incident Details for Better Visualization) to summarize incidents in detail.

Based on MDTI recommendations and MDTI analyze impact of incidents, provide recommendations with a focus on the following key topics: 
- Related threat impact and mitigations visualize this part by using the following tables: Security Posture Recommendations from Defender XDR & Recommendations from Threat Intelligence Based on MITRE Tactics

In addition, propose actionable steps to reduce Mean Time to Triage (MTTT) and Mean Time to Respond (MTTR) based on observed inefficiencies. 
- Provide recommendations based on trends in severity and tactics
- Provide additional insights for improving operational effectiveness, such as optimizing resources, leveraging automation, or enhancing monitoring. 
- Ensure the report is concise, data-driven, and structured to support decision-making and proactive responses.
   ```

This flow is based on the Microsoft template. It has been further developed to enable automated incident reporting through Az Logic Apps by correlating MDTI data with security posture enhancements. 

# Enriched AiTM Incident Investigation

Required Plugin(s): *MDCAttackPath Plugin*

Plugins:
- [MDCAttackPath Plugin](https://github.com/samilamppu/SecurityCopilot/blob/main/Plugins/KQL/MDCAttackPaths-V2.yaml)


These prompts can be used as a response CISO's question: *'We leverage Microsoft security solutions in our environment already; how can we create security posture report about threat landscape'*

**Daily Security Posture Report flow**

<img src="https://raw.githubusercontent.com/samilamppu/SecurityCopilot/main/Media/DailySecurityPosture-flow.png" alt="DailyPostureReport-flow" width="600">

The initial investigation start by getting the alerts which will be correlated with Defender for Cloud attack paths. Next, we will gather MDTI data and correlate top threat to the organization data.


1. **Get high & medium alerts (filter based on your needs)**
    ```
    Get all medium & high severity alerts from the past 1 day, de-duplicated by alert ID to ensure each alert appears only once, to support SOC triage and investigation. Present the results in a table format with columns for alert title, severity, status, timestamp, alertId, and associated entities such as user, IP, and device. Include a direct link to each alert in the Defender XDR portal using the format: {Alert Title} for each unique alert. Use data from Microsoft Defender XDR and Microsoft Sentinel.
    ```

2. **Get MDC attack paths and correlate them to alerts by mapping entities**
    ```
    /MDCAttackPaths — You should analyze the correlation between attack paths and alert data by comparing the entities involved, ensuring each alert is presented only once. Include all unique alerts from the previous prompt. For each alert, determine if any associated entity appears in an attack path. Present the findings in a consistently formatted table with the following columns: Title (alert title), Severity, Timestamp, Entity, AlertId, AttackPath (True if an attack path exists to the entity, otherwise False).
    ```

3. **Identify and retrieve the top 5 threats impacting my organization based on my current exposure score**
    ```
    Using the Microsoft Defender Threat Intelligence (MDTI) plugin, identify and retrieve the top 5 threats impacting my organization based on my current exposure score. Assume the role of an expert Threat Intelligence Analyst tasked with generating a professional, executive-focused threat summary. Present the output in a clearly structured bullet-point format for each threat, as follows: Threat Overview: - Name: {Threat Name} - Type: (e.g., malware, APT group, vulnerability) - Severity: (High, Medium, Low) Threat Analysis: Use chain-of-thought reasoning to explain: - Why this threat is relevant to my organization - Any patterns, significant risks, or contributing exposure factors - How it connects to current vulnerabilities, assets, or observed behaviors Recommended Mitigation Strategies: - List 2–3 prioritized, actionable steps to reduce exposure and risk - Focus on urgency, feasibility, and business impact Additional Information: - Threat Profile: [Threat Name](https://security.microsoft.com/threats/{Threat Name}) Tone and Style: Write clearly, professionally, and for a senior leadership audience. Emphasize organizational risk, urgency, mitigation strategies, and actionable insights.
     ```

4. **Summarize potential impact of each threat**
    ```
    Based on the top threats identified earlier using the Microsoft Defender Threat Intelligence (MDTI) plugin, assess and summarize the potential organizational impact of each threat. Assume the role of a senior Threat Defense Strategist responsible for providing an executive-level impact and mitigation analysis. - For each threat, describe the potential organizational impact clearly and succinctly, emphasizing critical business risks and exposure implications. - Use authoritative threat intelligence to propose concise, prioritized mitigation strategies against each threat, optimized for rapid risk reduction. - Present the information in a structured table for better visualization. Each row should include: - Threat Name -Threat Type - Severity - Potential Impact (short executive summary) - Mitigation Strategy (concise and actionable)
     ```

5. **Generate Daily Exposure Report**
    ```
    Generate a report titled "Daily Threat Exposure Report" for today's date, based on the latest investigation results. Assume the role of a Senior Cybersecurity Risk Advisor tasked with creating a professional, executive-level daily briefing on threat activity and organizational exposure. The report must include the following sections: 1. Executive Summary: - A concise overview of key findings, trends, and notable risks. - Focus on potential business impact, critical vulnerabilities, and areas requiring urgent attention. - Limit the summary to 100-150 words for quick executive consumption. 2. Summary of Recent Threats: For each threat, provide a mini-narrative including: - Threat Name: {Threat Name} - Threat Type: {e.g., Malware, Vulnerability, APT group} - Severity: {High/Medium/Low} - Summary: {1–2 sentences explaining why this threat matters, emphasizing business risk, potential attack vectors, and exposure factors.} - Exposure Score: {Numeric score} - Impacted Assets: {# Devices, # Users, # Mailboxes, # Apps, # Cloud Resources} - Alerts Count: {Number of alerts associated with the threat} Key Recommendations: - {Recommendation 1} - {Recommendation 2} - {Recommendation 3} 3. Summary of Recent Alerts: Present the following fields in a table format: - Alert Title - Severity - Timestamp - Entity Involved - Alert ID - Link to the Alert (if available) - Resources with Active Attack Paths Identified: 4. Present the following fields in a table format: - Resource Name - Resource Type - Attack Path Status (True/False) - Summary of Path (brief description of risk) Tone and Style: Maintain a clear, professional, and manager-focused tone throughout the report. Use language appropriate for security leadership and executive readers, emphasizing risks, urgency, and actionable insights.
     ```
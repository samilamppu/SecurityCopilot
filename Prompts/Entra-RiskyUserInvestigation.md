# Entra ID Risky Users Investigation Promptbook

Required Plugin(s): *Entra plugin, AbuseIPDB plugin*

Plugins:
- [Microsoft Entra plugin](https://learn.microsoft.com/en-us/entra/fundamentals/copilot-security-entra?bc=%2Fsecurity-copilot%2Fbreadcrumb%2Ftoc.json&toc=%2Fsecurity-copilot%2Ftoc.json)
- [AbuseIPDB Plugin](https://learn.microsoft.com/en-us/copilot/security/plugin-abuseipdb)


This promptbook/prompts can be used for further investigating of Entra ID Protection alerts and risky users. 

As mentioned on the Security Copilot page: Organizations face significant challenges in investigating and triaging identity protection alerts for sign-in anomalies, especially when users appear to log in from geographically disparate locations within hours. These challenges include:
- Volume of Alerts: Large organizations generate numerous risky sign-in events daily.
- False Positives: Legitimate activities, such as VPN connections or device relocations, may be flagged.
- Resource Constraints: Security teams must efficiently prioritize true positives for investigation

The initial investigation starts from Entra Identity Protection, and analysts can enrich the investigation in the standalone SC portal using these prompts.The first prompt is based on Microsoft's investigate anomalous sign-ins, found in [Security Copilot GitHub](https://github.com/Azure/Security-Copilot/blob/main/Promptbook%20samples/Anomalous%20Sign-Ins%20detection.md). 

1. **Filter Risky Users based on geo location (distance more than 500km)**
    ```
    Retrieve Defender XDR information using this KQL query: let riskyusers = AADUserRiskEvents | where TimeGenerated is greater than or equal ago(7d) | project UserPrincipalName, TimeGenerated, Location, IpAddress, RiskState, Id, RiskEventType; riskyusers | join kind=inner ( riskyusers | extend TimeGenerated1 = TimeGenerated, LocationDetails1 = Location ) on UserPrincipalName | where TimeGenerated is less than TimeGenerated1 and datetime_diff('hour', TimeGenerated1, TimeGenerated) is less than or equal 3 | extend latyy = Location.geoCoordinates.latitude | extend longy= Location.geoCoordinates.longitude | extend latyy1 = LocationDetails1.geoCoordinates.latitude | extend longy1 = LocationDetails1.geoCoordinates.longitude | extend distance = geo_distance_2points(todouble(Location.geoCoordinates.latitude), todouble(Location.geoCoordinates.longitude), todouble(LocationDetails1.geoCoordinates.latitude), todouble(LocationDetails1.geoCoordinates.longitude)) | where distance is greater than or equal 50000 | summarize arg_max(TimeGenerated, *) by Id | where RiskState is not equal @"dismissed" | project UserPrincipalName, TimeGenerated, IpAddress, Location, TimeGenerated1, IpAddress1, LocationDetails1, RiskEventType, distance
    ```

2. **Enrich Impacted user information**
    ```
    Using the previously returned dataset of impacted users (including User Principal Name, IP address, sign-in times, location, and risk event type): For each user, generate a concise and actionable summary addressed to both a security analyst and a security engineer. Follow this structure:
    1. Start with the User Principal Name (UPN)
    2. Summarize significant trends or behavioral anomalies, such as:
    - Unusual login times
    - Geo-location changes or travel patterns
    - Use of unfamiliar or high-risk applications
    3. Reference past sign-in history over the last 7 days, including:
    - Typical login patterns vs. these events
    - MFA or conditional access policy usage
    - Any repeat appearances in other risk events or incidents
    4. Application context, including:
    If the exact app is not listed in the prior data, use the risk event type and sign-in context to infer whether it relates to:
    - Microsoft 365 (Exchange, Teams, SharePoint, etc.)
    - Azure AD-integrated SaaS applications
    - Admin portals or privileged management apps
    5. Actionable recommendations, such as:
    - Suggested user investigation or escalation
    - Conditional access or authentication policy adjustments
    - Tagging or labeling for further watch
    - Audit trail reviews for impacted applications
    Use only the data already available in the previous prompt result — do not run additional KQL queries. Prioritize clarity, relevance, and analyst-oriented guidance.
    ```

3. **Enrich IP-address information by AbuseIPDB**
    ```
    Goal: Evaluate every unique IP address contained in the in-session table riskyusers (returned in Prompt #1) against the AbuseIPDB plugin and merge the results back into that table. Context: riskyusers currently holds UserPrincipalName, IpAddress, TimeGenerated, Location, and RiskEventType columns for the last 7 days. No new KQL should be executed. Expectations: 1. Use the AbuseIPDB plugin call @abuseipdb ip={IpAddress} for each distinct IP. 2. Capture at least these plugin-returned fields: Abuse Score, Last Reported, Total Reports, TOR. 3. Append those three fields as new columns to the existing riskyusers rows. 4. When multiple users share the same IP, reuse the one plugin response (no duplicate look-ups). 5. After enrichment, show a single combined table sorted by Abuse Score (highest first). Source. Only the data already present in riskyusers and the AbuseIPDB plugin. Do not generate or run additional KQL queries.
   ```

An example output from the last prompt where IP-addressess are analyzed.

**Prompt**

 <img src="https://raw.githubusercontent.com/samilamppu/SecurityCopilot/main/Media/AbuseIPDB-prompt.png" alt="Querying IPs from AbuseIPDB" width="400">

**Output**

 <img src="https://raw.githubusercontent.com/samilamppu/SecurityCopilot/main/Media/geo-1.png" alt="Response after analysis" width="600">
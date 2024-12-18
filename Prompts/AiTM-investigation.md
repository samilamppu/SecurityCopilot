# Adversary-in-the-Middle (AiTM) Investigation

Required Plugin(s): *KQL investigation (KQL_AiTMInvestigation.yaml)*

To investigate possible AiTM attack related activities using Copilot for Security, you could use the following series of prompts:

1. **Investigate sign-in details from the past 14 days including the key parameters for correlation**::
   ```
   /AiTM Investigation DaysAgo:<Timestamp> and include sessionId, applicationId and AccountObjectId on the results
   ```

2. **Summarize user sign-in activites (from the earlier prompt) to Office Home app and per country to identify possible suspicious patterns**:
   ```
    /AiTMCountrySummary - Summarize user sign-ins to Office Home application per country from the last 10 days
   ```

3. **Is the user's device part of the risk?**:
   ```
   Investigate behaviors for <userPrincipalName>
   ```

These prompts are designed to guide Security Copilot through Adversary-in-the-Middle investigation. This only scrathes the surface. More information about the AiTM attack in https://github.com/Cloud-Architekt/AzureAD-Attack-Defense/blob/main/Adversary-in-the-Middle.md#custom-detections-and-hunting. Remember to tailor these prompts to fit the specific context and systems of your organization.

The first two queries are from Microsoft Dart Team - https://www.microsoft.com/en-us/security/blog/2022/07/12/from-cookie-theft-to-bec-attackers-use-aitm-phishing-sites-as-entry-point-to-further-financial-fraud/?msockid=28c8c6feb17d6e740accd40eb04a6f51/
# Enriched AiTM Incident Investigation

Required Plugin(s): *Entra, Conditional Access Policies Plugin (custom API plugin), AiTM-investigation (custom KQL plugin)*

Plugins:
- [Conditional Access Policies plugin](https://github.com/samilamppu/SecurityCopilot/blob/main/Plugins/API/EIDCAP-API.yaml)
- [AiTM Investigation KQL Plugin](https://github.com/samilamppu/SecurityCopilot/blob/main/Plugins/KQL/KQL_AiTMInvestigation.yaml)


These prompts can be used for further investigating of an AiTM attack to response CISO's question: *'I thought our CA policies require phishing-resistant MFA, and our environment would be protected from AiTM attacks?'*

**Incident investigation flow**

<img src="https://raw.githubusercontent.com/samilamppu/SecurityCopilot/main/Media/Flow.png" alt="Incident investigation flow" width="600">

The initial investigation starts from the Defender XDR portal using Security Copilot embedded mode. After finalizing the investigation in the XDR portal, analysts can enrich the investigation in the standalone SC portal using these prompts.

![Incident summary](https://raw.githubusercontent.com/samilamppu/SecurityCopilot/main/Media/IncidentSummary.png)

1. **Get user information**
    ```
    Retrieve detailed user information for <userPrincipalName> using the Microsoft Entra plugin. Use the Microsoft Entra plugin to retrieve the user's group memberships.
    ```

2. **List environment CA policies**
    ```
    Execute /ListConditionalAccessPolicies to retrieve all Conditional Access policies, including policy name, conditions, grant controls, and enforcement status.
    ```

3. **Evaluate CA policies**
    ```
    /AskGPT - From the Conditional Access policies retrieved, list the policies where the grant controls require 'Phishing-resistant MFA' (e.g., FIDO2, Certificate-based authentication, Windows Hello for Business). Provide policy names and their assigned conditions (users, groups, roles).
    ```

4. **Correlate CA policies to user**
    ```
    For the following policies retrieved on the previous prompt: [<List CA policies here, separated with comma, policy1, policy2, etc>], check if '<userPrincipalName' is directly assigned or part of any assigned group/role. Use the Microsoft Entra plugin to retrieve the user's group memberships and match against the policy assignments.
    ```

5. **Evaluate AiTM Activities**
    ```
    Get possible AiTM related sign-ins correlated to OfficeHome application from the past 7 days.
    ```

6. **Find if there are any inbox rule made on the sessions above**
    ```
    /AiTMFindCreatedInboxRules - Find new email inbox rules created during a suspicious sign-in session
    ```
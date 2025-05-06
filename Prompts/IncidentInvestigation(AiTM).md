# Enriched Incident Investigation

Required Plugin(s): *Entra, ListConditionalAccessPolicies, AiTM-investigation*

These prompts can be used for further investigating AiTM attack to response CISO's question: 'I thought our CA policies require phishing-resistant MFA, and our environment would be protected from AiTM attacks?'

1. **Get user information**
    ```
    Review detailed user information for <userPrincipalName> using the Microsoft Entra plugin. Include the following data: sign-n activity, device registration status, authentication methods (especially MFA), assigned roles, risk detections, and alert history. Summarize the user's current risk posture in 3-4 bullet points.
    ```

2. **List environment CA policies**
    ```
    Execute /ListConditionalAccessPolicies to retrieve all Conditional Access policies, including policy name, conditions, grant controls, and enforcement status
    ```

3. **Evaluate CA policies**
    ```
    /AskGPT - From the Conditional Access policies retrieved, list the policies where the grant controls require 'Phishing-resistant MFA' (e.g., FIDO2, Certificate-based authentication, Windows Hello for Business). Provide policy names and their assigned conditions (users, groups, roles).
    ```

4. **Correlate CA policies to user**
    ```
    For the CA policies retrieved above, check if <userPrincipalName> is directly assigned or part of any assigned group/role. Use the Microsoft Entra plugin to retrieve the user's group memberships and match against the policy assignments.
    ```

5. **Evaluate AiTM Activities**
    ```
    Get possible AiTM related sign-ins correlated to OfficeHome application from the past 7 days.
    ```

6. **Find if there are any inbox rule made on the sessions above**
    ```
    /AiTMFindCreatedInboxRules - Find new email inbox rules created during a suspicious sign-in session
    ```
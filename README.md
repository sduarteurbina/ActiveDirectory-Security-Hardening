# ActiveDirectory-Security-Hardening
Active Directory Security Hardening: Implementing Foundational Password &amp; Account Lockout Policies

#### **1. Project Overview**

This project demonstrates the implementation of essential security controls within a Windows Server Active Directory environment. The objective was to configure and enforce baseline password and account lockout policies using Group Policy Objects (GPOs) to strengthen the domain's security posture against common credential-based attacks.

This is a core task for any system administrator or cybersecurity professional, as properly configured account policies are the first line of defense in mitigating unauthorized access and protecting against brute-force attempts, as outlined in frameworks like the NIST Cybersecurity Framework and CIS Controls.

#### **2. Tools and Environment**

*   **Virtualization:** Microsoft Hyper-V
*   **Operating System:** Windows Server 2022
*   **Core Services:** Active Directory Domain Services (AD DS), DNS
*   **Configuration Tool:** Group Policy Management Console (GPMC)

#### **3. Configuration Process**

The "Default Domain Policy" GPO was edited to ensure these settings were applied domain-wide. The configuration was divided into two critical areas: Password Policy and Account Lockout Policy.

##### **A. Password Policy Configuration**

These settings enforce password complexity and lifecycle management, making credentials more resistant to guessing, cracking, and reuse.

| Policy Setting | Value Configured |
| :--- | :--- |
| Enforce password history | 10 passwords remembered |
| Maximum password age | 90 days |
| Minimum password age | 14 days |
| Minimum password length | 10 characters |
| Password must meet complexity | Enabled |

<img width="2350" height="1545" alt="Screenshot 2025-10-26 224020" src="https://github.com/user-attachments/assets/28425029-4e7b-4589-96e1-c25714b778e0" />


##### **B. Account Lockout Policy Configuration**

These settings are a direct countermeasure to automated brute-force attacks, preventing an adversary from making unlimited, rapid-fire logon attempts.

| Policy Setting | Value Configured |
| :--- | :--- |
| Account lockout threshold | 5 invalid logon attempts |
| Account lockout duration | 60 minutes |
| Reset account lockout counter after | 10 minutes |

<img width="2345" height="1550" alt="Screenshot 2025-10-26 224133" src="https://github.com/user-attachments/assets/9f797bcf-4bee-498b-a40c-f861d3390834" />


#### **4. Security Rationale & Impact Analysis**

Each configured policy directly maps to a specific security principle and mitigates a known threat vector.

##### **Rationale for Password Policies:**

*   **Enforce password history (10 passwords):** This prevents users from immediately reusing old, potentially compromised passwords, thereby disrupting an attacker's ability to use credentials from previous breaches.
*   **Maximum password age (90 days):** This limits the effective lifespan of a credential. If a password is compromised, this policy ensures it will eventually become invalid, reducing the window of opportunity for an attacker.
*   **Minimum password age (14 days):** This control prevents a user from immediately cycling through 10 passwords to get back to their original one after a forced change, thus defeating the purpose of the password history requirement.
*   **Minimum password length (10 characters):** Increasing password length exponentially increases the time and computational resources required for an attacker to successfully execute a brute-force or dictionary attack.
*   **Complexity Requirements (Enabled):** Requiring a mix of uppercase, lowercase, numeric, and special characters further expands the character set, dramatically increasing the difficulty of password cracking attempts.

##### **Rationale for Account Lockout Policies:**

*   **Account lockout threshold (5 attempts):** This is a direct mitigation for **MITRE ATT&CK T1110 - Brute Force**. By locking an account after a small number of failed attempts, automated password-spraying and brute-force tools are rendered ineffective.
*   **Account lockout duration (60 minutes):** This setting imposes a significant time penalty on the attacker, making any continued manual or automated attack effort impractical.
*   **Reset lockout counter (10 minutes):** A shorter reset counter ensures that a legitimate user who simply mistyped their password a few times does not have to wait the full lockout duration to try again, balancing security with user productivity. A critical consideration here is the risk of a Denial-of-Service (DoS) attack, where an attacker could intentionally lock out legitimate users. The configured values represent a reasonable balance between mitigating brute-force attacks and minimizing operational disruption.

#### **5. Next Steps & Further Enhancements**

While this project establishes a critical baseline, a more mature security implementation would include the following steps:

1.  **Fine-Grained Password Policies (FGPPs):** Implement more stringent policies (e.g., 15+ character passwords, shorter max age) for privileged accounts like Domain Admins and Service Accounts, as these are high-value targets.
2.  **SIEM Integration:** Forward Active Directory security event logs (e.g., Event ID 4625 - Failed Logon, 4740 - Account Lockout) to a SIEM like Splunk or Wazuh. This would enable real-time alerting on suspected brute-force activity, providing the visibility needed for a rapid incident response.
3.  **Auditing and Compliance:** Regularly audit GPO settings against established security baselines, such as the CIS Benchmarks for Windows Server, to ensure continuous compliance and detect unauthorized changes.

# Phishing Email Investigation

## Alert Information

| Field      | Value                                   |
| ---------- | --------------------------------------- |
| Date       | March 27, 2025                          |
| Time       | 19:25                                   |
| Severity   | Medium                                  |
| Alert Name | Email Marked as Phishing After Delivery |
| Status     | In Progress                             |
| Verdict    | True Positive (TP)                      |
| Category   | Phishing / Email Threat                 |

---

# Summary

A phishing email was detected after delivery to a user's mailbox. The email impersonated Microsoft Support and attempted to convince the recipient to open an attached report regarding a fake Microsoft Teams pricing increase.

The email contained multiple indicators associated with phishing activity, including failed authentication checks, brand impersonation, urgent language, and a suspicious compressed attachment.

---

# Email Details

| Field               | Value                                                 |
| ------------------- | ----------------------------------------------------- |
| Sender Display Name | Microsoft Support                                     |
| Sender Address      | [support@microsoft.com](mailto:support@microsoft.com) |
| Recipient           | Eddie Huffman                                         |
| Subject             | Important Update: Microsoft Teams Pricing Increase    |
| Attachment          | REPORT.rar                                            |

---

# Indicators Observed

| Indicator            | Finding                                     | Risk                                     |
| -------------------- | ------------------------------------------- | ---------------------------------------- |
| SPF Authentication   | Failed                                      | Sender identity could not be verified    |
| DKIM Authentication  | Failed                                      | Email authenticity could not be verified |
| Sender Impersonation | Microsoft branding used suspiciously        | Possible spoofing attempt                |
| Attachment           | REPORT.rar                                  | Potential malware delivery method        |
| Social Engineering   | Urgent message encouraging immediate action | User manipulation technique              |

---

# Investigation

## 1. Email Authentication Review

SPF and DKIM authentication results were reviewed.

### Findings:

* SPF authentication failed.
* DKIM authentication failed.

### Analysis:

The sender could not prove ownership of the sending domain. Failed authentication checks are common indicators of spoofed or unauthorized email activity.

---

## 2. Sender Analysis

The email claimed to originate from Microsoft Support.

### Observed:

* Trusted brand impersonation.
* Urgent business-related messaging.
* Request to open an attachment.

### Analysis:

Attackers commonly impersonate trusted organizations to increase the likelihood that users will interact with malicious content.

---

## 3. Attachment Analysis

Attachment:

```
REPORT.rar
```

### Risk:

Compressed archives are frequently used by attackers to deliver malicious files because they can contain:

* Executable files
* Scripts
* Malware payloads
* Obfuscated content

### Analysis:

The attachment should be submitted for sandbox analysis before execution.

---

# MITRE ATT&CK Mapping

| Technique                          | ID        | Description                                  |
| ---------------------------------- | --------- | -------------------------------------------- |
| Phishing: Spearphishing Attachment | T1566.001 | Malicious attachment delivered through email |
| Masquerading                       | T1036     | Impersonating a trusted organization         |

---

# Response Actions

## Completed Actions:

* Reviewed the phishing alert.
* Verified SPF and DKIM authentication results.
* Identified suspicious attachment indicators.
* Determined the email was likely malicious.
* Escalated the attachment for further analysis.

## Recommended Actions:

* Quarantine the phishing email.
* Search the environment for similar messages.
* Block malicious sender indicators if confirmed.
* Verify whether the recipient opened the attachment.
* Investigate endpoint activity if user interaction occurred.

---

# Final Verdict

## True Positive (TP)

### Reason:

The email was classified as a True Positive due to multiple phishing indicators:

* Failed SPF authentication.
* Failed DKIM authentication.
* Microsoft brand impersonation.
* Urgent social engineering tactics.
* Suspicious compressed attachment.

The email should be treated as a phishing attempt requiring remediation.

---

# Lessons Learned

* Authentication failures such as SPF and DKIM should be investigated alongside other indicators.
* Attackers frequently use trusted brands to increase phishing success rates.
* User interaction checks are important after malicious email delivery.
* Email security investigations require analyzing both technical indicators and human behavior.


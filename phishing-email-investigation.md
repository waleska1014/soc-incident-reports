# Phishing Email Investigation

## Alert Information

- **Date:** March 27, 2025
- **Time:** 19:25
- **Severity:** Medium
- **Alert:** Email Marked as Phishing After Delivery
- **Status:** In Progress

## Summary

A phishing email was identified after delivery to the recipient's mailbox. The email impersonated Microsoft Support and attempted to convince the recipient to open an attached report regarding a Microsoft Teams pricing increase.

## Email Details

| Field | Value |
|-------|-------|
| Sender | Microsoft Support <support@microsoft.com> |
| Recipient | Eddie Huffman <e.huffman@tryhackme.thm> |
| Subject | Important Update: Microsoft Teams Pricing Increase |
| Attachment | REPORT.rar |

## Indicators Observed

- SPF authentication failed.
- DKIM authentication failed.
- Sender attempted to impersonate Microsoft.
- Urgent language encouraged immediate action.
- Suspicious RAR attachment.

## Analysis

The failed SPF and DKIM authentication checks indicate the sender could not be verified. Combined with the urgent messaging and compressed attachment, the email displays several common phishing characteristics.

## Response

- Reviewed the alert.
- Verified email authentication results.
- Recommended quarantining the email.
- Checked for potential user interaction.
- Escalated the attachment for further analysis if necessary.

## Conclusion

The email was determined to be a likely phishing attempt based on failed authentication checks, sender impersonation, and the suspicious attachment.

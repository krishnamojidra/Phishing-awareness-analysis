# Phishing-awareness-analysis
Cyber security project-3 phishing awareness analysis 
📌 1. Objective

Technical firewalls cannot compensate for human error — 80% of security breaches involve phishing, and it takes attackers an average of just 82 seconds to get their first click in a simulated campaign. This project demonstrates the ability to manually detect, dissect, and triage phishing attempts without relying on automated tools, using pure analytical/security thinking.

Key Requirements Covered:


✅ Identify suspicious links or keywords
✅ List red flags found in phishing messages
✅ Explain why each message is unsafe
✅ Build a reusable triage checklist + decision tree



🧪 2. Sample Email Analysis

Three mock phishing emails were created (modeled on real-world attack patterns: display-name spoofing, Business Email Compromise, and QR-code/quishing) and analyzed below.


Sample 1 — Fake "Microsoft" Account Security Alert

Show Image

FieldObserved ValueDisplay NameMicrosoft Account TeamActual Sender Domainaccounts-secure-msft.com (not microsoft.com)Subject"URGENT: Unusual sign-in activity detected on your account"Suspicious Linkhxxp://micr0soft-login.verify-id.tech/authAttachmentAccount_Security_Report.html.scr

Red Flags Identified:


Sender-Domain Mismatch — display name says "Microsoft," but the real domain (accounts-secure-msft.com) does not belong to Microsoft. Classic display-name spoofing.
Urgency / Fear Trigger — "account will be LOCKED in 15 MINUTES" is artificial time pressure designed to bypass rational thinking.
Typosquatted / Homoglyph Link — micr0soft-login uses a zero in place of "o," and the real root domain is .verify-id.tech, not Microsoft's.
Dangerous Attachment — .html.scr is a double-extension trick; .scr is an executable screensaver file format frequently abused to deliver malware.


Why It's Unsafe: This message combines authority impersonation, manufactured urgency, a lookalike credential-harvesting domain, and a disguised executable payload — a textbook mass/credential phishing attempt.


Sample 2 — Fake "CEO" Wire Transfer Request (Business Email Compromise)

Show Image

FieldObserved ValueDisplay NameJohn Carter — CEOActual Sender Domaingmail.com (free webmail, not the company domain)Subject"STRICTLY CONFIDENTIAL – Urgent Wire Transfer"Suspicious Keywords"urgent," "confidential," "do NOT discuss," "bypass," "before 4 PM"

Red Flags Identified:


Free Webmail Impersonating an Executive — a real CEO would never email finance from a personal Gmail account.
Vague Subject + High Stakes — confidentiality framing discourages the recipient from verifying with anyone else.
Urgent Bypass Request — explicit instruction to skip the standard purchase-order/approval process — a major red flag per any internal control policy.
Isolation Tactic — "do NOT discuss this with anyone else on the team" is a grooming-style social engineering technique to prevent verification.
Artificial Deadline — "before 4 PM" manufactures urgency (fight-or-flight response).


Why It's Unsafe: This is a classic Business Email Compromise (BEC) / Whaling attack. It exploits Authority + Urgency + Secrecy — the same pattern used in the real-world Quanta Computer exploit, where a single spear-phishing attacker stole over $100M from Google and Facebook by spoofing a trusted vendor identity and bypassing finance checks.


Sample 3 — Fake Delivery Notice with Malicious QR Code (Quishing)

Show Image

FieldObserved ValueDisplay NameDelivery NotificationActual Sender Domainfedex-trackparcel.info (combosquatted domain)Subject"Your parcel could not be delivered – Action required"Suspicious ElementEmbedded QR code requesting payment

Red Flags Identified:


Combosquatted Domain — fedex-trackparcel.info adds delivery-related words to a legitimate brand name; real FedEx domains are fedex.com.
Fear of Loss — "parcel will be returned to sender" pressures quick action.
Unsolicited QR Code (Quishing) — forces the user onto an unmanaged mobile device, bypassing desktop URL/email filters and making the malicious domain harder to inspect.
Small "Low-Stakes" Payment Request — a $1.99 fee feels harmless, lowering the recipient's guard ("foot-in-the-door" psychology) before harvesting card details.


Why It's Unsafe: This is a Quishing (QR phishing) attack. Because the malicious link is hidden inside an image rather than clickable text, traditional email security filters and "hover-to-preview" habits are bypassed entirely.


🚩 3. Consolidated Red-Flag Reference Table

#Red FlagDescriptionSeen In1Sender–Domain MismatchDisplay name doesn't match the real "From" address domainSample 1, 32Lookalike / Typosquatted DomainMisspellings, homoglyphs, or combosquatting (e.g., micr0soft, fedex-trackparcel.info)Sample 1, 33Free Webmail Impersonating Staff/ExecsA real exec/internal team would not use Gmail/Yahoo for official requestsSample 24Urgency / Artificial Deadlines"15 minutes," "before 4 PM," "account locked"Sample 1, 2, 35Authority PressureImpersonating CEO, IT, or "Microsoft Support" to demand complianceSample 1, 26Secrecy / Isolation Requests"Don't discuss with anyone," "strictly confidential"Sample 27Bypass-Procedure RequestsAsking to skip normal approval/PO/security stepsSample 28Dangerous AttachmentsUnusual extensions like .scr, .js, .iso, or double extensionsSample 19Unsolicited QR Codes (Quishing)Forces mobile redirection, bypassing desktop filtersSample 310Fear/Greed TriggersThreats of loss (parcel returned) or rewards (refunds, prizes)Sample 311Mismatched/Suspicious LinksHovering reveals a URL different from the displayed text/brandSample 1


🌳 4. Phishing Triage Decision Tree

A non-expert employee should be able to follow this flow for any incoming suspicious message:

                    ┌────────────────────────┐
                    │  Incoming Suspicious   │
                    │        Email           │
                    └───────────┬────────────┘
                                │
                  ┌─────────────┼─────────────┐
                  ▼             ▼             ▼
              [ SAFE ]     [ SUSPICIOUS ]  [ MALICIOUS ]
            No red flags   1-2 minor red    3+ red flags /
            Known sender   flags, unclear   confirmed fake
            verified link  intent           domain or link
                  │             │             │
                  ▼             ▼             ▼
               CLOSE        WARN USER      BLOCK DOMAIN
            (no action     (flag email,    & ESCALATE
             needed)       advise caution,  (report to
                           do not click)    security team,
                                            purge from all
                                            inboxes)

Step-by-Step Checklist (Pause → Verify → Report)

StepAction1. PAUSERecognize the cognitive trigger (urgency, fear, authority, curiosity). Stop interacting with the message immediately. Apply the Five-Minute Rule — wait before acting.2. VERIFY• Check the actual sender address, not just the display name.
• Hover over (don't click) any links — read the URL right to left to find the true root domain.
• Confirm unusual requests through a second, out-of-band channel (e.g., call a known/directory phone number — never one provided in the suspicious message itself).3. REPORTUse the internal "Report Phishing" button/plugin. Do not simply delete the email — reporting allows the security team to purge the threat from all other inboxes and block the sending domain.

Quick Header/URL Audit SOP


Open full email headers (not just the preview).
Compare the From: and Return-Path domains — do they match the claimed organization?
Check for SPF/DKIM/DMARC authentication failures.
For any URL, identify the true root domain by reading from right to left (e.g., in www.decodelabs.tech.login-update.com, the real root is login-update.com, not decodelabs.tech).
Flag any unexpected file extensions (.scr, .js, .iso, .html disguised as .pdf).



🧠 5. Why These Attacks Work (Psychology Recap)

TriggerExploitsAuthorityUnquestioned compliance with perceived C-suite/IT/law enforcementUrgencyFight-or-flight response, reduced rational processingCuriosityDesire to fill a knowledge gap ("see what was said about you")Fear / GreedThreats of loss or promises of unearned reward


✅ 6. Conclusion

This project simulated the core duties of a SOC/security-awareness analyst: dissecting communication to separate legitimate messages from social-engineering attacks using analytical reasoning alone, before any enterprise-grade firewall is involved. The deliverables above — annotated sample analysis, a consolidated red-flag reference, and a reusable Pause-Verify-Report decision tree — form a portfolio-ready artifact demonstrating threat-identification and triage skills.


🎯 "The modern cybersecurity perimeter is no longer the network firewall. It is the user." — and this project is one brick in building that human firewall.

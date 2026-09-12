# IRCTC Booking Platform — Threat Model

STRIDE-based threat model of a ticket-booking platform (auth, booking API,
payment orchestration, legacy backend integration, admin portal), built in
OWASP Threat Dragon, with DREAD risk scoring applied to each finding.

## Architecture

<img width="1787" height="1495" alt="New STRIDE diagram (1)" src="https://github.com/user-attachments/assets/9b08f67c-85fa-4611-a800-d7d92e780dff" />


6 trust boundaries modeled: Internet↔DMZ, DMZ↔App Tier, App↔PRS Legacy,
App↔External Payment, App↔Data Tier, App↔Admin Interface.

Key components: Load Balancer + WAF + CAPTCHA, Auth Service, Search/Booking
API, Payment Orchestration Service, PRS Legacy System, 3rd-Party Payment
Gateway, User DB, Booking DB, Session Store, Audit Log Store, Admin/Agent
Portal.

## Summary

- **14 threats** identified across all 6 STRIDE categories
- Severity: **8 High**, 5 Medium, 1 Low
- Risk-scored using **DREAD** (Damage, Reproducibility, Exploitability,
  Affected Users, Discoverability)

## Top Findings (by DREAD average)

| Threat | STRIDE Category | DREAD Avg | Description |
|---|---|---|---|
| T2 – Tatkal Window DoS via Bot Flooding | Denial of Service | 8.0 | Automated bots fire booking requests at millisecond precision during high-demand windows, exhausting inventory for legitimate users |
| T4 – IDOR on Booking/Profile Data | Information Disclosure | 7.8 | Missing server-side authorization on object reference (PNR/booking ID) allows enumeration of other users' bookings/PII |
| T1 – Credential Stuffing / Account Takeover | Spoofing | 7.4 | No MFA or lockout policy; leaked-credential lists used against the login endpoint |
| T3 – CAPTCHA Bypass / Scalping | Elevation of Privilege | 7.2 | Headless browser automation / CAPTCHA-solving services bypass anti-bot controls for unfair inventory access |
| T6 – Payment Status Tampering | Tampering | 6.6 | Client-side/weakly-signed payment callback trusted without server-to-server verification |

Full 14-threat table with descriptions and mitigations is in the PDF report
and the model file below.

## Methodology

1. Reconnaissance and DFD construction (trust boundaries, processes, data
   stores, data flows)
2. STRIDE categorization per element and flow
3. DREAD scoring per identified threat
4. Mitigation mapping for every open finding

## Files

- [`ThreatDragonModels/IRCTC-Booking-Platform-Threat-Model/IRCTC-Booking-Platform-Threat-Model.json`](ThreatDragonModels/IRCTC-Booking-Platform-Threat-Model/IRCTC-Booking-Platform-Threat-Model.json) — open directly in [Threat Dragon](https://www.threatdragon.com/) to view/edit the live diagram
- `IRCTC-Booking-Platform-Threat-Model.pdf` — full exported report (all 14 threats, descriptions, mitigations)

## Tools

OWASP Threat Dragon

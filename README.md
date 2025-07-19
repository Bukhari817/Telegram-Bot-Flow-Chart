# User Onboarding & Lead Distribution Flow

```mermaid
flowchart TD
    Start([START])
    Start --> A1[User Enters Bot]
    A1 --> A2[/start command (handle_start_command)]
    A2 --> A3[Parse Payload, Set Role, Clear State]
    A3 --> A4[Is Partner? -> Yes → partner_select_tier | No → investor_agreement]
    A4 --> A5[Bot Asks: “Please select your role”]

    A5 --> B1[Investor]
    A5 --> B2[Agent]
    A5 --> B3[Developer]

    %% ----- Developer Path (mirrors Agent) -----
    B3 --> C1[Developer Flow (Same as Agent)]
    C1 --> C2[Success Fee Agreement (5–7%)]

    %% ----- Agent Path -----
    B2 --> D1[user_state[role] = "Real Estate Agent (licensed)"]
    D1 --> D2[Partner Tier Selection]
    D2 --> D3[Standard: $120 | Verified: $150 | Institutional: $300]
    D3 --> D4[User Selects Tier]
    D4 --> D5[Validate Tier → Save to user_state]
    D5 --> D6[Partner Payment Confirmation]
    D6 --> D7[Upload Photo → Save to Drive]
    D7 --> D8[Generate Invoice → Save ID]
    D8 --> D9[Partner Category Selection]
    D9 --> D10[User Selects Category]
    D10 --> D11[Partner Email → Validate → Save]
    D11 --> D12[License Upload → Save to Drive]
    D12 --> D13[Trigger DocuSign: Success Fee Agreement (30%)]
    D13 --> D14[Admin Review: Approve / Modify / Reject]
    D14 --> D15[Agent Signs “Signed”]
    D15 --> D16[Save PDF → CRM Update → Email Agent]
    D16 --> D17[Partner Registration Complete → partner_code]
    D17 --> D18[user_state Cleared]

    %% ----- Investor Path -----
    B1 --> E1[user_state[step] = investor_agreement]
    E1 --> E2[Bot: Reply “I Agree”]
    E2 --> E3[Validate → Step = referral]
    E3 --> E4[Bot Asks: Referral Code]
    E4 --> E5[Save Code or “None”]
    E5 --> E6[Investor Email → Validate → Save]
    E6 --> E7[Phone Number → Validate → Save]
    E7 --> E8[Role Selection → “Investor”]
    E8 --> E9[Region Selection]
    E9 --> E10[Purpose Selection]
    E10 --> E11[Budget Selection]
    E11 --> E12[Investment Type Selection]
    E12 --> E13[Notes]
    E13 --> E14[Save to LEADS → Create CRM Deal]
    E14 --> E15[KYC: Upload ID/POF → Save to Drive]
    E15 --> E16[Admin Notified for KYC]
    E16 --> E17[Auto Funnel Assignment (≥€500K = Premium)]
    E17 --> E18[Activate 3-Day Trial (One-Time per Telegram ID)]
    E18 --> E19[Trigger DocuSign: Referral Agreement (2–3%)]
    E19 --> E20[Save PDF → Email → CRM Update]
    E20 --> E21[Payment Flow (JCC)]
    E21 --> E22[Webhook or Manual Approval]
    E22 --> E23[Teaser Delivery & Access Control]

    %% ----- Teaser Watermarking -----
    E23 --> F1[Filter Teasers: Country, Budget, Type]
    F1 --> F2[Watermark Generated:
        - Full Name
        - Email
        - Access Date
        - Unique ID]
    F2 --> F3[Log Views to Google Sheets]
    F3 --> F4[Admin Notified]

    %% ----- Partner Property Submission -----
    D18 --> G1[Partner Uploads Property → Bot or Form]
    G1 --> G2[Admin Creates Teaser → Adds to Project Pool]

    %% ----- Lead Distribution -----
    G2 --> H1[Assign Lead: Region, Budget, Purpose]
    H1 --> H2[Update LEADS & Pipedrive]
    H2 --> H3[Agent Notified → 60min Deadline]

    %% ----- Admin Panel -----
    H3 --> I1[Admin Panel Features:
        - Approve/Reject Commission
        - Hide Leads
        - Upload Teasers
        - Edit Templates
        - Pause Delivery
        - View Logs
        - Add Countries]
    I1 --> I2[Admin Notified of All Actions]

    %% ----- Lead Timeout -----
    I2 --> J1[Check Every 5 min]
    J1 --> J2[If Unaccepted > 60 min:
        - Mark Timeout
        - +1 Missed Leads
        - ≥3 Missed = Disqualified
        - Pipedrive → LOST
        - Notify Admin]

    J2 --> End([END OF FLOW])
```

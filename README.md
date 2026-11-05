# Make.com — Restaurant QR Loyalty & Email Marketing System

A Make.com automation system that turns a QR code scan into a full customer loyalty loop — from registration and discount delivery, through GDPR-compliant unsubscribe, to personalized promotional campaigns.

---

## The Problem It Solves

Restaurants want to build a direct customer database and run email promotions, but the signup process is usually friction-heavy. Paper forms get lost, QR codes that just open a website don't capture data, and sending personalized emails manually doesn't scale.

This system makes it frictionless: customer scans a QR code, fills in a 30-second Tally form, and immediately receives a discount code by email. The restaurant owner can later send targeted promotions to the entire subscriber list with one click — personalized per favorite dish, filtered by active GDPR consent.

---

## How It Works

The system consists of three Make.com scenarios sharing one Airtable database:

### Scenario 1 — Registration & Discount Code

```
Customer scans QR code at the restaurant
        ↓
Fills in Tally form (name, email, favorite dish, feedback, GDPR consent)
        ↓
Make checks Airtable for duplicate email
        ↓
New customer → record saved to Airtable
        ↓
Discount email sent (free item, unique code number, valid 24h)

Returning customer → "You already have your code" email sent
```

### Scenario 2 — Unsubscribe (Marketing Consent Withdrawal)

```
Customer clicks unsubscribe link in discount email
        ↓
Webhook receives email parameter
        ↓
Make finds matching record in Airtable
        ↓
Sets Marketing Consent field to false
```

### Scenario 3 — Promotional Email Campaign

```
Restaurant owner triggers scenario manually (or on schedule)
        ↓
Fetch all customer records from Airtable
        ↓
Filter: Marketing Consent = true only
        ↓
Send personalized email with 10% discount on their favorite dish
```

---

## Data Collected (Airtable)

| Field | Type | Description |
|-------|------|-------------|
| First Name | Text | Customer first name |
| Email for Discount Code | Email | Customer email address |
| Favorite Dish | Select | Chosen from restaurant menu options |
| What do you like and what could we improve? | Text | Open feedback |
| What is missing from the local food scene? | Text | Open feedback |
| Marketing Consent | Boolean | GDPR consent flag |
| Code Number | Number | Auto-assigned discount code number |
| Submitted At | Date | Form submission timestamp |

---

## Tech Stack

| Component | Technology |
|-----------|-----------|
| Automation | Make.com |
| Form / QR trigger | Tally |
| Database | Airtable |
| Email | Zoho Mail |

---

## File Structure

```
scenario_1_registration_and_discount_code.json    — Registration & discount code
scenario_2_marketing_consent_withdrawal.json      — Unsubscribe / consent withdrawal
scenario_3_promotional_email_campaign.json        — Promotional email campaign
```

---

## Setup

1. **Airtable** — create a base with the fields listed above; set `Code Number` as an auto-number field
2. **Tally** — create a form with matching fields and connect its webhook to Scenario 1
3. **Zoho Mail** — connect your account in Make.com
4. **Make.com** — import all 3 JSON files as separate scenarios
5. Replace the following placeholders in each scenario:

| Placeholder | Where to find it |
|-------------|-----------------|
| `YOUR_AIRTABLE_BASE_ID` | Airtable base URL |
| `YOUR_AIRTABLE_TABLE_ID` | Airtable table URL |
| `YOUR_AIRTABLE_CONNECTION_ID` | Make.com → Connections |
| `YOUR_ZOHOMAIL_CONNECTION_ID` | Make.com → Connections |
| `YOUR_ZOHO_ACCOUNT_ID` | Zoho Mail account settings |
| `YOUR_TALLY_WEBHOOK_ID` | Make.com → Tally webhook |
| `YOUR_UNSUBSCRIBE_WEBHOOK_ID` | Make.com → Custom webhook (Scenario 2) |
| `YOUR_UNSUBSCRIBE_WEBHOOK_TOKEN` | URL of the Scenario 2 webhook |
| `[RESTAURANT_NAME]` | Your restaurant name |
| `your-email@yourdomain.com` | Your sending email address |

6. Activate Scenarios 1 and 2 — they run on triggers automatically
7. Run Scenario 3 manually whenever you want to send a promotional campaign


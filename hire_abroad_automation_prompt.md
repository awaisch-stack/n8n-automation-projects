# Automation Build Prompt — Hire Abroad Community Funnel

Use this prompt with an AI assistant (or as your own build spec / n8n workflow brief) to build the complete automation.

---

## PROMPT

I need to build a complete automation funnel in n8n (or an equivalent automation tool) for a community business called "Hire Abroad". Here is the full flow and requirements:

### Business context
- The client runs a paid community called "Hire Abroad" that helps people find jobs abroad.
- Leads currently come through Instagram DMs.
- Membership costs ₹999 (one-time or recurring — confirm with client) and grants access to a private Telegram group.
- Everything is currently done manually and needs to be fully automated.

### Funnel to build

**1. Instagram lead capture**
- Trigger: New DM/comment on Instagram Business account.
- Auto-reply with a short intro message about the Hire Abroad community.
- Send a link to a video/webinar explaining the community and its benefits.
- Log the lead (name, Instagram handle, timestamp, source) into a Google Sheet / Airtable "Leads" table.

**2. Webinar / video engagement tracking**
- Track whether the lead opened/watched the video or registered for a live webinar.
- If registered for a live webinar: send a reminder message 1 hour before and 10 minutes before it starts.
- After the webinar/video: send a follow-up message with the payment link.
- If no action within 24–48 hours: send one automated follow-up nudge message.

**3. Payment (Razorpay)**
- Generate a Razorpay payment link for ₹999 automatically for each qualified lead.
- Listen for the Razorpay payment webhook (payment.captured event).
- On successful payment, mark the lead as "Paid" in the Leads sheet with payment date and transaction ID.
- On failed/abandoned payment, send a gentle reminder after a few hours.

**4. Telegram access**
- On confirmed payment, auto-generate a unique, single-use (or time-limited) Telegram invite link via the Telegram Bot API.
- Send this invite link to the customer via Instagram DM or WhatsApp/Email (whichever channel is available).
- Track which Telegram user ID joined using which invite link, and store it against the lead record.

**5. Welcome message**
- When the new member joins the Telegram group, auto-send a welcome message (group rules, pinned resources, how to get support).

**6. Renewal / expiry management**
- Store each member's join date and membership expiry date (if it's a recurring/monthly access model — confirm with client).
- X days before expiry, send an automated renewal reminder with a new payment link.
- If not renewed by expiry date, automatically remove/ban the member from the Telegram group and update their status in the sheet.

**7. Lead tracking dashboard**
- Maintain a single source-of-truth sheet/database with columns: Name, Instagram handle, Date of first contact, Webinar attended (Y/N), Payment status, Payment date, Telegram joined (Y/N), Membership expiry date, Renewal status.
- This should update automatically at every step above — no manual entry.

### Technical requirements
- Build this in n8n (cloud or self-hosted).
- Use Razorpay's official webhook for payment confirmation — do not rely on polling.
- Use Telegram Bot API for invite generation, join tracking, and member removal.
- Use Instagram's Messaging API (via a Business/Creator account connected to a Facebook Page) — flag if a third-party layer like Manychat is needed due to Meta's API restrictions.
- Store all lead/member data in Google Sheets or Airtable (specify which).
- Add a scheduled/cron trigger for the renewal-check step (e.g., runs daily to check upcoming expiries).
- Include basic error handling: if any step fails (e.g., Telegram API error), log the failure and notify the admin via a Slack/Telegram alert instead of failing silently.

### Deliverable
Provide:
1. A visual workflow diagram of all connected nodes.
2. The working n8n workflow (exported .json) with all credentials placeholders clearly marked.
3. A short setup guide listing exactly which accounts/API keys/permissions the client needs to provide (Instagram Business login, Razorpay API keys, Telegram bot token, Google Sheet/Airtable access).
4. A brief test walkthrough showing one full lead going through the entire funnel successfully.

---

*Tip: Paste this prompt into Claude, ChatGPT, or directly use it as your own n8n build checklist. Fill in the confirm-with-client items (recurring vs one-time payment, notification channel for invite links) before starting the build.*

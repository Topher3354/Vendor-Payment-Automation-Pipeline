# Use Case 07 — Vendor Payment Automation Pipeline

**Category:** Finance & Payments
**Author:** Christopher Ayodeji Ojo — [Trublshut](https://whop.com/trublshut)
**Tools:** Microsoft Power Automate · Microsoft Power Apps · REST API (payment gateway) · SharePoint

---

## 🔴 The Problem

Finance teams manually process vendor payment requests — collecting details, chasing approvals, entering data into payment systems, and sending confirmations. Every step is manual, slow, and error-prone.

A single payment request touches multiple people across multiple systems. Data gets re-entered at each step, creating opportunities for error. Approvals get lost in email chains. Vendors chase Finance for confirmation. And Finance has no clean audit trail when questions arise later.

---

## ✅ The Solution

An end-to-end vendor payment pipeline where the vendor or internal requester submits via a structured Power Apps form, the system validates the data, routes for approval, triggers the payment via API, and sends a confirmation — with zero manual data entry at any step.

---

## ⚙️ How It Works

```
Vendor / requester submits payment request (Power Apps form)
        ↓
Power Automate triggered on form submission
        ↓
Validate: bank details format, invoice number, amount range
        ↓
Route to Finance approver (Approvals connector — Teams card)
        ↓
Approver reviews + approves or rejects in Teams
        ↓
On approval: call payment gateway REST API with validated data
        ↓
Receive payment confirmation + reference number from API
        ↓
Update payment status in SharePoint payment tracker
        ↓
Send payment confirmation to vendor via email (with reference)
        ↓
Log full transaction record: requester, approver, amount, timestamp, reference
```

---

## 🛠️ Tech Stack

| Tool | Role |
|------|------|
| Microsoft Power Apps | Payment request form (structured, validated input) |
| Microsoft Power Automate | Workflow orchestration + approvals |
| REST API (payment gateway) | Payment execution + confirmation |
| SharePoint | Payment tracker + audit log |
| Microsoft Teams | Approval cards for Finance team |

---

## 🔧 Key Logic

- **Form validation in Power Apps:** bank account number format check, invoice number uniqueness check against existing SharePoint records, amount within pre-approved vendor range
- **Pre-approved vendor list:** SharePoint lookup table — submission against an unlisted vendor triggers a secondary approval step
- **Approval routing:** amounts below threshold → single Finance Officer approval; above threshold → Finance Manager approval
- **Rejection handling:** if rejected, vendor / requester receives email with rejection reason; record logged as rejected with approver comment
- **Payment API call:** structured JSON payload built from form data — no manual re-entry
- **Idempotency check:** invoice number checked against payment log before API call to prevent duplicate payments
- **Confirmation record:** payment reference stored in SharePoint + attached to original request

---

## 📊 Outcomes

- ✅ Payment processing time significantly reduced — no manual handoffs
- ✅ Zero manual data entry errors — data captured once at source
- ✅ Full approval chain documented for audit and compliance
- ✅ Vendor confirmations sent automatically — no manual follow-up
- ✅ Duplicate payment risk eliminated via invoice number deduplication

---

## 📁 How to Use This

1. Build a Power Apps form with fields: vendor name, bank account, sort code, invoice number, amount, invoice attachment, cost centre
2. Add client-side validation in Power Apps for required fields and format checks
3. On submission, Power Automate triggers — perform SharePoint lookup to validate vendor and check for duplicate invoice number
4. Create an Approval action in Power Automate (Teams-based) routed to the appropriate Finance approver based on amount
5. On approval, construct the payment API JSON payload from form fields and call your payment gateway's REST API
6. Parse the API response for payment reference and status
7. Update the SharePoint payment tracker row with status + reference
8. Send confirmation email to vendor using the Outlook connector

---

## 🔗 Related Use Cases

- [Use Case 08 — Staff Airtime & Incentive Disbursement (DAA Portal)](./UC-08-Staff-Airtime-Incentive-DAA-Portal.md)
- [Use Case 09 — Invoice Processing & Approval Automation](./UC-09-Invoice-Processing-Approval.md)

---

*© 2026 Christopher Ayodeji Ojo · Trublshut AI Automation · christopherayodeji131@gmail.com*

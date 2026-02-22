# Zero-Touch Lead Pipeline & Smart Routing

## 📉 The Business Problem: The "Speed-to-Lead" Bottleneck
In B2B sales, conversion rates drop by 400% if a lead is not contacted within the first 5 minutes. 
However, standard pipelines rely on manual triage: a prospect fills out a form, and an expensive sales representative spends 10-15 minutes researching the company on LinkedIn/Google to determine if they are a qualified target. 
This manual "human middleware" creates a critical bottleneck, delays response times, and wastes high-value sales hours on basic data entry.

## 🏗️ The Architectural Solution: Event-Driven Enrichment
This repository demonstrates a **Zero-Touch Lead Pipeline** built in n8n. 

It replaces manual research with automated, instant API enrichment. The architecture intercepts the inbound lead in real-time, queries a firmographic database to assess company size/revenue, and utilizes conditional logic to route the prospect to the appropriate sales channel. 

Enterprise leads trigger instant push notifications to Senior Account Executives, while standard leads are silently routed to the CRM queue.

### Core Benefits:
* **Maximized Speed-to-Lead:** High-ticket prospects are identified and routed to a human closer in under 800 milliseconds.
* **Zero Manual Research:** Sales reps receive fully enriched profiles (employee count, sector, revenue) without leaving their communication tools (Slack/Teams).
* **Automated Triage:** Removes subjectivity and human error from the lead qualification process.

---

## 🗺️ Workflow Logic

### 1. Data Ingestion (Webhook)
* Acts as the universal entry point. Captures raw JSON payloads from any frontend form (Typeform, Webflow, WordPress, custom landing pages).

### 2. Data Enrichment (HTTP Request)
* Extracts the domain from the prospect's email and queries an external firmographic API (e.g., Clearbit, Apollo).
* Appends missing business data to the payload (e.g., `employee_count`, `industry`, `annual_revenue`).

### 3. Conditional Router (Switch Node)
* Evaluates the enriched data against predefined business rules.
* **Branch A (High-Ticket):** If `employee_count > 50`.
* **Branch B (Standard):** If `employee_count <= 50`.

### 4. Execution & Notification
* **VIP Lane:** Triggers a real-time Slack/Teams alert to the Enterprise Sales channel with actionable data, and upserts the CRM record with a "High Priority" flag.
* **Standard Lane:** Silently upserts the CRM record and assigns it to a junior queue or an automated nurturing campaign.

<img width="1664" height="465" alt="image" src="https://github.com/user-attachments/assets/f929533f-2959-40c5-ade0-86981f2077cf" />

---

## ⚙️ How to Implement
1. Import the `zero-touch-lead-pipeline.json` into your n8n instance.
2. Update the Webhook URL in your frontend form provider.
3. Configure the HTTP Request node with your preferred enrichment API credentials (Bearer Token/API Key).
4. Adjust the Switch node parameters to match your company's specific ICP (Ideal Customer Profile) thresholds.
5. Connect your specific CRM and communication nodes (e.g., HubSpot, Salesforce, Slack) at the end of the branches.

# ShopTalk AI – Voice Agent Call Summary & Lead Capture (n8n Workflow)

A production-ready asynchronous backend infrastructure pipeline built on **n8n**. This workflow captures end-of-call webhooks dispatched from an autonomous AI voice agent platform (**Vapi**), dynamically splits data payloads based on structure, routes concise call logs into corporate messaging networks, and updates structured sales data matrices.

![Workflow Canvas](assets/call_canvas.jpeg)

## 🚀 Architectural Design & Data Ingress Routing

The automation engine operates seamlessly with a single webhook receiver and conditional evaluation rules:

1. **Webhook Data Ingress Layer:** Acts as a centralized endpoint monitoring structured event payloads pushed instantly upon call termination from the voice agent routing workspace.
2. **Dynamic Conditional Splitting:** Employs a custom data schema evaluator to analyze JSON parameters. The `Identify summary or Leads` conditional block safely separates operational data logs from incoming payload parameters:
   * **True Pathway (Operational Summaries):** Executes if an abstract analysis field exists inside the webhook data payload. It extracts raw multi-line strings and streams them straight into targeted corporate **Slack channels** for immediate team observation.
   * **False Pathway (CRM Lead Capture Data):** Filters customer detail arrays (`customer.number`, `structuredData.name`, `email`, and `intent`). This values are mapped dynamically to write-appends on a centralized tracking sheet.

---

## 🔧 Component Dependencies

To run or replicate this integration pipeline, you will require active accounts and authorization keys for the following services:

* **n8n Instance:** Self-hosted docker engine or cloud workspace environment.
* **Vapi Account Platform:** Configured voice assistants with webhooks pointed toward your n8n target endpoint URL.
* **Google Workspace Account:** Access to Google Sheets for structured customer data tracking.
* **Slack App Workspace:** Authorized Slack token credentials supporting message delivery integrations.

### Google Sheets CRM Setup
Ensure your tracking spreadsheet contains an active sheet named `Sheet1` with these explicit structural header assignments in the top row:
* `caller-phone`
* `name`
* `email`
* `intent`

---

## 💻 Installation & Orchestration Steps

1. Create a blank workflow canvas inside your n8n configuration environment.
2. Select **Import from File** from the top right context menu and upload the `workflow/vapi_call_summary_leads.json` file found in this repository.
3. Access the `Monitor vapi calls` webhook node, copy your production-ready URL address string, and register it inside your Vapi dashboard as the designated destination for call end status notifications.
4. Authenticate your target Google Account inside the `Add leads into sheet` spreadsheet processing node.
5. Authorize your Slack Workspace token inside the `send call_summary` notification gateway block.
6. Toggle your workflow from inactive to **Active**.

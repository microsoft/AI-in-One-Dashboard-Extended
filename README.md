# ℹ️ **Public Preview: Agent Dashboard in Copilot Analytics**
The Agent Dashboard is now in public preview, providing one-click visibility into agent usage. Work with your IT admin to enable it.



# 🤖 AI-in-One Dashboard — Extended Version

<p style="font-size:small; font-weight:normal;">
This repository contains the <strong>AI-in-One Dashboard — Extended Version</strong> Power BI template. This report provides comprehensive insights into Microsoft Copilot and Agent adoption, empowering AI and business leaders to make informed decisions regarding AI implementation, licensing, and enablement strategies. The Extended Version adds three optional data sources — <strong>Copilot credit consumption</strong>, <strong>product feedback</strong>, and <strong>Copilot Studio agent conversation transcripts</strong> — for deeper agent health, cost, and user sentiment analysis.
</p>

> 📦 **Looking for the standard version?** See the base [AI-in-One Dashboard](https://github.com/microsoft/AI-in-One-Dashboard) repository.

---

## 📸 Dashboard Preview

See the dashboard in action:

![AI-in-One Dashboard animated preview](Images/AIO%20v10%20Gif.gif)

---

<details>
<summary>⚠️ <strong>Important usage & compliance disclaimer</strong></summary>

Please note:

While this tool helps customers better understand their AI usage data, Microsoft has **no visibility** into the data that customers input into this template/tool, nor does Microsoft have any control over how customers will use this template/tool in their environment.

Customers are solely responsible for ensuring that their use of the template tool complies with all applicable laws and regulations, including those related to data privacy and security.

**Microsoft disclaims any and all liability** arising from or related to customers' use of the template tool.

**Experimental Template Notice:**
This is an experimental template with audit logs as the primary source. The audit logs from Microsoft Purview are intended to support security and compliance use cases. While they provide visibility into Copilot and Agent interactions, they are not intended to serve as the sole source of truth for licensing or full-fidelity reporting on Copilot or Agent activity. For the most accurate and reliable usage insights, users are encouraged to refer to data from the Microsoft 365 Admin Center and Viva Insights. Currently available in English only.

</details>

---

## 📊 What This Dashboard Provides

- **Comprehensive visibility into M365 Copilot, unlicensed Copilot Chat, and Agent usage** across your organization
- **User engagement tracking over time** to identify adoption patterns and trends across all Copilot surfaces
- **Data-driven insights** to optimize AI investments, license allocation, and employee enablement
- **Customizable views** to segment data by department, role, or other organizational dimensions
- **Extended analytics** (optional): Copilot credit consumption, user product feedback, and Copilot Studio agent conversation transcript analysis

---

## 🚀 How This Helps Leaders

- **Make informed AI and Microsoft Copilot investment decisions** using comprehensive usage data and analytics consolidated in one place
- **Identify Copilot and Agent adoption champions** and areas needing additional enablement
- **Optimize enablement and change management efforts** based on actual usage patterns across M365 Copilot, unlicensed Copilot Chat, and Agents
- **Accelerate AI readiness, adoption, and impact** across the organization—from licensed Copilot experiences to emerging Agent capabilities
- **Understand cost and sentiment** through optional credit consumption and product feedback data layers

---

## ✅ What You'll Do

**Quick Overview**: Export 4 core data sources → optionally add up to 3 extended sources → Connect to Power BI → Analyze your AI adoption

### Choose Your Method

<details>
<summary>🖱️ Option A: Manual Export via Web Portal (Recommended for first-time setup)</summary>

Follow the traditional workflow using browser-based portals to export your data:

1. **Export Copilot audit logs** from Microsoft Purview
2. **Download licensed user data** from Microsoft 365 Admin Center
3. **Export org data** from Microsoft Entra Admin Center
4. **Connect CSV files** to Power BI template

**Optional extended sources** (see [Extended Data Sources](#-optional-extended-data-sources) below):
- Export **Copilot credit consumption** from Microsoft 365 Admin Center
- Export **product feedback** from Microsoft Admin Center (Health)
- Export **Copilot Studio conversation transcripts** via Power Automate or Power Apps

**Best for**: One-time setup, first-time users, or those who prefer GUI-based workflows

👉 **See detailed instructions below** in the [Detailed Steps](#-detailed-steps) section

</details>

<details>
<summary>⚡ Option B: Automated PowerShell Scripts (For regular refreshes)</summary>

Use the PowerShell automation scripts in the [scripts](scripts/) folder for a faster, repeatable workflow. This method supports two execution modes:

- Run locally (PowerShell) and export CSVs
- Run in Azure Automation (runbooks) and upload outputs to SharePoint

**Advantages**:
- ✅ Automated data export via Microsoft Graph API
- ✅ Reduced manual steps and potential errors
- ✅ Easy to schedule for regular data refreshes
- ✅ Consistent results every time

**Prerequisites**:
- PowerShell 5.1 or later
- Microsoft Graph PowerShell modules
- Appropriate permissions (same as manual method)

**Quick Start (Local execution)**:
~~~powershell
# 1. Install required modules
Install-Module Microsoft.Graph.Beta.Security -Scope CurrentUser

# 2. Navigate to scripts folder and run
cd scripts
.\create-query.ps1              # Creates audit log query
.\get-copilot-interactions.ps1  # Exports query results
.\get-copilot-users.ps1         # Exports licensed users list
~~~

**Quick Start (Azure Automation execution)**:
~~~powershell
cd scripts/automation
.\deploy.ps1
~~~

📖 **Documentation**:
- Local scripts: [scripts/readme.md](scripts/readme.md)
- Azure Automation runbooks: [scripts/automation/README.md](scripts/automation/README.md)

</details>
---

## 📁 Detailed Steps

<details>
<summary>🔍 Step 1 (skip if using 'Option B'): Download Copilot Interactions Audit Logs (Microsoft Purview)</summary>

### What This Data Provides
This log provides detailed records of Copilot interactions across all surfaces (Chat, M365 apps, Agents), as well as interactions with **third-party and custom-built AI applications** (e.g., Confluence Cloud, Jira Cloud, Miro), enabling deep analysis of usage patterns and engagement across the full AI landscape.

### Requirements
- Access level required: **Audit Reader** or **Compliance Administrator**
- Portal: Microsoft Purview Compliance Portal
- Permissions needed: View and export audit logs

### Step-by-Step Instructions

1. **Navigate to the portal**
   - Go to: [security.microsoft.com](https://security.microsoft.com)
   - In the left pane, scroll down and click **Audit**
   - Ensure you have appropriate compliance roles (e.g., **Audit Reader**). If not, contact your IT admin

2. **Configure the audit search**
   - In **Activities > Friendly Names**, select:
     - `Copilot Activities – Interacted with Copilot` *(required)* — M365 Copilot interactions across all surfaces (RecordType: CopilotInteraction)

   - **Recommended**: Also select these two additional activities to capture **third-party and custom AI app** usage:
     - `Copilot Activities – Interacted with a Connected AI App` — Custom-built Copilots and registered 3rd-party AI apps that your org has deployed (RecordType: ConnectedAIAppInteraction)
     - `Copilot Activities – Interacted with an AI App` — Non-Microsoft 3rd-party AI apps accessed via Microsoft 365, even if not formally deployed in your org (RecordType: AIAppInteraction)

   - Set a **Date Range** (recommended: 1–3 months to match your Viva query)
   - Give your search a descriptive name (e.g., "Copilot Audit Export - Oct 2025")

   > 💡 **Why include the extra activities?**
   > The standard `Interacted with Copilot` activity only captures M365 Copilot usage. As organisations adopt third-party agents and custom Copilots (e.g., Confluence Cloud, Jira Cloud, Miro), these interactions are logged under separate record types. Including them gives you a **complete picture of AI adoption** — not just Microsoft Copilot, but the full ecosystem of AI tools your users are engaging with.

   > ⚠️ **Pay-as-you-go billing for "Interacted with an AI App" (AIAppInteraction / RT405):**
   > This third activity type uses **Microsoft Purview pay-as-you-go (PAYG) billing** and is **not** included in standard Audit (Standard) or Audit (Premium) subscriptions. To enable it:
   > 1. An admin must set up [pay-as-you-go billing](https://learn.microsoft.com/en-us/purview/audit-copilot#auditing-for-non-microsoft-ai-applications) in Microsoft Purview, which bills based on the volume of audit records generated
   > 2. Once enabled, Purview begins logging interactions with non-Microsoft AI applications
   > 3. Costs are consumption-based — you only pay for the records actually generated
   >
   > **If PAYG is not enabled**, selecting this activity will simply return no results — the first two activities will still work normally. You can always add this later without re-exporting the other data.
   >
   > The second activity (`Interacted with a Connected AI App`) does **not** require PAYG — it is included with your existing Audit subscription and covers custom-built Copilots and registered 3P apps.

3. **Run and export the search**
   - Click **Search**
   - Wait until the status changes to **Completed**
   - Click into the completed search
   - Select **Export > Download all results**
   - Save the CSV file to a known location (e.g., `C:\Data\Copilot_Audit_Logs.csv`)

### Expected File Format
- **File format**: CSV
- **Typical size**: Varies widely (5 MB–500 MB depending on org size and activity)
- **Columns**: ~50+ columns including timestamps, user IDs, activity types, surfaces
- **Rows**: One row per Copilot interaction

📖 **Learn more**: [Export, configure, and view audit log records – Microsoft Learn](https://learn.microsoft.com/en-us/microsoft-365/compliance/audit-log-search)

</details>

<details>
<summary>👤 Step 2 (skip if using 'Option B'): Download Copilot Licensed User List (Microsoft 365 Admin Center) </summary>

### What This Data Provides
This data provides a list of users with Copilot licenses, enabling you to track license utilization and identify licensed vs. unlicensed usage patterns.

### Requirements
- Access level required: **Global Administrator** or **Reports Reader**
- Portal: Microsoft 365 Admin Center
- Permissions needed: View usage reports

### Step-by-Step Instructions

1. **Navigate to the portal**
   - Go to: [admin.microsoft.com](https://admin.microsoft.com)
   - Log in as a **Microsoft 365 Global Administrator** or **Reports Reader**

2. **Unhide usernames** (if concealed)
   - Go to **Settings > Org Settings**
   - Under the **Services** tab, choose **Reports**
   - **Deselect**: "Display concealed user, group, site names in all reports"
   - Click **Save changes**

3. **Navigate to Copilot reports**
   - Go to: **Reports > Usage > Microsoft 365 Copilot**
   - Click on the **Readiness** tab

4. **Export license data**
   - Scroll to **Copilot Readiness Details** section
   - Ensure the column `Has Copilot license assigned` is visible
   - Click the ellipsis (`...`) menu
   - Choose **Export** to download the file as CSV
   - Save to a known location (e.g., `C:\Data\Copilot_Licensed_Users.csv`)

### Expected File Format
- **File format**: CSV
- **Typical size**: 1–10 MB for 1,000–10,000 users
- **Columns**: ~10–15 columns including UserPrincipalName, Department, LicenseStatus, LastActivityDate
- **Rows**: One row per user in your organization

📖 **Learn more**: [Microsoft 365 Copilot Readiness Report – Microsoft Learn](https://learn.microsoft.com/en-us/microsoft-365/admin/activity-reports/microsoft-365-copilot-readiness)

</details>

<details>
<summary>🤖 Step 3: Export Agent 365 Data (Microsoft Admin Center)</summary>

### What This Data Provides
This file provides a catalogue of agents available in your tenant via the Agent 365 platform in the Microsoft Admin Center (MAC), enabling analysis of agent provisioning, availability, and adoption across your organization.

### Requirements
- Access level required: **Global Administrator** or **Reports Reader**
- Portal: Microsoft Admin Center (MAC)
- Permissions needed: Access to Agent 365 / Agent Inventory

### Step-by-Step Instructions

1. **Navigate to the Agent 365 section**
   - Go to: [admin.microsoft.com](https://admin.microsoft.com)
   - In the left navigation, go to **Agents**
   - You should see the **Agents Overview**
   - This displays all agents: their availability status, templates applied, and assigned users/sources

3. **Export the Agent data**
   - Click the **Export** button (or ellipsis `...` menu → **Export**)
   - Download the file as CSV
   - Save to a known location (e.g., `C:\Data\Agent365_Inventory.csv`)

### Expected File Format
- **File format**: CSV
- **Columns**: Agent name, Agent ID, Availability status, Last Activity Date, Template, Assigned users/sources
- **Rows**: One row per agent in your tenant


</details>

<details>
<summary>📥 Step 4: Access Org Data File (Microsoft Entra or Viva Insights)</summary>

### What This Data Provides
This file provides organizational hierarchy and user attributes, enabling segmentation by department, role, location, or other organizational dimensions.

### Requirements
- Access level required: **User Administrator** or **Global Reader** (Entra) OR **Insights Administrator** (Viva)
- Portal: Microsoft Entra Admin Center or Viva Insights
- Permissions needed: View and export user data

### Option A: Export from Microsoft Entra

1. **Navigate to the portal**
   - Sign in to: [entra.microsoft.com](https://entra.microsoft.com)
   - In the left-hand navigation, go to: `Identity ➝ Users`

2. **Select and download users**
   - Click **All users**
   - Click the **"Download users"** button (in toolbar or under `...` menu)

3. **Configure the export**
   - In the download dialog, select attributes to include:
   - **Required fields**:
     - `UserPrincipalName`
     - `Department`
   - **Optional but recommended fields**:
     - `JobTitle`
     - `Office`
     - `City`
     - `Country`
     - `Manager`
     - Any custom attributes relevant for reporting

4. **Download the file**
   - Choose **CSV format**
   - Click **Download**
   - Save to a known location (e.g., `C:\Data\Org_Data_Entra.csv`)

### Option B: Use Custom Org Data (Recommended)

If you have a custom org data file with organizational hierarchy and user attributes, you can use that instead. Ensure it includes:
- **Required columns**: UserPrincipalName or PersonID, Department or Organization

### Expected File Format
- **File format**: CSV
- **Typical size**: 1–20 MB depending on org size and attributes
- **Columns**: Varies (5–30+ columns)
- **Required columns**: UserPrincipalName, Department
- **Rows**: One row per user

💡 **Note**: Avoid downloading non-essential attributes as it can degrade performance and increase file size unnecessarily.

📖 **Learn more**: [Download a list of users – Microsoft Learn](https://learn.microsoft.com/en-us/entra/identity/users/users-bulk-download)

</details>

---

## 🔌 Optional: Extended Data Sources

The following data sources are not required for the core dashboard but unlock additional pages in the **Extended Version** of the report — covering agent health costs, user sentiment, and deep conversation analytics. Each can be added independently.

<details>
<summary>💳 Optional Step A: Export Copilot Credit Consumption (Microsoft 365 Admin Center)</summary>

### What This Data Provides
Copilot credit consumption shows how your organisation's AI capacity is being drawn down across licensed users and agents — including Microsoft 365 Copilot, Copilot Studio, and pay-as-you-go workloads. This enables cost attribution, budget forecasting, and identification of high-consumption agents or user groups.

### Requirements
- Access level required: **Global Administrator** or **Billing Administrator**
- Portal: Microsoft 365 Admin Center
- Permissions needed: View billing and usage reports

### Step-by-Step Instructions

1. **Navigate to the portal**
   - Go to: [admin.microsoft.com](https://admin.microsoft.com)
   - Log in as a **Global Administrator** or **Billing Administrator**

2. **Go to Copilot usage**
   - In the left navigation, click **Copilot**
   - Select **Usage** or **Credit consumption** (label may vary by tenant)

3. **Select your date range**
   - Use the date filter to match the period covered by your audit log export

4. **Export the data**
   - Click the **Export** button (top-right of the report view) or the ellipsis (`...`) menu → **Export**
   - Download as CSV
   - Save to a known location (e.g., `C:\Data\Copilot_Credits.csv`)

### Expected File Format
- **File format**: CSV
- **Columns**: Date, UserPrincipalName or AgentId, Product, Credits consumed, Workload type
- **Rows**: One row per consumption event or daily summary per user/agent

> 💡 **Extended report use**: Credit consumption data enables cost-per-user and cost-per-agent analysis when joined to the Purview audit log and Agent 365 inventory in Power BI.

📖 **Learn more**: [Manage Microsoft 365 Copilot credits – Microsoft Learn](https://learn.microsoft.com/en-us/microsoft-365/admin/misc/copilot-for-microsoft-365-ai-usage)

</details>

<details>
<summary>💬 Optional Step B: Export Product Feedback Report (Microsoft Admin Center — Health)</summary>

### What This Data Provides
The Product Feedback report captures in-product satisfaction signals submitted by users directly within Microsoft 365 Copilot experiences (thumbs up/down, star ratings, and free-text comments). This provides a qualitative and sentiment layer to complement quantitative usage data.

### Requirements
- Access level required: **Global Administrator** or **Reports Reader**
- Portal: Microsoft 365 Admin Center
- Permissions needed: Access to Health reports

### Step-by-Step Instructions

1. **Navigate to the portal**
   - Go to: [admin.microsoft.com](https://admin.microsoft.com)
   - Log in as a **Global Administrator** or **Reports Reader**

2. **Open the Health section**
   - In the left navigation, click **Health**
   - Select **Product feedback**

3. **Filter to Copilot feedback**
   - Use the **Product** filter to select **Microsoft 365 Copilot** (and optionally Copilot Studio or individual apps)
   - Set a **Date range** matching your audit log period

4. **Export the data**
   - Click the **Export** button or the ellipsis (`...`) menu → **Export data**
   - Download as CSV
   - Save to a known location (e.g., `C:\Data\Copilot_Feedback.csv`)

### Expected File Format
- **File format**: CSV
- **Columns**: Date, UserPrincipalName (if unmasked), Product, Feedback type (positive/negative), Rating, Verbatim comment (where provided)
- **Rows**: One row per feedback submission

> ⚠️ **Privacy note**: User-level feedback data may be anonymised depending on your tenant's reporting privacy settings. To see user-level data, ensure "Display concealed user names in all reports" is disabled under **Settings → Org Settings → Reports**.

> 💡 **Extended report use**: Feedback data enables sentiment trend analysis and correlation between usage frequency and satisfaction — helping identify which experiences are delighting vs. frustrating users.

📖 **Learn more**: [Microsoft 365 product feedback reports – Microsoft Learn](https://learn.microsoft.com/en-us/microsoft-365/admin/misc/feedback-user-control)

</details>

<details>
<summary>🤖 Optional Step C: Export Copilot Studio Agent Conversation Transcripts (Power Automate)</summary>

### What This Data Provides
Copilot Studio conversation transcripts contain the full text of every user interaction with your custom agents — including questions asked, topics triggered, session outcomes (Resolved / Abandoned / Escalated), turn counts, and captured variables. This enables deep agent performance analysis beyond the session counts available in the core dashboard.

### Requirements
- Access level required: **System Administrator**, **System Customizer**, or **Environment Maker** (with Dataverse read access)
- Portal: Power Automate / Power Apps (Dataverse)
- Permissions needed: Read access to the `ConversationTranscript` table in the Power Platform environment where your agents are published

> 💡 **Where transcripts are stored**: Transcripts are automatically saved in Microsoft Dataverse (table: `ConversationTranscript`) in the same environment as your published Copilot Studio agents. No configuration is needed — they are captured after every completed conversation.

### Choose Your Export Method

**Option A — One-time export via Power Apps (quickest)**

1. Go to [make.powerapps.com](https://make.powerapps.com) and select your agent's environment (top-right dropdown)
2. In the left menu, click **Tables** (or **Dataverse → Tables**)
3. Search for and open the **ConversationTranscript** table
4. Click **Data** → **Export data to Excel**
5. Select columns: `ConversationTranscriptId`, `ConversationStartTime`, `Content`, `Metadata`, `CreatedOn`, `bot_conversationtranscriptId`
6. Save as CSV (e.g., `C:\Data\Studio_Transcripts.csv`)

**Option B — Automated recurring export via Power Automate (recommended for ongoing refresh)**

1. Go to [make.powerautomate.com](https://make.powerautomate.com) and select your environment
2. Click **+ Create** → **Scheduled cloud flow**
3. Name it `Daily Transcript Export` and set your schedule (e.g., daily at 6:00 AM)
4. Add action: **Dataverse → List rows** — Table: `ConversationTranscripts` — Filter: `createdOn ge [yesterday's date]`
5. Add action to write output: **Create file in OneDrive/SharePoint** (CSV) or **Send email**
6. Save and test manually to verify transcripts are exported

**Option C — Direct Power BI connection to Dataverse (for real-time dashboards)**

1. In Power BI Desktop, click **Get Data → Dataverse**
2. Enter your environment URL (e.g., `orgname.crm.dynamics.com`)
3. Authenticate with your Power Platform credentials
4. Select the **conversationtranscript** table → **Transform Data**
5. Parse the `Content` column as JSON in Power Query and expand the `activities` array

### Expected File Format
- **File format**: CSV (or direct Dataverse connection)
- **Key columns**: `ConversationTranscriptId`, `ConversationStartTime`, `Content` (JSON), `Metadata`, `bot_conversationtranscriptId`
- **Content field**: JSON array of activity events — parse in Power Query using `Json.Document([Content])` to extract session outcomes, messages, topics, and variables

> 💡 **Extended report use**: Transcript data enables resolution rate tracking, topic frequency analysis, abandonment patterns, and verbatim user question themes — all joinable to the Agent 365 inventory via agent ID.

📖 **Learn more**:
- [ConversationTranscript table reference – Microsoft Learn](https://learn.microsoft.com/en-us/power-apps/developer/data-platform/reference/entities/conversationtranscript)
- [Analyze agent conversation transcripts – Microsoft Learn](https://learn.microsoft.com/en-us/power-platform/architecture/reference-architectures/analyze-agent-conversation-transcripts)
- [Power CAT Copilot Studio Kit](https://learn.microsoft.com/en-us/microsoft-copilot-studio/guidance/kit-overview) — enterprise option with pre-built KPI processing and dashboards

</details>

---

<details>
<summary>🔐 Step 5: Open and Configure the Power BI Template</summary>

### What You'll Do
Connect the Power BI template to your data sources using file paths for the CSV files.

### Step-by-Step Instructions

1. **Download the template**
   - Download **AI-in-One Dashboard - Extended Version - Template.pbit** from this repository

2. **Open the template in Power BI Desktop**
   - Double-click the `.pbit` file
   - A parameter dialog will appear

3. **Enter file paths for core data sources**
   - **Copilot Audit Log Path**: Full path to your audit log CSV
     Example: `C:\Data\Copilot_Audit_Logs.csv`
   - **Licensed Users Path**: Full path to your licensed users CSV
     Example: `C:\Data\Copilot_Licensed_Users.csv`
   - **Agent 365 Path**: Full path to your Agent 365 CSV
     Example: `C:\Data\Agent365_Inventory.csv`
   - **Org Data Path**: Full path to your org data CSV
     Example: `C:\Data\Org_Data_Entra.csv`

4. **Enter file paths for optional extended data sources** *(leave blank to skip)*
   - **Credits Path**: Full path to your Copilot credit consumption CSV
     Example: `C:\Data\Copilot_Credits.csv`
   - **Feedback Path**: Full path to your product feedback CSV
     Example: `C:\Data\Copilot_Feedback.csv`
   - **Studio Transcripts Path**: Full path to your Copilot Studio conversation transcripts CSV
     Example: `C:\Data\Studio_Transcripts.csv`

5. **Load the data**
   - Click **Load**
   - Wait for all queries to refresh (may take 5–15 minutes on first load)
   - If errors occur, verify file paths are correct and files are accessible

6. **Save and publish**
   - Save as a `.pbix` file (e.g., `AI-in-One Dashboard - Extended.pbix`)
   - Publish to your Power BI workspace
   - Configure scheduled refresh for CSV files in Power BI Service (recommended weekly or monthly)

### Troubleshooting

- **Issue**: "File not found" error
  - **Solution**: Verify file paths use absolute paths (e.g., `C:\Data\file.csv`, not `.\file.csv`) and files exist at those locations

- **Issue**: Data refresh takes extremely long
  - **Solution**: Check CSV file sizes. Very large audit logs (>500 MB) may need to be filtered or split.

- **Issue**: Optional data source pages show blank visuals
  - **Solution**: Confirm the optional CSV files have been exported and the correct paths entered in the template parameters. Pages will load once valid file paths are provided.

</details>

<details>
<summary>📊 Step 6: Review and Customize</summary>

### What You'll Do
Review the dashboard, customize visualizations, and share with stakeholders.

### Recommended Actions

1. **Review dashboard pages**
   - Navigate through all report pages
   - Verify data loaded correctly
   - Check that filters and slicers work as expected

2. **Customize for your organization**
   - Update visuals to match your branding (colors, logos)
   - Adjust hierarchies to match your org structure
   - Add or remove pages based on your needs
   - Create bookmarks for common views

3. **Set up filters and parameters**
   - Configure default date ranges
   - Set up department/role filters
   - Create user-specific views if needed

4. **Publish and share**
   - Publish to Power BI Service if not already done
   - Set up Row-Level Security (RLS) if needed
   - Share with stakeholders via workspace access or apps
   - Create subscriptions for key reports

5. **Document customizations**
   - Keep notes on any changes you make
   - Version your .pbix file if making significant updates
   - Archive old versions in the `/Archived Templates` folder

### Best Practices

- 🔄 **Refresh schedule**: Set up weekly or monthly refresh for CSV files in Power BI Service
- 🔒 **Security**: Use Row-Level Security to restrict sensitive data by department or role
- 📧 **Subscriptions**: Set up email subscriptions for executives who want regular updates
- 📊 **Usage tracking**: Monitor dashboard usage in Power BI Service to understand what resonates

</details>

---

## 🔗 Related Resources

**Base Dashboard:** The standard [AI-in-One Dashboard](https://github.com/microsoft/AI-in-One-Dashboard) covers core Copilot and Agent adoption analytics without the extended data sources.

**Viva Insights Sample Code:** Explore the [Viva Insights Sample Code Repository](https://github.com/microsoft/viva-insights-sample-code) for ready-to-use code examples, API integration patterns, and reference implementations to extend your AI adoption analytics.

**Super Usage Analysis:** For deep-dive analysis into Copilot super users and adoption patterns, check out the [DecodingSuperUsage](https://github.com/microsoft/DecodingSuperUsage) repository.

---

## 🔄 Version History

Check the `/Archived Templates` folder for previous versions of the dashboard template.

---

##  License

This project is licensed under the MIT License - see the [LICENSE.md](LICENSE.md) file for details.

---

## 🔒 Security

Please see [SECURITY.md](SECURITY.md) for information on reporting security vulnerabilities.

---

Found this useful? ⭐ Star this repo to help others discover it!

That's it! 🚀

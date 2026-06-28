# Agentforce Lead Agent

Production-ready Salesforce Agentforce implementation with AI-powered 
lead qualification, enrichment, and automated outreach.

## What Problem Does This Solve?

Sales teams waste hours manually qualifying leads, researching prospects, 
drafting outreach emails, and notifying reps. This agent automates the 
entire workflow end-to-end — from lead creation to rep notification — 
in seconds, with zero manual intervention.

**Before:** SDR spends 20 minutes per lead on research + outreach  
**After:** Agent completes full qualification cycle in under 60 seconds

## Architecture

[DIAGRAM GOES HERE — we build this next]

## How It Works

1. **Trigger** — A new Lead is created in Salesforce. A Record-Triggered 
   Flow fires automatically and dispatches the Agentforce Bot in 
   Autonomous mode.

2. **Get Lead Details** — Agent looks up the Lead and pulls all 
   available data from Salesforce.

3. **Enrich Lead** — Agent scores the lead and updates custom fields:
   `Lead_Score__c`, `Lead_Tier__c`, `Is_Enriched__c`

4. **Draft & Send Outreach Email** — Using Generative AI + pre-built 
   templates, the agent writes a personalized email and sends it 
   directly to the prospect.

5. **Notify Rep via Chatter** — Agent posts an update to Salesforce 
   Chatter so the assigned Sales Rep knows the lead is qualified 
   and contacted.

## Components

| Component | Type | Purpose |
|---|---|---|
| Lead_Agent_v2 | Agentforce Bot | AI reasoning engine |
| Lead_AI_Agent | Record-Triggered Flow | Entry point trigger |
| Get_Lead_Details | Sub-Flow | Lead data retrieval |
| Enrich_Lead_Record | Sub-Flow | Scoring + enrichment |
| Draft_and_Send_Outreach_Email | Sub-Flow | GenAI email generation |
| Notify_Rep_Chatter | Sub-Flow | Rep notification |
| AI_Outreach_* | Email Templates | Industry-specific templates |
| Agentforce_Lead_Access | Permission Set | Agent access control |

## Custom Fields on Lead Object

| Field | Purpose |
|---|---|
| Lead_Score__c | AI-calculated score 0-100 |
| Lead_Tier__c | Classification: Hot / Warm / Cold |
| Is_Enriched__c | Flag: has agent processed this lead |
| Company_Size_Bucket__c | SMB / Mid-Market / Enterprise |
| ProductInterest__c | Detected product interest |

## Problems Faced & How They Were Solved

**1. Agent not triggering autonomously**  
The Agentforce Bot must be invoked via a Flow in Autonomous mode — 
it cannot self-trigger. Solved by adding a Record-Triggered Flow 
(Lead_AI_Agent) as the entry point that dispatches the bot in the 
background.

**2. Permission errors on agent actions**  
Agent was failing silently on Chatter posts and email sends. Root cause 
was missing object-level permissions on the Permission Set. Solved by 
explicitly granting Lead read/write, Chatter create, and Flow execute 
permissions to Agentforce_Lead_Access.

**3. GenAI email tone inconsistency**  
Early drafts were too generic. Solved by creating industry-specific 
templates (Manufacturing, Technology, Default) with structured merge 
fields that give the GenAI model enough context to personalize correctly.

## Prerequisites

Before deploying, you need:

- Salesforce org with **Agentforce enabled**
- **Einstein generative AI** credits active in your org
- Salesforce CLI installed: `npm install -g @salesforce/cli`
- Git installed

Check Agentforce is enabled:  
Setup → Einstein → Generative AI → Enable

## Deployment

```bash
# 1. Authenticate to your org
sf org login web

# 2. Deploy all components
sf project deploy start

# 3. Assign permission set to your user
sf org assign permset --name Agentforce_Lead_Access

# 4. Assign permission set to the Agent user
sf org assign permset --name Lead_Agent_Access
```

## Testing the Agent

1. Open Salesforce and create a new Lead
2. Wait 30-60 seconds for the trigger to fire
3. Check the Lead record — `Is_Enriched__c` should be checked
4. Check the Lead's Chatter feed — rep notification should appear
5. Check the prospect's email inbox — outreach email should be sent

Or test manually via Agent Builder:  
Setup → Agents → Lead_Agent_v2 → Preview

## Demo

[SCREENSHOT OF AGENT WORKING IN SANDBOX GOES HERE]

## Key Insight

**The agent is a trigger, not an executor.** Agentforce interprets 
natural language and routes to your Flows. Your existing validation 
rules, sharing rules, and Apex logic all still apply. You are not 
replacing your Salesforce logic — you are adding an AI brain on top.

## Tech Stack

- Salesforce Agentforce (Einstein Copilot)
- Salesforce Flows (Record-Triggered + Sub-Flows)
- Einstein Generative AI
- Salesforce Chatter
- SFDX / Salesforce CLI
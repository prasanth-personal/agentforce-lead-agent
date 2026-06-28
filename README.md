Salesforce Agentforce: Lead + SAP Integration
Production-ready Salesforce Agentforce implementation with AI-powered lead management and real-time SAP inventory integration.

Status: Phase 2 Complete — MCP Server + Apex Backend Integration Live

Components Included
Agentforce Bot (Lead_Agent_v2): The AI reasoning engine (Copilot) that interprets prompts and dynamically selects the right tools (Flows) to execute tasks.
Email Content (AI_Outreach_*): Pre-configured email templates containing dynamic merge fields (e.g., Name, Company) and the Calendly booking link.
Core Sub-Flows: The individual "skills" the Agent uses to perform work:
Get_Lead_Details: Looks up a Lead in the database.
Enrich_Lead_Record: Updates the Lead with scoring data (Lead_Score__c, Lead_Tier__c).
Draft_and_Send_Outreach_Email: Uses Generative AI and templates to email the prospect.
Notify_Rep_Chatter: Posts an alert to the Sales Rep on Salesforce Chatter.
Automation Trigger (Lead_AI_Agent): A Record-Triggered Flow that listens for new Leads and automatically dispatches the Agentforce Bot in the background (Autonomous mode) to run the business workflow.
Lead Data Model: Custom fields on the standard Lead object used to store enrichment data and Einstein Lead Scores.
Permissions (Agentforce_Lead_Access): The specific Permission Set required to grant the Agent access to Leads, Chatter, and the required Flows.

# Problem Context — Why This FinOps Guardrails Project Exists

## The Real Problem
In SaaS environments, teams deploy resources quickly, often across different regions and services without a consistent ownership model.

That creates immediate FinOps risk:
- You can’t confidently answer “who owns this cost?”
- You can’t reliably allocate spend by application/team
- Budget alerts fire without context
- Governance becomes reactive (and political) instead of operational

In short: **you can’t manage what you can’t attribute.**

## The Symptoms This Project Addresses
This project was built to address three common symptoms seen in real enterprise Azure subscriptions:

1) **Resources exist with no tags**
   - No Owner
   - No App
   - No CostCenter
   - No Env

2) **Cost Management is weak without structure**
   - Budgets/alerts help, but without tags they don’t tell you *who* should respond.

3) **Governance enforcement can create friction**
   - If you roll out strict policies too early, you block legitimate platform setup.
   - If you don’t roll out governance, you get chaos.

This project demonstrates how to handle that tradeoff cleanly.

## The Outcome (What “Good” Looks Like)
After completing the steps in this repo, you should be able to say:

- Every workload resource group has required tags
- Azure Policy is auditing and reporting missing tags
- Compliance can be verified in Policy → Compliance
- A budget guardrail exists and alerts are configured
- Platform bootstrapping can occur without permanently weakening governance

## What Makes This Portfolio-Ready
This repo includes “real world behavior”:

- You will see non-compliance immediately after policy assignment
- You will see policy evaluation delays (eventual consistency)
- You will see governance blocking governance tooling (Action Groups)
- You will see how to use a temporary exclusion responsibly during bootstrap

That’s exactly what cloud management looks like in a production organization.
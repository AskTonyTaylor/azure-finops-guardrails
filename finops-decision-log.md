# FinOps Decision Log (Azure SaaS Guardrails)

This document records what decisions were made during the build, **why** they were made, and what tradeoffs were accepted.
The goal is to make the project reproducible and understandable by another engineer.

## Decision 001 — Use 4 Required Tags as the Minimum Standard
**Decision:** Enforce the following required tags across the workload:
- `App`
- `CostCenter`
- `Env`
- `Owner`

**Why:**
- These four tags are the minimum needed for cost allocation + accountability.
- Anything less produces unclear reporting and unknown owner cost.

**Tradeoff:**
- Some teams will see this as overhead.
- The first enforcement phase is **Audit**, not Deny.

**Status:** Adopted

## Decision 002 — Separate Resource Groups: Workload vs Platform/Bootstrap
**Decision:** Use two RGs:
- `rg-saas-finops-demo` (workload)
- `rg-saas-finops-platform` (platform/bootstrap)

**Why:**
- Workloads need consistent governance and cost controls.
- Platform setup sometimes requires flexibility early (tools, monitoring, supporting services).
- Separating scopes prevents governance friction from stalling setup.

**Tradeoff:**
- Two RGs adds minor structure overhead.
- It prevents bigger delays later (policy conflicts, ownership confusion).

**Status:** Adopted

## Decision 003 — Start with Azure Policy “Audit” (Not Deny)
**Decision:** Assign Azure Policy definitions to **audit** missing tags at the subscription scope.

**Why:**
- Audit gives visibility and measurement without breaking deployments.
- A phased rollout is realistic in enterprise environments and ideal for change management.

**Tradeoff:**
- Audit does not prevent bad behavior; it reveals it.
- Deny can be introduced later once teams/tooling are trained and ready.

**Status:** Adopted

## Decision 004 — Use Budget Guardrail First (Fastest Win)
**Decision:** Create a resource-group scoped monthly budget with email alerts.

**Why:**
- Budgets provide immediate cost visibility.
- It’s simple to configure, easy to explain, and useful immediately.
- It creates a “cost ceiling” while governance standards mature.

**Tradeoff:**
- Budgets don’t fix attribution by themselves.
- That’s why tags + policy still matter.

**Status:** Adopted

## Decision 005 — Action Groups Were Blocked by Policy (Governance Blocking Governance)
**Decision:** When Action Group creation was denied by tag policy, use a controlled workaround:
- temporary exclusion for the RG

**Why:**
- Action Group resources themselves must comply with tag policies.
- In early rollout, platform tooling often needs to be created before everything is fully standardized.
- A temporary exclusion enables bootstrap without permanently weakening governance.

**Tradeoff:**
- Exclusions can be abused if left in place.
- The intent is temporary, with a follow-up to remove it once tooling is tagged and stable.

**Status:** Adopted (temporary)

## Decision 006 — App Service Plan Tagging Limitation: Use RG Tags as the Attribution Backstop
**Decision:** One App Service Plan (Free tier, Linux) failed direct tagging due to provider-level limitations.
To maintain attribution, apply required tags at the resource-group level.

**Why:**
- Some resource types and tiers can behave inconsistently with direct tagging.
- RG-level tags still provide effective tagging for cost allocation.

**Tradeoff:**
- Resource-level tagging is preferred, but not always possible.
- RG-level tags are the operational safety net.

**Status:** Adopted

## Decision 007 — Accept Policy Evaluation Delay as Normal (Eventual Consistency)
**Decision:** After tagging, compliance did not update immediately. No extra configuration changes were made; we allowed the policy evaluation cycle to complete.

**Why:**
- Azure Policy compliance is evaluated periodically.
- Immediate UI feedback is not guaranteed at subscription scope.

**Tradeoff:**
- Engineers can misdiagnose this as “something is broken.”
- The correct approach is: verify tags exist, then wait for evaluation.

**Status:** Adopted (documented as expected behavior)

## Follow-Up Cleanup (Important)
- Remove the temporary exclusion once platform resources are tagged and stable.
- Consider bundling tag requirements into a single initiative for cleaner governance management.
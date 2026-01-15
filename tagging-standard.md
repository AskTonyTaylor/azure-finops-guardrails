# Azure Tagging Standard – SaaS Cost Accountability

## Objective
Enable cost allocation, ownership, and accountability across SaaS workloads by enforcing a consistent tagging standard across Azure resources and resource groups.

This standard is intentionally small: **4 required tags**. If you can’t consistently do these four, everything else (budgets, forecasting, optimization) becomes guesswork.

## Required Tags (Minimum Standard)

| Tag | Purpose | Example |
|---|---|---|
| Env | Environment classification | `prod` / `nonprod` |
| Owner | Accountable team or individual | `finops-demo` / `data-platform` |
| App | Logical SaaS application/workload | `saas-finops-guardrails` |
| CostCenter | Financial allocation reference | `CC-0001` |

## Where to Apply Tags

### Resource Group Level (Baseline)
**Always tag the Resource Group.**
This creates a baseline for:
- cost allocation reporting (“effective tags”)
- consistent grouping and filtering
- a safety net when resource-level tagging is inconsistent

### Resource Level (Preferred When Possible)
When the provider supports it, add the same tags directly to the resource.
Resource-level tags provide:
- higher fidelity reporting
- portability if a resource moves across RGs later
- clearer ownership at the object level

**Reality check:** Some resources and deployment flows don’t tag cleanly, or teams forget. That’s why RG-level tagging is non-negotiable.

## Why This Matters (FinOps Context)
Without consistent tagging:
- cloud costs cannot be attributed to teams or applications
- budgets and alerts lack context
- anomaly investigation becomes manual and slow
- executive cost conversations become reactive instead of proactive

With consistent tagging:
- ownership is explicit
- cost conversations become measurable
- optimization is targeted (not random)
- accountability becomes operational (not political)

## Standard Values Used in This Project (Demo)
For this repo, the demo values were:

- `App = saas-finops-guardrails`
- `CostCenter = CC-0001`
- `Env = nonprod`
- `Owner = finops-demo`

If you copy this project for your own org, keep the same tag keys but change values to match your company’s naming conventions.
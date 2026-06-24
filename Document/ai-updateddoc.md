# AI PR Review – Cost Estimation

## Model Used

This estimation is based on Microsoft Foundry using the GPT-4o-mini model.

## Deployment Region Note

GPT-4o-mini is currently available in East US and was not available in Central India at the time of setup. Because of this, the current deployment has been created in the East US region.

## Official Pricing Reference

As per the Azure OpenAI / Azure AI Foundry pricing referenced for the currently deployed GPT-4o-mini Global configuration:

* Input tokens: ₹105.27 per 1M tokens
* Output tokens: ₹421.05 per 1M tokens

Note: Cached input pricing is also available on the Azure pricing page, but it has not been considered in this estimate to keep the calculation simple and conservative.

---

## Assumptions Used for Estimation

For a typical PR review, assume:

* Average PR diff size: ~200 lines of code
* Estimated input tokens: ~3,000 tokens
* Estimated AI review output: ~800 tokens

Where:

* Input = prompt + PR diff + metadata
* Output = AI-generated PR review comments, summary, and suggestions

---

## Cost per PR

### Input cost

3,000 tokens × ₹105.27 / 1,000,000
= ₹0.31581

### Output cost

800 tokens × ₹421.05 / 1,000,000
= ₹0.33684

### Total cost per PR

₹0.31581 + ₹0.33684 = ₹0.65265

Approximate cost per PR:

## ₹0.65 per PR

---

## Monthly Cost Example

Assuming:

* 20 PRs per day
* 22 working days per month

Total PRs per month:

20 × 22 = 440 PRs

### Monthly cost

440 × ₹0.65265 = ₹287.17 per month

Approximate monthly cost:

## ₹285–290 per month

---

## Cost Summary

| Item                             | Estimate            |
| -------------------------------- | ------------------- |
| Model                            | GPT-4o-mini         |
| Input pricing                    | ₹105.27 / 1M tokens |
| Output pricing                   | ₹421.05 / 1M tokens |
| Approx. cost per PR              | ₹0.65               |
| PRs per month (20/day × 22 days) | 440                 |
| Approx. monthly cost             | ₹285–290            |

---

## Conclusion

Based on the above assumptions and the Azure pricing for the currently deployed GPT-4o-mini Global configuration, the estimated cost of running AI PR Review is approximately ₹0.65 per PR.

For an expected volume of 20 PRs per day over 22 working days, the monthly cost is estimated to be around ₹285–290.

---

## References

1. Azure OpenAI Service Pricing
   https://azure.microsoft.com/en-us/pricing/details/azure-openai/

2. Azure AI Foundry Models Pricing
   https://azure.microsoft.com/en-us/pricing/details/ai-foundry-models/aoai/

3. Microsoft announcement for GPT-4o mini on Azure
   https://azure.microsoft.com/en-us/blog/openais-fastest-model-gpt-4o-mini-is-now-available-on-azure-ai/

4. Azure AI Foundry / Azure OpenAI billing and model information
   https://learn.microsoft.com/en-us/azure/foundry/foundry-models/concepts/models-sold-directly-by-azure

---

## Notes

* This estimate assumes a standard GPT-4o-mini deployment used only for PR review inference and excludes optional add-on services or future infrastructure changes.
* The cost shared in this document is based on assumptions and publicly available Microsoft/Azure pricing documentation for the currently deployed Global configuration.
* Cached input pricing has not been included in the estimate. If any part of the repeated prompt/instructions benefits from caching, the actual cost may be slightly lower.
* Actual cost may vary depending on PR size, token usage, workflow behavior, and Azure billing details, and can only be confirmed once the Azure bill/invoice is received after usage.

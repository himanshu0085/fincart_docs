# AI PR Review – Cost Estimation

## Model Used

This estimation is based on **Microsoft Foundry using GPT-4o-mini**.

## Official Pricing Reference

As per Azure OpenAI / Azure AI Foundry pricing for **GPT-4o-mini**:

* **Input tokens:** **$0.15 per 1M tokens**
* **Output tokens:** **$0.60 per 1M tokens**

---

## Assumptions Used for Estimation

For a typical PR review, assume:

* **Average PR diff size:** ~200 lines of code
* **Estimated input tokens:** ~3,000 tokens
* **Estimated AI review output:** ~800 tokens

Where:

* **Input** = prompt + PR diff + metadata
* **Output** = AI-generated PR review comments / summary / suggestions

---

## Cost per PR

### Input cost

3,000 tokens × $0.15 / 1,000,000
= **$0.00045**

### Output cost

800 tokens × $0.60 / 1,000,000
= **$0.00048**

### Total cost per PR

**$0.00093 per PR**

At an approximate conversion of **₹83–86 per USD**, this is roughly:

## **₹0.08 per PR**

---

## Monthly Cost Example

Assuming:

* **20 PRs per day**
* **22 working days per month**

Total PRs per month:

**20 × 22 = 440 PRs**

### Monthly cost

440 × $0.00093
= **$0.4092 per month**

In INR, this is approximately:

## **₹34–36 per month**

---

## Cost Summary

| Item                             | Estimate          |
| -------------------------------- | ----------------- |
| Model                            | GPT-4o-mini       |
| Input pricing                    | $0.15 / 1M tokens |
| Output pricing                   | $0.60 / 1M tokens |
| Approx. cost per PR              | ₹0.08             |
| PRs per month (20/day × 22 days) | 440               |
| Approx. monthly cost             | ₹34–36            |

---

## Conclusion

Using **GPT-4o-mini on Microsoft Foundry** for AI PR Review results in a **very low operating cost**.

For an estimated workload of **20 PRs per day**, the total monthly cost is approximately:

## **₹34–36 per month**

---

## References

1. **Azure OpenAI Service Pricing**
   https://azure.microsoft.com/en-us/pricing/details/azure-openai/

2. **Azure AI Foundry Models Pricing**
   https://azure.microsoft.com/en-us/pricing/details/ai-foundry-models/aoai/

3. **Microsoft announcement for GPT-4o mini on Azure**
   https://azure.microsoft.com/en-us/blog/openais-fastest-model-gpt-4o-mini-is-now-available-on-azure-ai/

4. **Azure AI Foundry / Azure OpenAI billing and model information**
   https://learn.microsoft.com/en-us/azure/foundry/foundry-models/concepts/models-sold-directly-by-azure

---

## Notes

* This estimate assumes a **standard GPT-4o-mini deployment used only for PR review inference**, and excludes **optional add-on services or future infrastructure changes**.
* This is an **estimated cost based on publicly available Microsoft / Azure pricing documentation**. Actual cost may vary depending on real PR size, token usage, workflow configuration, and Azure billing, and should be validated against the final production bill.

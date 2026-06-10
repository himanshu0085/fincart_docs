# AI PR Review Automation POC – Findings and Observations

## 1. Objective

The objective of this POC was to evaluate the feasibility of automating Pull Request reviews using AI within Azure DevOps and assess the quality, performance, operational considerations, and infrastructure requirements for broader adoption.

---

## 2. Scope

The POC covered:

* PR diff extraction from Azure DevOps
* AI-based code review generation
* SonarQube findings integration
* Semgrep security findings integration
* Automated PR comment publishing
* Model evaluation and comparison
* Infrastructure and performance assessment
* Build pipeline optimization opportunities

---

## 3. Solution Architecture

The solution introduces AI-assisted Pull Request review capabilities into the existing Azure DevOps pipeline without impacting the existing build, code quality, or deployment workflow.

| Layer             | Component                  | Details                                                    |
| ----------------- | -------------------------- | ---------------------------------------------------------- |
| Source Control    | Azure DevOps Git           | Batch_Processing repository                                |
| CI Pipeline       | Azure Pipelines            | Microsoft-hosted agent (ubuntu-latest)                     |
| Build Cache       | Azure Pipeline Cache Task  | Maven dependency caching based on pom.xml hash             |
| Code Quality      | SonarQube                  | Static code analysis integrated into pipeline              |
| SAST              | Semgrep                    | Security scanning integrated into pipeline                 |
| AI Review Engine  | Ollama + Qwen 2.5 Coder 7B | AI model hosted on dedicated Azure VM                      |
| Review Processing | Python (pr-review.py)      | Extracts PR diff, combines scan findings, generates review |
| PR Integration    | Azure DevOps REST API      | Posts generated review comments back to Pull Requests      |

### Workflow Overview

1. Pull Request is created or updated.
2. Azure DevOps pipeline is triggered.
3. PR diff is extracted.
4. SonarQube findings are collected.
5. Semgrep security findings are collected.
6. Review context is sent to Ollama for analysis.
7. AI-generated review is created.
8. Review is automatically posted back to the Pull Request.

---

## 4. Validation Outcomes

The following capabilities were validated:

* PR diff extraction from Azure DevOps
* AI-generated code review creation
* SonarQube integration
* Semgrep integration
* Automated PR comment publishing
* End-to-end workflow execution

Multiple Pull Requests were used to evaluate review quality, response behavior, and review relevance.

---

## 5. Model Evaluation

### Models Evaluated

* Qwen 2.5 Coder 3B
* Qwen 2.5 Coder 7B
* Qwen 2.5 Coder 14B
* Gemma
* Mistral

### Observations

* Qwen 2.5 Coder 3B provided faster responses but review quality was comparatively less detailed.
* Qwen 2.5 Coder 7B provided the best balance between review quality, response time, and infrastructure requirements.
* Qwen 2.5 Coder 14B generated more detailed reviews but required significantly higher inference time and compute resources.
* Gemma and Mistral were evaluated based on recommendations from the AI team.
* No significant improvement in review quality or review relevance was observed with Gemma and Mistral compared to the Qwen models.
* Resource utilization and response characteristics remained broadly comparable across the evaluated models.

### Result

Qwen 2.5 Coder 7B was selected as the preferred model for the POC based on the balance between review quality, response latency, infrastructure consumption, and operational simplicity.

---

## 6. Infrastructure Observations

### Current Architecture

* Azure DevOps Microsoft-hosted Agent
* SonarQube hosted on a separate VM
* Ollama hosted on a separate VM

### Observed Challenges

* Multiple network hops between services
* Additional pipeline startup overhead
* Increased latency for larger Pull Requests
* Occasional timeout scenarios during long-running inference requests
* Dependency on communication between Azure-hosted agents and externally hosted services

### Observation

The current architecture is functional for POC validation but introduces latency and timeout considerations when processing larger Pull Requests.

As a potential improvement area, self-hosted Azure DevOps agents can be evaluated to determine whether they help reduce network overhead and improve execution consistency. However, this would require additional validation and testing and would also introduce operational responsibilities such as VM management, patching, monitoring, and capacity planning.

---

## 7. Performance Observations

### Current VM

**D4as_v5**

* Approximate Cost: ₹7,850/month
* Average inference time: ~130 seconds
* Reliable processing up to approximately 6,000 characters of PR diff content

### Observed Limitation

When larger diffs are processed:

* Prompt size increases significantly
* Model inference time increases
* Ollama requests may hit timeout limits
* Review generation becomes less predictable

### Potential Upgrade Option

**NV4as_v4 (GPU Enabled)**

* Approximate Cost: ~₹20,000–21,000/month
* Expected inference time: ~15–20 seconds
* Potential support for 2–3 concurrent review requests (requires validation)

---

## 8. SonarQube Integration Observations

### Implementation

* SonarQube findings were integrated into the AI review workflow.
* SonarQube issues are collected during pipeline execution and provided as additional context to the AI review process.
* Sonar findings are included alongside code changes and Semgrep results to improve review relevance.

### Observations

* SonarQube findings provided additional context during AI-generated reviews.
* The quality and usefulness of AI recommendations improved when static analysis findings were included in the review prompt.
* Integration was validated as part of the end-to-end workflow.

### Limitation

* The effectiveness of SonarQube-based review feedback depends on the quality and relevance of findings available for the analyzed project.
* Additional project-level validation may be required before broader adoption.

---

## 9. AI Review Governance

Although Azure DevOps APIs allow automation of Pull Request actions, automatic PR merge or closure based solely on AI review output is not recommended.

### Reasons

* AI-generated reviews may produce false positives.
* AI-generated reviews may miss business-specific issues.
* Human review remains necessary for approval decisions.
* AI should act as a review assistant rather than a replacement for engineering review processes.

### Recommended Usage

AI review should be treated as advisory feedback and should complement existing review and approval workflows.

---

## 10. Build Pipeline Optimization

### Objective

Review the existing Maven build process where all dependencies were downloaded during every pipeline execution.

### Implementation

* Analyzed the existing Azure DevOps Maven build workflow.
* Implemented Maven dependency caching using Azure DevOps Cache tasks.
* Configured pipelines to reuse previously downloaded Maven dependencies.
* Ensured that only newly added or updated dependencies are downloaded during subsequent builds.

### Benefits Achieved

* Reduced dependency download overhead.
* Improved pipeline execution efficiency.
* Reduced external repository calls.
* Reduced network utilization.
* Improved overall build performance for Maven-based applications.

### Observation

During validation, it was identified that a shared pipeline template change impacted a non-Maven application (UAT-Fincart-Dashboard). This highlighted the need for technology-specific pipeline templates to avoid cross-project impact when introducing build optimizations.

---

## 11. Conclusion

The POC demonstrated the feasibility of integrating AI-assisted code reviews into the Azure DevOps Pull Request workflow.

The following objectives were achieved:

* Automated Pull Request diff extraction
* AI-based review generation
* SonarQube findings integration
* Semgrep security findings integration
* Automated Pull Request comment publishing

The primary limitations identified relate to infrastructure sizing, model inference latency for large Pull Requests, and scalability considerations for broader adoption.

Overall, the POC establishes a functional foundation for AI-assisted code review within Azure DevOps while highlighting the infrastructure and operational considerations that would need to be evaluated before wider adoption.

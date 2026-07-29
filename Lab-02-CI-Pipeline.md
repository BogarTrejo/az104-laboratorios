# Lab 2: Automated CI Pipeline with GitHub Actions

## 📌 Objective
Implement a Continuous Integration (CI) pipeline using GitHub Actions to automate repository validation, ensuring code and documentation structure integrity upon every push and pull request.

## 🛠️ Steps Performed
1. **Workflow Initialization:** 
   - Created the required directory structure (`.github/workflows/`) within the repository.
2. **Pipeline Configuration:** 
   - Developed a YAML-based workflow (`ci-validation.yml`) configured to trigger automatically on main branch pushes and pull requests.
   - Defined runner environments (`ubuntu-latest`) and integrated checkout and runtime setup steps.
3. **Execution & Validation:** 
   - Validated the pipeline execution in GitHub Actions, ensuring successful build status and automated directory structure inspection.

## 📸 Visual Evidence
<img width="1516" height="350" alt="CICDPipeline" src="https://github.com/user-attachments/assets/6c2710e7-9ffd-44a9-b2e0-a6c1a9255d7f" />

## 🧠 Key Learnings & Architecture Notes
- **Automation First:** Automating quality gates prevents manual oversights and mirrors modern enterprise GitOps workflows.
- **Feedback Loops:** Fast pipeline execution ensures immediate validation of infrastructure documentation and code changes.

# CI Pipeline

## Workflow configuration
The pipeline is defined in .github/workflows/build-test-pipeline.yml and uses GitHub Actions.

The workflow is triggered by:
- push to the main branch
- pull_request targeting the main branch

It runs on an ubuntu-latest runner.

## Pipeline steps
1. **Check out the code:** Uses actions/checkout to retrieve the repository contents so that following steps can access the project files.
2. **Run the build script:** Executes ./scripts/build.sh to perform the build checks. If ShellCheck is not installed, the script skips that check.
3. **Run the test script:** Executes ./scripts/test.sh to verify that required repository files exist, that scripts/build.sh is executable and that README.md contains a top-level Markdown heading.

## Failure handling
The pipeline relies on exit codes to determine whether a step succeeds or fails.

The build script uses `set -euo pipefail` to detect errors and stop execution when something fails. The test script records failed checks and exits with a non-zero status if any checks fails.

If a step fails, GitHub Actions marks the job as failed. 

## AI generated CI pipeline
For comparison with the original workflow, I used AI (specifically ChatGPT) to generate an alternative CI pipeline. This is saved in `.github/workflows/ai-build-test-pipeline`.
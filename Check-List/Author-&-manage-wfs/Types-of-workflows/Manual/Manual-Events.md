# Configure different events for Workflows to run

## Manual Events
---- 
- To run a workflow manually the workflow must be configured to run on the `workflow_dispatch` event
- Workflow must be in the default branch
- Can be triggered using GitHub API, GitHub CLI or the GitHub UI
- `on: workflow_dispatch`
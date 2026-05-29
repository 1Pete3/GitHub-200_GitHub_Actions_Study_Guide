# Configure different events for Workflows to run

## Manual Events
---- 
- To run a workflow manually the workflow must be configured to run on the `workflow_dispatch` event
- Workflow must be in the default branch
- Can be triggered using GitHub API, GitHub CLI or the GitHub UI
- `on: workflow_dispatch`

Example:
```
name: Manual-Workflow
run-name: Manual-Workflow
on: workflow_dispatch
jobs:
    run-manual-wf:
        runs-on: ubuntu-latest
        steps:
            - name: "random name"
              uses: octodemo-resources/name-generator-action@eaebafb63565c11a483907f1f589adf02fa7be85
            
            - name: "echo"
              run: echo "This is a manual workflow that uses the workflow_dispatch event"
```
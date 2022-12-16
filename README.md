# Automatic movement Issue column for Jira
Github actions give us the ability to set a trigger for our events. <br/>
You can automatically update the column status of your Jira issues by using this yml file. <br/>
For instance, after the relevant branch is merged with the dev branch, you may want to move your issue to the QA stage. <br/>
This is great because it will increase production. <br/>
It is also possible to change the status of other columns <br /><br />
<b>Notice:</b><br />
In your Github secrets, you must include your Jira base URL, user email, and API token
<br/>

```OASv2-yaml
name: Move branchs issue to QA 
'on': #select an event to run
  push:
    branches:
      - dev
jobs:
  login-to-jira:
    runs-on: ubuntu-latest
    steps:
      - uses: atlassian/gajira-login@master
        env:
          JIRA_BASE_URL: '${{ secrets.JIRA_BASE_URL }}'
          JIRA_USER_EMAIL: '${{ secrets.JIRA_USER_EMAIL }}'
          JIRA_API_TOKEN: '${{ secrets.JIRA_API_TOKEN }}'
      - uses: atlassian/gajira-find-issue-key@v3
        with:
          from: branch
        id: issue
      - uses: tuanddd/jira-automate-transition@v1.0.5
        with:
          github-token: '${{ secrets.GITHUB_TOKEN }}'
          # column-to-move-to-when-review-requested: In Review
          # column-to-move-to-when-changes-requested: In Progress
          column-to-move-to-when-merged: QA
          jira-issue-id: '${{ steps.issue.outputs.issue_key }}'

```

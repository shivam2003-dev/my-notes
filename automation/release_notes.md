Yes, automating the creation of release notes from GitHub commits to Confluence is possible and can significantly streamline your release process. This automation can be achieved using a combination of GitHub Actions, Jira, and Confluence integrations. Here's how you can approach this:

## Automation Solution

### 1. GitHub Actions Workflow

You can create a GitHub Actions workflow to trigger the release notes generation process. This workflow can be set up to run when a new release is created or tagged in your repository.

Here's a sample workflow YAML file:

```yaml
name: Generate Release Notes

on:
  release:
    types: [created]

jobs:
  create-release-notes:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      
      - name: Extract Release Notes
        id: extract_notes
        run: |
          # Extract commit messages since last release
          git log $(git describe --tags --abbrev=0)..HEAD --pretty=format:"%s" > release_notes.txt
      
      - name: Create Confluence Page
        uses: atlassian/gajira-login@master
        env:
          JIRA_BASE_URL: ${{ secrets.JIRA_BASE_URL }}
          JIRA_USER_EMAIL: ${{ secrets.JIRA_USER_EMAIL }}
          JIRA_API_TOKEN: ${{ secrets.JIRA_API_TOKEN }}
        
      - name: Create Confluence Page
        uses: atlassian/gajira-create@master
        with:
          project: YOUR_PROJECT_KEY
          issuetype: Task
          summary: Release Notes for ${{ github.ref }}
          description: |
            Release notes for version ${{ github.ref }}:
            $(cat release_notes.txt)
```

### 2. Jira and Confluence Integration

To create the release notes in Confluence, you can use the Jira Cloud REST API to create a new page. This can be done within your GitHub Actions workflow or as a separate script.

### 3. Customization

You may want to customize the release notes format, filter certain commit messages, or add additional information from Jira issues. This can be done by modifying the script that extracts the release notes.

## Implementation

To implement this automation:

1. Create the GitHub Actions workflow file in your repository under `.github/workflows/`.

2. Set up the necessary secrets in your GitHub repository for Jira and Confluence authentication.

3. Customize the workflow to fit your specific needs, such as formatting the release notes or adding additional information from Jira.

4. Test the workflow by creating a new release in your GitHub repository.

## Who Can Work on This

This automation project requires someone with the following skills:

1. Familiarity with GitHub Actions and workflow YAML syntax
2. Knowledge of Git commands for extracting commit messages
3. Experience with Jira and Confluence APIs
4. Basic scripting skills (e.g., Bash, Python) for customizing the release notes extraction

Ideally, a DevOps engineer or a developer with experience in CI/CD processes would be well-suited to implement this automation. They should also have access to the necessary Jira and Confluence permissions to create and modify pages.

By implementing this automation, your HES team can save time and ensure consistency in generating release notes, making it easier to track and communicate changes for each new release[1][2][3].

Citations:
[1] https://www.youtube.com/watch?v=lwg0_q1GIjQ

[2] https://community.atlassian.com/t5/App-Central-articles/Create-stellar-release-notes-with-Confluence-and-GitHub/ba-p/2211865

[3] https://www.atlassian.com/blog/confluence/streamline-release-notes-creation-with-confluence-and-jira

[4] https://community.atlassian.com/t5/Marketplace-Apps-Integrations/How-to-automate-Release-notes-to-Confluence-page/qaq-p/2121904

[5] https://tray.ai/connectors/atlassian-confluence-github-integrations

[6] https://github.com/marketplace/actions/release-notes-to-confluence-publisher

[7] https://drymerge.com/use-case/create-confluence-pages-from-github-release-notes-confluence-github

[8] https://docs.github.com/en/repositories/releasing-projects-on-github/automatically-generated-release-notes

[9] https://confluence.atlassian.com/automation/use-automation-with-github-1141480582.html

[10] https://www.linkedin.com/pulse/how-did-i-build-automated-release-notes-miroslav-sommer

[11] https://docs.github.com/en/repositories/releasing-projects-on-github/automatically-generated-release-notes

[12] https://github.com/viqueen/confluence-content-extractor

[13] https://www.moveworkforward.com/blog/keeping-confluence-documentation-up-to-date-with-github-changes

[14] https://marketplace.atlassian.com/apps/1230629/connector-for-github-and-confluence?tab=overview&hosting=cloud

[15] https://support.atlassian.com/bitbucket-cloud/docs/use-smart-commits/

[16] https://github.com/marketplace/actions/release-notes-to-confluence-publisher

[17] https://github.com/marketplace/actions/jira-release-notes

[18] https://www.moveworkforward.com/blog/automating-confluence-updates-in-microsoft-teams-a-comprehensive-guide

[19] https://www.youtube.com/watch?v=vYlXRl7vIsQ

[20] https://www.moveworkforward.com/product/microsoft-teams-confluence-connector

[21] https://zapier.com/apps/confluence/integrations/github

[22] https://www.youtube.com/watch?v=e5yr7nIqTc8

[23] https://github.com/atlassian/github-for-jira

[24] https://www.youtube.com/watch?v=N-RZjp4og28

[25] https://github.com/anmolagrwl/forge-ai-confluence-keyword-extractor

[26] https://github.com/marketplace/actions/jira-extract-issue-keys

[27] https://www.linkedin.com/pulse/how-did-i-build-automated-release-notes-miroslav-sommer

[28] https://support.atlassian.com/jira-cloud-administration/docs/create-release-notes/

[29] https://www.youtube.com/watch?v=lwg0_q1GIjQ

[30] https://www.k15t.com/rock-the-docs/confluence-use-cases/documentation-in-confluence-cloud/building-a-foundation-for-your-release-notes-in-confluence

[31] https://bitbucket.org/product/

[32] https://support.atlassian.com/cloud-automation/docs/use-atlassian-automation-with-github/

[33] https://confluence.atlassian.com/automation0802/use-automation-with-microsoft-teams-1283687057.html

[34] https://confluence.atlassian.com/automation/use-automation-with-microsoft-teams-993924683.html
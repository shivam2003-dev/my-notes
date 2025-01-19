Yes, it is feasible to create a GitHub Actions workflow that runs an Ansible playbook on workflow dispatch with custom inputs like IP address and vault URL. This approach allows you to trigger the workflow manually and provide the necessary parameters each time you run it.

Here's an example workflow file that demonstrates how to achieve this:

```yaml
name: Run Ansible Playbook

on:
  workflow_dispatch:
    inputs:
      ip_address:
        description: 'Target IP address'
        required: true
      vault_url:
        description: 'URL of the vault storing the PEM file'
        required: true

jobs:
  run-ansible:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Set up Python
        uses: actions/setup-python@v4
        with:
          python-version: '3.x'

      - name: Install Ansible
        run: |
          python -m pip install --upgrade pip
          pip install ansible

      - name: Retrieve PEM file from vault
        run: |
          # Add commands to retrieve the PEM file from the vault
          # This step will depend on your specific vault implementation
          # For example, you might use curl or a specific vault CLI tool
          # curl -H "X-Vault-Token: ${{ secrets.VAULT_TOKEN }}" ${{ github.event.inputs.vault_url }} > key.pem
          echo "Retrieving PEM file from ${{ github.event.inputs.vault_url }}"
          # Placeholder command:
          touch key.pem

      - name: Set permissions for PEM file
        run: chmod 600 key.pem

      - name: Run Ansible playbook
        run: |
          ansible-playbook -i "${{ github.event.inputs.ip_address }}," \
                           --private-key key.pem \
                           -u your_ssh_user \
                           your-playbook.yml
```

This workflow does the following:

1. It's triggered manually using the `workflow_dispatch` event.

2. It accepts two inputs: `ip_address` and `vault_url`.

3. It sets up Python and installs Ansible.

4. It retrieves the PEM file from the specified vault URL (you'll need to implement this step based on your specific vault solution).

5. It sets the correct permissions for the PEM file.

6. Finally, it runs the Ansible playbook using the provided IP address and the retrieved PEM file.

To use this workflow:

1. Place this YAML file in your repository under `.github/workflows/`.

2. Replace `your-playbook.yml` with the actual name of your Ansible playbook.

3. Replace `your_ssh_user` with the appropriate SSH user for your target machines.

4. Implement the step to retrieve the PEM file from your vault, replacing the placeholder command.

5. If your vault requires authentication, you may need to add a secret to your GitHub repository (e.g., `VAULT_TOKEN`) and use it in the vault retrieval step.

When you want to run the playbook, go to the Actions tab in your GitHub repository, select this workflow, and click "Run workflow". You'll be prompted to enter the IP address and vault URL before the workflow runs[1][4][6].

Remember to secure any sensitive information, such as vault tokens or passwords, using GitHub Secrets rather than hardcoding them in the workflow file[5].

Citations:
[1] https://graphite.dev/guides/github-actions-inputs

[2] https://www.pluralsight.com/courses/ansible-integrating-github-actions

[3] https://docs.github.com/en/actions/about-github-actions/understanding-github-actions

[4] https://www.youtube.com/watch?v=Sb_zLeHEVqQ

[5] https://spacelift.io/blog/github-actions-ansible

[6] https://docs.github.com/en/actions/writing-workflows/quickstart

[7] https://stackoverflow.com/questions/71155641/github-actions-how-to-view-inputs-for-workflow-dispatch/75578941

[8] https://www.geeksforgeeks.org/integrating-ansible-with-github-for-cicd-pipelines/

[9] https://codefresh.io/learn/github-actions/github-actions-workflows-basics-examples-and-a-quick-tutorial/

[10] https://dev.to/joshduffney/deploy-ansible-with-github-actions-2f3d

[11] https://github.com/marketplace/actions/workflow-dispatch

[12] https://www.devopstricks.in/deploy-ansible-playbook-using-github-action/

[13] https://docs.github.com/en/actions/writing-workflows/workflow-syntax-for-github-actions

[14] https://github.blog/changelog/2021-11-10-github-actions-input-types-for-manual-workflows/

[15] https://networkbrouhaha.com/2020/05/ansible-with-github-actions/

[16] https://docs.github.com/en/actions/use-cases-and-examples/creating-an-example-workflow

[17] https://docs.github.com/en/enterprise-cloud@latest/actions/writing-workflows/workflow-syntax-for-github-actions

[18] https://github.com/marketplace/actions/run-ansible-playbook

[19] https://github.com/cypress-io/github-action/blob/master/.github/workflows/example-basic.yml

[20] https://github.com/orgs/community/discussions/49648

[21] https://stackoverflow.com/questions/74048180/how-to-run-ansible-playbook-from-github-actions-without-using-external-action

[22] https://github.com/actions/starter-workflows/actions

[23] https://www.youtube.com/watch?v=Dpo4j2YAilM

[24] https://github.com/google-github-actions/example-workflows
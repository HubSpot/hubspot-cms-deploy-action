# HubSpot CMS Deploy Action

Automatically deploy a HubSpot CMS project to your account 🚀



## Usage
In your GitHub repo, create one new [secret](https://docs.github.com/en/free-pro-team@latest/actions/reference/encrypted-secrets#creating-encrypted-secrets-for-a-repository) for:
- `HUBSPOT_PERSONAL_ACCESS_KEY` - Your [personal access key](https://developers.hubspot.com/docs/cms/personal-cms-access-key)

Then create a [variable](https://docs.github.com/en/actions/writing-workflows/choosing-what-your-workflow-does/store-information-in-variables#creating-configuration-variables-for-a-repository) (in the same interface as secrets):
- `HUBSPOT_ACCOUNT_ID` - This is your [HubSpot account ID](https://knowledge.hubspot.com/account-management/manage-multiple-hubspot-accounts#:~:text=Check%20your%20current%20account,name%20and%20unique%20Hub%20ID.)

This guide walks through setting up a new workflow file that automatically uploads new changes on your `main` branch to your HubSpot CMS account. If you're adding a deployment step to an existing workflow, you can [skip ahead](#integrating-into-an-existing-workflow).

1. In your project, create a GitHub Action workflow file at `.github/workflows/main.yml`
2. Copy the following example workflow into your `main.yml` file.
```yaml
on:
  push:
    branches:
    - main
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4
      - name: HubSpot Deploy Action
        uses: HubSpot/hubspot-cms-deploy-action@v2
        with:
          src_dir: <src> ## ex. src
          dest_dir: <src> ## ex. my-theme
          account_id: ${{ vars.hubspot_account_id || secrets.hubspot_portal_id }}
          personal_access_key: ${{ secrets.hubspot_personal_access_key }}
```
3. Replace the `src_dir` with the directory of your CMS project in your repo
4. Replace the `dest_dir` with the directory it should be uploaded to in your target account
5. Commit and merge your changes

*Note:* Do not change the `account_id` or `personal_access_key` values in your workflow. Auth related values should only be stored as GitHub secrets.

### Deploying to a staging account
If you'd like to auto-deploy to a staging account you have in HubSpot, you can create an additional workflow that runs on `push` to your associated stanging branch in your repo.
```yaml
on:
  push:
    branches:
    - qa
...
account_id: ${{ vars.hubspot_account_id || secrets.hubspot_portal_id }}
```

### Integrating into an existing workflow
To add HubSpot CMS deployment as a step in an existing GitHub Action workflow, add the following step:
```yaml
- name: HubSpot Deploy Action
  uses: HubSpot/hubspot-cms-deploy-action@v2.0.1
  with:
    src_dir: <src> ## ex. src
    dest_dir: <src> ## ex. my-theme
    account_id: ${{ vars.hubspot_account_id || secrets.hubspot_portal_id }}
    personal_access_key: ${{ secrets.hubspot_personal_access_key }}
```

*Note:* You can configure your action to run based on different criteria, such as pushing to a QA branch. Learn more about events that trigger actions [here](https://docs.github.com/en/actions/reference/events-that-trigger-workflows).

## Action Spec
### Inputs
- `src_dir` - Project directory relative to the repo
- `dest_dir` - Target directory in HubSpot

### Variables
- `HUBSPOT_ACCOUNT_ID` - Target account id
### Secrets
- `HUBSPOT_PERSONAL_ACCESS_KEY` - Authentication key
#### Deprecated secret
- `HUBSPOT_PORTAL_ID` - Target account id. This was deprecated in favor of `HUBSPOT_ACCOUNT_ID`, this is more consistent with how we refer to accounts, additionally we moved it to be a variable since GitHub variables now exist and allow for you to be able to see and modify the value. The Account ID does not need the same protection that an authentication key does.

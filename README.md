# HubSpot CMS Deploy Action

Automatically deploy a HubSpot CMS project to your account

## Usage
In your GitHub repo, create two new [secrets](https://docs.github.com/en/free-pro-team@latest/actions/reference/encrypted-secrets#creating-encrypted-secrets-for-a-repository) for:
- `HUBSPOT_PORTAL_ID` - This is your HubSpot account ID
- `HUBSPOT_PERSONAL_ACCESS_KEY` - Your [personal access key](https://developers.hubspot.com/docs/cms/personal-cms-access-key)

If  this is your first time setting up a GitHub action for this project, follow the instructions below. If you're adding deployment to an existing workflow, [skip ahead](#integrating-into-an-existing-workflow).

- In your project, create a GitHub Action workflow file at `.github/workflows/main.yml`
- Copy the following example workflow into your `main.yml` file.
```yaml
on:
  push:
    branches:
    - master
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - name: HubSpot Deploy Action
        uses: actions/hubspot-cms-deploy-action # TODO: Confirm this
        with:
          src_dir: <src>
          dest_dir: <src>
          portal_id: ${{ secrets.hubspot_portal_id }}
          personal_access_key: ${{ secrets.hubspot_personal_access_key }}
```
- Replace the `src_dir` with the directory of your CMS project in your repo
- Replace the `dest_dir` with the directory it should be uploaded to in your target account
- Commit and merge your changes

*Note:* Do not change the `portal_id` or `personal_access_key` values in your workflow. Auth related values should only be stored as GitHub secrets.

### Integrating into an existing workflow
To add HubSpot CMS deployment as a step in an existing GitHub Action workflow, add the following step:
```
- name: HubSpot Deploy Action
  uses: actions/hubspot-cms-deploy-action # TODO: Confirm this
  with:
    src_dir: <src>
    dest_dir: <src>
    portal_id: ${{ secrets.hubspot_portal_id }}
    personal_access_key: ${{ secrets.hubspot_personal_access_key }}
```

## Action Spec
### Inputs
- `src_dir` - Project directory in repo
- `dest_dir` - Target directory in HubSpot

### Secrets
- `HUBSPOT_PORTAL_ID` - Target account id
- `HUBSPOT_PERSONAL_ACCESS_KEY` - Authentication key

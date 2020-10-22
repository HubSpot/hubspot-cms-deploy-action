# hubspot-cms-deploy-action

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
        uses: actions/hubspot-cms-deploy-action
        with:
          src_dir: <src>
          dest_dir: <src>
          portal_id: ${{ secrets.hubspot_portal_id }}
          personal_access_key: ${{ secrets.hubspot_personal_access_key }}
```

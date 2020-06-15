# GitHub Action for publish UPM

## Features

* Publish Unity Packages to UPM registry
* Automatic edit `package.json` and `CHANGELOG.md`

## Prepare

### 1. Setup Secrets in Repository Settings

1. Open Secrets settings<br />![image](https://user-images.githubusercontent.com/838945/74607506-af49b980-511c-11ea-9081-e8b23e695068.png)
1. Add new secrets: Name: `NPM_AUTH_TOKEN`, Value: Your auth token of UPM Registry<br />![image](https://user-images.githubusercontent.com/838945/74607552-1cf5e580-511d-11ea-8407-941511298909.png)
1. Add new secrets: Name: `NPM_REGISTRY_URL`, Value: Your UPM Registry URL.<br />![image](https://user-images.githubusercontent.com/838945/84682501-ad7bef80-af70-11ea-9868-4b53a497be3d.png)

### 2. Setup GitHub Actions Workflow

Setup GitHub Actions Workflow as below example.

```yaml
name: Publish UPM Package

on:
  release:
    types: [published]

jobs:
  upm-publish:

    runs-on: ubuntu-latest

    steps:
    - uses: actions/checkout@v2
    - uses: monry/actions-upm-publish@v1
      with:
        npm_registry_url: ${{ secrets.NPM_REGISTRY_URL }}
        npm_auth_token: ${{ secrets.NPM_AUTH_TOKEN }}
```

You can specify directory path of `package.json` by set path into `package_directory_path` (default: `Assets`)

### Note: Registry URL

You can also specify registry URL using `.npmrc` instead of specifying in actions workflow.

```.npmrc
registry=[UPM Registry URL]
```

## Usages

When you create a new release on GitHub, Actions are executed to do the following:

* Update `version` field in `Assets/package.json`
* Add release notes into `Assets/CHANGELOG.md`
* Commit and Push to registry and replace hash of release tag
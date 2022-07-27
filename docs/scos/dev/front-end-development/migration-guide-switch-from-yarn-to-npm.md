---
title: Migration guide - Switch from Yarn to NPM v8
description: Use this guide to migrate the project from Yarn to NPM v8.
template: concept-topic-template
---

This document provides instructions for migrating from YARN to NPM version 8 in your Spryker project.

## Overview

Since the beginning of the Merchant Portal project, Yarn v2 has been used to install dependencies.
If the package versions differ between some packages, it will not install all dependencies correctly.
The issue has been managed internally by updating all package versions simultaneously, but when customers develop the project, this cannot be guaranteed.
Since Yarn has not responded to the reported issue for more than a year, we must switch back to NPM v8, which also supports workspaces.

*Estimated migration time: 2h*

## 1) Update modules

Run the following command to update the modules:

```bash
composer update spryker/chart spryker/dashboard-merchant-portal-gui spryker/discount spryker/gui spryker/gui-table spryker/merchant-profile-merchant-portal-gui spryker/product-merchant-portal-gui spryker/product-offer-merchant-portal-gui spryker/product-relation-gui spryker/sales-merchant-portal-gui spryker/security-merchant-portal-gui spryker/state-machine spryker/user-merchant-portal-gui spryker/zed-ui spryker-shop/product-review-widget spryker-shop/shop-ui
```

## 2) Update configuration files

1. Set up new versions of node/npm in the main `*.yml` files, like `deploy.yml`, `deploy.dev.yml`, and `deploy.ci.yml`:

```yaml
image:
    ...
    node:
        version: 16
        npm: 8
```

{% info_block infoBox "Note" %}

To ensure the CI jobs will run successfully, add the same config part to all 'deploy.*.yml' files used by the frontend.

{% endinfo_block %}

2. Run the following command to pull the latest `docker-sdk` version:

```bash
cd docker && git pull origin master
git log
```

`git log` command will print the table with the last three commits, copy the `hash` of the first commit and replace content in the `.git.docker` file:

```text
// paste `hash` text here
```

{% info_block infoBox "Note" %}

The minimum `hash` text should be `e9ebb666feccae1754792d41e49df3b9f95ef0aa` or higher.

{% endinfo_block %}

3. Update the `package.json`:

```json
{
    ...
    "workspaces": [
        "vendor/spryker/*",
        "vendor/spryker/*/assets/Zed"
    ],
    ...
    "engines": {
        "node": ">=16.0.0",
        "npm": ">=8.0.0"
    },
    ...
    "devDependencies": {
        ...
        "@babel/plugin-proposal-class-properties": "~7.17.12",
        "@babel/plugin-transform-runtime": "~7.17.12",
        "@babel/preset-typescript": "~7.17.12",
        "@spryker/oryx-for-zed": "~2.12.0",
        "babel-loader": "~8.2.5",
        "sass": "~1.51.0",
        ...
    },
    ...
}
```

4. Update the `.travis.yml`:

```yaml
before_install:
  ...
  - nvm install 16 > /dev/null
  - nvm use 16
  ...
```

5. In the root directory, create a new `.npmrc` file with the following content:

```text
legacy-peer-deps=true
```

6. Install dependencies with `frontend:project:install-dependencies` command instead of `frontend:{yves/zed/mp}:install-dependencies`.

7. Delete the following folders/files from the root directory:

- `.yarn` folder
- `.yarnrc.yml` file
- `yarn.lock` file

## 3) Build the project

1. Run the following commands to apply the docker changes:

```bash
docker/sdk boot deploy.dev.yml
docker/sdk up
```

2. Regenerate `package-lock.json`:

```bash
rm -rf package-lock.json
docker/sdk cli npm install
```

3. Run the following commands to build the project using NPM v8:

```bash
rm -rf node_modules && docker/sdk cli rm -rf node_modules
docker/sdk boot deploy.dev.yml
docker/sdk up --build --assets --data
```

{% info_block infoBox "Note" %}

The following commands are deprecated and will work correctly only with the previous versions (using Yarn):

```bash
frontend:yves:install-dependencies
frontend:zed:install-dependencies
frontend:mp:install-dependencies
```

Use the following command to pass all (yves, zed, mp) frontend dependencies:

```bash
frontend:project:install-dependencies
```

{% endinfo_block %}

{% info_block infoBox "Note" %}

If you use 'node' and 'npm' locally, make sure their versions are correct:

```bash
node -v
npm -v
```

To update the versions, use official documentation:

- [node](https://nodejs.org/en/download/package-manager)
- [npm](https://docs.npmjs.com/try-the-latest-stable-version-of-npm)

{% endinfo_block %}
# GitHub Actions recipes

[![code style: prettier](https://img.shields.io/badge/code_style-prettier-ff69b4.svg)](https://github.com/prettier/prettier)
[![GitHub license](https://img.shields.io/badge/license-MIT-blue.svg)](https://github.com/malcodeman/github-actions-recipes/blob/master/LICENSE)

These are some use cases and code snippets to get you started with GitHub Actions.

## Recipes

### Surge deployment

```
name: Deployment

on:
  push:
    branches: [main]
  workflow_dispatch:

jobs:
  deployment:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - uses: actions/setup-node@v2
        with:
          node-version: "14"

      - name: Install packages
        run: npm install

      - name: Build the app
        run: npm run build

      - name: Install Surge
        run: npm install -g surge

      - name: Deploy to surge
        run: surge ./out APP_NAME.surge.sh --token ${{secrets.SURGE_TOKEN}}
```

### Cron job

```
name: Cron

on:
  schedule:
    - cron: "0 9 * * *"
  workflow_dispatch:

jobs:
  cron:
    runs-on: ubuntu-latest
    env:
      NOTION_TOKEN: ${{ secrets.NOTION_TOKEN }}
      NOTION_DATABASE_ID: ${{ secrets.NOTION_DATABASE_ID }}
      SLACK_SIGNING_SECRET: ${{ secrets.SLACK_SIGNING_SECRET }}
      SLACK_BOT_TOKEN: ${{ secrets.SLACK_BOT_TOKEN }}
      SLACK_CHANNEL_ID: ${{ secrets.SLACK_CHANNEL_ID }}
    steps:
      - uses: actions/checkout@v2
      - uses: actions/setup-node@v2
        with:
          node-version: "14"

      - name: Install packages
        run: npm install

      - name: Start the app
        run: npm run start
```

## License

[MIT](./LICENSE)

# GitHub Actions recipes

[![code style: prettier](https://img.shields.io/badge/code_style-prettier-ff69b4.svg)](https://github.com/prettier/prettier)
[![GitHub license](https://img.shields.io/badge/license-MIT-blue.svg)](https://github.com/malcodeman/github-actions-recipes/blob/master/LICENSE)

These are some use cases and code snippets to get you started with GitHub Actions.

## Recipes

### Next.js Static HTML Export Surge deployment

`"build": "next build && next export"`

```yaml
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

### React.js Surge deployment

200.html page is required for [client-side routing](https://surge.sh/help/adding-a-200-page-for-client-side-routing).

```yaml
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

      - name: Rename index file
        run: mv build/index.html build/200.html

      - name: Install Surge
        run: npm install -g surge

      - name: Deploy to surge
        run: surge ./build APP_NAME.surge.sh --token ${{secrets.SURGE_TOKEN}}
```

### Expo

```yaml
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

      - name: Install Expo CLI
        run: npm install -g expo-cli

      - name: Deploy to Expo
        env:
          EXPO_TOKEN: ${{ secrets.EXPO_TOKEN }}
        run: expo publish --non-interactive
```

### Cron job

```yaml
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

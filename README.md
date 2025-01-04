# Sync Figma Variables from FigMayo

This GitHub Action fetches **Figma variables** from the **FigMayo API**, commits a JSON file to a branch in your repository, and creates a **pull request** if changes are detected.

It's designed to be manually invoked to let non-technical contributors make updates to Figma variables in the codebase via the GitHub UI.

---

## **Getting Started with FigMayo**

Before you can use this GHA you need to have your Figma variables pushed to a FigMayo documentation website.

### **1. Sign Up for FigMayo and Get an API Key**

1. Go to [www.figmayo.com](https://www.figmayo.com) and sign up for a **free account**.
2. After signing up, navigate to your [**Account Settings**](https://app.figmayo.com/api/v2/settings) in FigMayo.
3. Copy your **API key** from the settings page.

### **2. Publish Variables to FigMayo Using the Plugin**

1. Open **Figma** and go to the **Plugins menu**.
2. Search for and install the [**FigMayo Plugin**](https://www.figma.com/community/plugin/1426513201495859669).
3. Open the FigMayo plugin in Figma.
4. Paste your **API key** and the **share URL** of your Figma library into the plugin.
5. Click **“Sync Library”** to publish your variables to FigMayo.

### **3. Setup a GitHub Action**

1. Visit [GitHub](https://github.com/settings/tokens) to create a PAT with Pull Request read/write permissions on your repo.
2. Create the `github-pat` and `figmayo-api-key` [action secrets in the repo settings](https://docs.github.com/en/actions/security-for-github-actions/security-guides/using-secrets-in-github-actions).
3. [Create the workflow](https://docs.github.com/en/actions/security-for-github-actions/security-guides/using-secrets-in-github-actions) as shown in the usage example below

👉 For more detailed guidance on the plugin, check the [FigMayo help](https://help.figmayo.com/sites/PUCaV8RF/FigMayo-How-To-Guide/c/277:869?).

## **Inputs**

| Input             | Description                                 | Required |
| ----------------- | ------------------------------------------- | -------- |
| `github-pat`      | To authenticate the gh cli and raise a PR.  | ✅ Yes   |
| `figmayo-api-key` | FigMayo API Key for authentication.         | ✅ Yes   |
| `file-path`       | Path to save the Figma variables JSON file. | ✅ Yes   |

---

## **Outputs**

| Output   | Description                                     |
| -------- | ----------------------------------------------- |
| `pr-url` | URL of the pull request created for the update. |

---

## Prerequisites

Make sure you have synced your Figma variables from the FigMayo plugin Token Studio to a GitHub repo. You
can follow [this](https://help.figmayo.com/sites/PUCaV8RF/FigMayo-How-To-Guide/c/277:869?) guide to set it up.


## **Usage Example**

```yaml
# .github/workflows/sync-figma-variables.yml

name: Sync Figma Variables with FigMayo

on:
  workflow_dispatch: # Manual trigger

jobs:
  sync-tokens:
    runs-on: ubuntu-latest
    steps:
      - uses: figmayo/figmayo-public/actions/sync-figma-variables@sync-figma-variables@v1.0.0
        with:
          github-pat: ${{ secrets.GH_PAT }}
          figmayo-api-key: ${{ secrets.FIGMAYO_API_KEY }}
          file-path: "src/design-tokens.json" # <-- The target path to store the variables JSON in your repo
```

---

## **How It Works**

1. Fetches **Figma variables** from the **FigMayo API** using the provided **API key**.
2. Saves the variables as a JSON file in your repository at the specified path.
3. Checks if the fetched file has changes compared to the existing one.
4. If changes are detected, commits the updates and creates a **pull request** automatically using the gh CLI.

---

## **License**

This project is licensed under the [MIT License](LICENSE).

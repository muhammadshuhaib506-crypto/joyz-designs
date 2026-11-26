# Shopify theme CI/CD

The store theme (joyzdesigns.com) lives in the `theme/` folder. The rest of this
repo (the coming-soon page) is unchanged.

| Workflow | When it runs | What it does |
|---|---|---|
| **Theme check** | Every pull request and push to `main` | Checks the theme code for errors. |
| **PR preview theme** | Every pull request that changes `theme/` | Uploads the change to an unpublished "PR #N preview" theme and comments a preview link. Deleted when the PR closes. |
| **Deploy theme to live store** | Push to `main` (only if `AUTO_DEPLOY_LIVE` = `true`), or by hand | Checks the theme, then uploads it to the live theme. Never deletes store files and never overwrites `config/settings_data.json`, so theme-editor changes are kept. |
| **Sync theme from Shopify** | Daily 02:30 IST, or by hand | Downloads the live theme into `theme/` and commits it, so edits made in Shopify are saved in GitHub. |

## One-time setup

1. **Get a Theme Access password.** In Shopify admin: Apps → search "Theme Access" → install it (free, by Shopify) → Create password → enter your email. Open the email and copy the password (starts with `shptka_`).
2. **Save it in GitHub.** Repo → Settings → Secrets and variables → Actions → New repository secret. Name: `SHOPIFY_CLI_THEME_TOKEN`, value: the password.
3. **Copy the theme into GitHub.** Repo → Actions → "Sync theme from Shopify" → Run workflow. Leave the box empty for the live theme, or type a theme ID (for example `190607655142` for "Joyz Ivory Atelier (preview)").
4. **(Optional) Turn on automatic live deploys.** Settings → Secrets and variables → Actions → Variables → New variable. Name: `AUTO_DEPLOY_LIVE`, value: `true`. After this, every merge to `main` that changes `theme/` goes live.

## Everyday use

Make changes on a branch → open a pull request → check the preview link → merge.

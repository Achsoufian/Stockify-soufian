# Stockify Netlify setup

This version keeps the existing Stockify website but moves authentication to a Netlify Function.

## Before the first deploy

In Netlify, open the Stockify site and go to **Site configuration → Environment variables**. Add this variable:

```text
STOCKIFY_INITIAL_PASSWORD=your-first-password
```

Use a password with at least 8 characters. Keep the variable available to **Functions**. Do not put this password in `script.js` or commit it to GitHub.

## Deploy

Deploy the whole project, including:

- `netlify.toml`
- `package.json`
- `auth-bridge.js`
- `netlify/functions/auth.mjs`
- the existing HTML, CSS, JavaScript, and assets

Netlify will install `@netlify/blobs` and publish the function route `/api/auth/:action`.

## First login

After deployment, log in with the value of `STOCKIFY_INITIAL_PASSWORD`. On first use, the function creates the protected password configuration in Netlify Blobs.

## Change the password

Open **Settings**, scroll to **Administrator security**, enter the current password, enter the new password twice, and click **Change password**. The function increments the global security version and invalidates all existing sessions. Every device must log in again with the new password the next time it checks its session.

## Important reset note

The initial environment variable is used only when the Netlify Blobs configuration does not exist. Changing the environment variable later does not automatically change the active password. Use the Settings panel to change the password.

If the password is lost, remove the `config` record from the `stockify-auth` Netlify Blobs store or ask for a reset implementation before deleting production data.

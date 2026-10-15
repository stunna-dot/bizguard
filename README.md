# BizGuard — Netlify setup

Folder layout:
- public/index.html  — the whole website
- netlify/functions/api.mjs — the server (accounts, saved data)
- netlify.toml, package.json — Netlify settings
- samples/ — two test CSV files

## Deploy (use GitHub or the CLI, not drag-and-drop)
Drag-and-drop may skip installing the server's package, so the login would fail.

Option A: GitHub
1. Create a GitHub repository and upload everything in this folder (not node_modules).
2. In Netlify: Add new site > Import an existing project > pick the repository.
3. Leave the build command empty. Publish directory: public (already in netlify.toml).
4. Before the first deploy finishes, go to Site configuration > Environment variables and add:
   - ADMIN_PASSWORD = the admin password you choose
   - SESSION_SECRET = any long random text
5. Trigger a deploy again (Deploys > Trigger deploy).

Option B: CLI
1. npm install -g netlify-cli
2. In this folder: npm install, then netlify login, then netlify init
3. netlify env:set ADMIN_PASSWORD "your-password"
4. netlify env:set SESSION_SECRET "long random text"
5. netlify deploy --prod

## Optional: replies written from the customer's message
In Netlify environment variables, also add ANTHROPIC_API_KEY (from console.anthropic.com).
Without it, the Complaint Replies tool still works with templates. With it, the pasted comment is read and answered in the customer's language (English, Filipino or Taglish).
Optional: ANTHROPIC_MODEL to choose the model (default claude-sonnet-5-5). Each reply costs a small amount of API credit.

## Using the site
Admin: log in with BizGuard and your ADMIN_PASSWORD.
1. Add business: shop name, owner's email or mobile number, owner's password.
2. Load data: choose Sales transactions or Grab ads report, drop a CSV.
   Try samples/sample-sales.csv and samples/sample-ads.csv.
3. Fix or skip rows with no amount, then Publish to owner's dashboard.
Owner: log in with the email or mobile number and password the admin set.

## Owners upload their own reports
After the admin adds a shop, the owner logs in and uses **Upload report** in the top menu:
1. Download the sales or ads report from GrabMerchant (save as CSV).
2. Choose the report type, drop the file, check the green ticks.
3. Click Update my dashboard. Months in the new file replace the old ones; other months stay.
The dashboard warns the owner when the numbers are 7 or more days old.
Owners can also change their own password on that page. CSV and Excel (.xlsx, .xls) files both work (first sheet, headers in row 1).
Forgot password: the owner sends a request from the login page; the shop shows a "Password reset requested" tag in the admin list, and the admin sets a new password with Reset password.
Admin can rename a shop or change its login with Edit business.
Saved campaigns and creator checks are stored on the owner's account, so they appear on any device. The admin can still upload for a shop.

## CSV columns the site looks for
Sales: a date column, an amount (or total/sales) column, a status column
(words like cancelled or refunded), an item column. Items can be "Chicken silog x2;Iced coffee".
Ads: a date column, a spend column, an orders column.
Dates like 2026-06-03, 06/03/2026 or 15/06/2026 are understood. Files with day-first dates are detected when any day is above 12.
Five wrong password tries lock a login for 15 minutes.

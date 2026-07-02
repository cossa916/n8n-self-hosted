# Google Search Console setup for n8n

This guide connects your self-hosted n8n to the Google Search Console (GSC) API
so the workflow in `workflows/google-search-console-weekly-report.json` can pull
search performance data (queries, pages, clicks, impressions, positions) for
your site — completely free.

## 1. Verify your site in Search Console

Skip this step if the site is already verified.

1. Go to <https://search.google.com/search-console> and sign in with your Google account.
2. Click **Add property** and choose **Domain** (recommended). Enter your domain,
   e.g. `cheshire-landscape-gardener.com`.
3. Google gives you a DNS TXT record. Add it at your domain registrar / DNS
   provider, then click **Verify**. (DNS can take up to an hour to propagate.)

> Note: a **Domain** property is referenced in the API as
> `sc-domain:cheshire-landscape-gardener.com`. If you instead verified a
> **URL-prefix** property, the API site URL is the full URL, e.g.
> `https://cheshire-landscape-gardener.com/`. The workflow's **Config** node
> must match whichever type you verified.

## 2. Create Google API credentials

1. Go to <https://console.cloud.google.com> and create a project (e.g. `n8n-integrations`).
2. **APIs & Services → Library** → search for **Google Search Console API** → **Enable**.
3. **APIs & Services → OAuth consent screen**:
   - User type: **External**, fill in the app name and your email.
   - Add your own Google account under **Test users** (this is enough — the app
     never needs to be published for personal use).
4. **APIs & Services → Credentials → Create credentials → OAuth client ID**:
   - Application type: **Web application**
   - Authorized redirect URI: `https://<your-n8n-domain>/rest/oauth2-credential/callback`
     (e.g. `https://n8n.example.com/rest/oauth2-credential/callback` — must match
     `N8N_EDITOR_BASE_URL` in your `.env`)
5. Save the **Client ID** and **Client Secret**.

## 3. Create the credential in n8n

In the n8n UI: **Credentials → Add credential → OAuth2 API** (the generic one),
then fill in:

| Field | Value |
|---|---|
| Grant Type | Authorization Code |
| Authorization URL | `https://accounts.google.com/o/oauth2/v2/auth` |
| Access Token URL | `https://oauth2.googleapis.com/token` |
| Client ID | from step 2 |
| Client Secret | from step 2 |
| Scope | `https://www.googleapis.com/auth/webmasters.readonly` |
| Auth URI Query Parameters | `access_type=offline&prompt=consent` |
| Authentication | Body |

Name it something like `Google Search Console OAuth2`, click **Connect my
account**, and approve the Google consent screen. (You may see an "unverified
app" warning — click *Advanced → Continue*, that's expected for test-user apps.)

The `access_type=offline&prompt=consent` parameters are required so Google
issues a refresh token — without them the connection stops working after an hour.

## 4. Import and configure the workflow

1. In n8n: **Workflows → Import from file** → select
   `workflows/google-search-console-weekly-report.json`.
2. Open the **Top Queries** and **Top Pages** nodes and select the
   `Google Search Console OAuth2` credential on each.
3. Open the **Config** node and check `siteUrl` matches your verified property
   (see the note in step 1).
4. Run the workflow manually once to test, then toggle it **Active** for the
   Monday 8am schedule.

## What the workflow does

Every Monday at 8am it pulls the last 28 days of data (ending 3 days ago,
because GSC data lags by ~2–3 days) and produces a markdown report with:

- Top 20 search queries with clicks, impressions, CTR, and average position
- Top 20 pages with clicks, impressions, and CTR
- Total clicks and impressions for the period

The report ends up in the **Format Report** node's output. From there you can
add whatever delivery you like — an email node (SMTP/Gmail), Telegram, Slack,
or writing to a Google Sheet.

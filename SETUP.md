# One-time setup (20 min, all free) — do this once, then it's automatic

Instagram blocks auto-posting from personal accounts. You MUST do steps 1-2.

## 1. Convert Instagram (5 min, in the app)
1. Instagram app → profile → ☰ → **Account type and tools** → **Switch to Professional** → **Creator** (or Business)
2. Then **Settings → Business → Facebook → Connect** an existing Facebook Page, or create one at facebook.com/pages/create (can be unpublished/hidden — nobody needs to see it)

## 2. Create free Meta App + get token (10 min, on PC)
1. Go to developers.facebook.com → **My Apps → Create App → Other → Business**
2. In the app dashboard → **Add Product → Instagram Graph API** (or Instagram Platform)
3. **Tools → Graph API Explorer**:
   - Select your App + your Facebook Page → **Generate token** with scopes:
     `instagram_basic, instagram_content_publish, pages_show_list, pages_read_engagement`
   - Copy the short-lived token
4. Convert to 60-day token (in terminal, replace values):
```
curl -G "https://graph.facebook.com/v21.0/oauth/access_token" \
  --data-urlencode "grant_type=fb_exchange_token" \
  --data-urlencode "client_id=YOUR_APP_ID" \
  --data-urlencode "client_secret=YOUR_APP_SECRET" \
  --data-urlencode "fb_exchange_token=SHORT_TOKEN"
```
5. Find your IG Business ID:
```
curl "https://graph.facebook.com/v21.0/YOUR_PAGE_ID?fields=instagram_business_account&access_token=LONG_TOKEN"
```
   → you get `instagram_business_account.id` = your `IG_USER_ID`

## 3. Connect automation (5 min)
**Option A — GitHub Actions (recommended, free, always-on):**
1. Push this folder to a GitHub repo
2. Repo → **Settings → Secrets → Actions → New secret**:
   - `IG_USER_ID` = id from step 2.5
   - `IG_ACCESS_TOKEN` = long token from step 2.4
3. **Actions tab → daily-ig-post → Run workflow** to test. Then it runs daily 09:00 IST automatically.

**Option B — test on your PC first:**
```
pip install -r requirements.txt
cp .env.example .env   # fill IG_USER_ID + IG_ACCESS_TOKEN, set DRY_RUN=false
python auto_post.py
```

## 4. Token refresh (every ~55 days)
Long tokens live 60 days. Refresh before expiry (can be automated later):
```
curl -G "https://graph.facebook.com/v21.0/oauth/access_token" \
  --data-urlencode "grant_type=fb_exchange_token" \
  --data-urlencode "client_id=YOUR_APP_ID" \
  --data-urlencode "client_secret=YOUR_APP_SECRET" \
  --data-urlencode "fb_exchange_token=CURRENT_LONG_TOKEN"
```
→ update the GitHub Secret with the new token.

## Troubleshooting
| Error | Fix |
|---|---|
| `IG container error: OAuth` | Token expired or wrong scopes → regenerate |
| `media processing failed` | Image URL not public → check catbox link opens in incognito |
| `Application request limit` | Don't test more than ~5x/hour |
| Personal account error (`#10`) | Account not Business/Creator or not linked to Page |
| Image model slow | Pollinations free tier queues ~30-60s, workflow retries — normal |

Dry-run (no credentials) always works: `DRY_RUN=true python auto_post.py` → check `output/preview.html`.

# 📸 Insta Auto-Poster — daily 1 post, free

No manual posting. GitHub runs it every morning.

**Stack (all free):** Pollinations.ai Flux (images) + Pollinations text (quotes) + Instagram Graph API + GitHub Actions scheduler. No API keys needed until you go live.

## Quick start
```bash
pip install -r requirements.txt
DRY_RUN=true python auto_post.py
# open output/preview.html
```

## Files
- `auto_post.py` — main bot
- `config.yaml` — niche, themes, hashtags, size ← **edit this to change content**
- `.github/workflows/daily-post.yml` — runs daily 09:00 IST
- `SETUP.md` — one-time Instagram/Meta connection guide (required to go live)
- `.env.example` — credentials template
- `output/` — today's image + caption + preview

## Change your niche
Edit `config.yaml`: `niche`, `themes` (30 ideas), `hashtags`, `width/height`.
Examples: `funny cat illustrations`, `healthy breakfast recipes`, `crypto tips with neon backgrounds`.

## Go live
Follow `SETUP.md` once → add `IG_USER_ID` + `IG_ACCESS_TOKEN` as GitHub Secrets → done. One post/day forever.

## Costs / limits
Free tiers only. Instagram allows 100 API posts/24h (you use 1). Pollinations free tier may queue at peak — workflow waits.

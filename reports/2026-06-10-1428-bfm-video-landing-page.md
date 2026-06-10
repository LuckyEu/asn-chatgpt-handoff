# BFM workflow video landing page

**Verdict:** BFM VIDEO LANDING PAGE PASS

**Date:** 2026-06-10-1428  
**Route:** `/professional-access/bfm-workflow`  
**Branch:** `feat/bfm-professional-video-page`  
**Commit:** `bf2ab1a8`  
**PR:** #92

---

## Summary

Added a professional-facing ASN landing page for the v5.7 BFM teaser. Video is embedded from public handoff assets with VTT captions and an Open video fallback. CTAs link to pilot@ mailto and `/professional-access`.

After merge and deploy, attorneys should use `https://www.affidavitsupport.net/professional-access/bfm-workflow` instead of raw GitHub MP4 links.

---

## Page

| Section | Content |
|---------|---------|
| Hero | A cleaner workflow for marriage support statements. |
| Subhead | Guide applicants/witnesses · separate statements · review before signing |
| Video | Handoff MP4 + VTT · controls · fallback link |
| Cards | Clear instructions · Witness-authored facts · Attorney review trail |
| CTAs | Email pilot team · Request professional pilot access |
| Disclaimer | Workflow infrastructure · not a law firm · not legal advice |

---

## Video source

- MP4: `https://raw.githubusercontent.com/LuckyEu/asn-chatgpt-handoff/main/artifacts/bfm-video-current/bfm-teaser-90s.mp4`
- VTT: `https://raw.githubusercontent.com/LuckyEu/asn-chatgpt-handoff/main/artifacts/bfm-video-current/bfm-teaser-90s.vtt`

---

## Checks

| Check | Result |
|-------|--------|
| `pnpm exec tsc --noEmit` | PASS |
| `bfmWorkflowLandingPage.test.tsx` | PASS (4) |
| `bfmWorkflowVideoPlayer.test.tsx` | PASS (1) |
| Forbidden claims grep | none |
| Sitemap entry | added |

---

## Safety

No production writes · no emails sent · no payments · no forms added · no outreach sent.

---

## Not done

- Deploy to production (await merge + normal deploy)
- Outreach email

---

**BFM VIDEO LANDING PAGE PASS**

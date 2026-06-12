# Latest — BFM declaration quality fix merged — PR #97

**Verdict:** PR97_MERGE_PASS  
**Date:** 2026-06-11  
**Archive:** reports/2026-06-11-1733-pr97-bfm-declaration-quality-merge.md  
**Product PR:** https://github.com/LuckyEu/affidavit-support-network/pull/97

## Summary

- PR #97 merged to production at commit `6c78318a`.
- Global BFM declaration-generation fix — not a Joe case-specific patch.
- Fixes relationship sentence assembly, observation category labels (PAIR_PRO UI copy), BFM legal-awareness wording, `Couple:` PDF header, relationship context preservation, and default ID image embedding.
- Removes default fraud/dishonesty boilerplate from BFM relationship-proof declarations.
- Preserves GMC/N-400 behavior and §1746 declaration path.
- Joe production row/PDF was not touched.
- PR #96 (attorney review queue) remains open and must be rebased onto current `main` before merge.

## Production identity

- `GET https://www.affidavitsupport.net/api/build-info` → commit `6c78318a…`, ref `main`, env `production`
- No production write smoke or PDF regeneration in this pass.

## Next action

1. Rebase and merge PR #96 when ready.
2. Optional: preview BFM declaration PDF regen smoke with synthetic fixture (not Joe production row).

Synthetic demo · Not legal advice.

# Unified Brand Repository Merge Plan

## Objective
Merge all Aioverse/Aiotize brand design assets, documentation, and code-based design system from 5 source repositories into this single canonical repository (`AIO3-HQ/Aioverse-BrandNew-V--master-starter-`).

## Source Repositories

| # | Repository | Role | Unique Content |
|---|---|---|---|
| 1 | `AIO3-HQ/Aioverse-BrandNew-V--master-starter-` | **Canonical target** (this repo) | Base brand docs, tokens, folder scaffold |
| 2 | `Shivansh9000/Aioverse-BrandNew` | Most evolved fork | `docs/`, `SUMMARY.md`, `gitbook.yaml`, richer `FOLDER_STRUCTURE.md`, logos content |
| 3 | `Shivansh9000/glowing-disco` | Font + asset binary store | `Nohemi.zip`, `Tokyo Trail Mono Font.zip`, `nebula_2.zip` |
| 4 | `Shivansh9000/branding-system` | Code-based design system | Turborepo monorepo with ESLint, Prettier, token packages |
| 5 | `jcklpe/open-source-branding-toolkit` | External reference template | Structural scaffold patterns only |

## Target Unified Folder Structure

```
/
├── README.md                        # Master index (keep AIO3-HQ version + add GitBook badge)
├── SUMMARY.md                       # GitBook ToC (from Shivansh9000/Aioverse-BrandNew)
├── gitbook.yaml                     # GitBook config (from Shivansh9000/Aioverse-BrandNew)
├── CHANGELOG.md                     # New: version history
├── BRAND_INDEX.md                   # New: map of every doc to its purpose
├── UNIFIED_BRAND_MERGE_PLAN.md      # This file
├── tokens                           # Design tokens (shared SHA - already present)
├── fonts/
│   └── zips/
│       ├── Nohemi.zip               # From glowing-disco
│       ├── Tokyo Trail Mono Font.zip # From glowing-disco
│       └── nebula_2.zip             # From glowing-disco
├── logos/                           # From Aioverse-BrandNew (has actual content)
├── icons/
├── illustrations/
├── photos/
├── guidelines/
├── docs/                            # From Shivansh9000/Aioverse-BrandNew
├── brand-docs/
│   ├── AIOTIZE_BRAND_PERSONA.md
│   ├── AIOVERSE_BRAND_IMPLEMENTATION.md
│   ├── AIOVERSE_ECOSYSTEM.md
│   ├── AIOVERSE_NAMING.md
│   ├── AIOVERSE_USAGE_GUIDE.md
│   ├── AIOVERSE_VISUAL_SYSTEMS.md
│   ├── BRAND_GUIDELINES_CHECKLIST.md
│   ├── FIGMA_NOTION_SETUP_GUIDE.md
│   └── FOLDER_STRUCTURE.md         # Use larger version from Aioverse-BrandNew
└── design-system/                   # Turborepo monorepo from branding-system repo
    ├── packages/
    ├── apps/
    ├── turbo.json
    └── package.json
```

## Merge Steps (Git Commands)

```bash
# 1. Clone canonical repo
git clone https://github.com/AIO3-HQ/Aioverse-BrandNew-V--master-starter-.git unified-brand
cd unified-brand

# 2. Bring in unique content from Aioverse-BrandNew
git remote add aioverse-new https://github.com/Shivansh9000/Aioverse-BrandNew.git
git fetch aioverse-new
git checkout aioverse-new/main -- docs/ gitbook.yaml SUMMARY.md

# 3. Bring in font binaries from glowing-disco
git remote add glowing-disco https://github.com/Shivansh9000/glowing-disco.git
git fetch glowing-disco
git checkout glowing-disco/main -- "Nohemi.zip" "Tokyo Trail  Mono Font.zip" nebula_2.zip
mkdir -p fonts/zips
mv Nohemi.zip "Tokyo Trail  Mono Font.zip" nebula_2.zip fonts/zips/

# 4. Add branding-system code as design-system/ subtree
git subtree add --prefix=design-system \
  https://github.com/Shivansh9000/branding-system.git main --squash

# 5. Use larger FOLDER_STRUCTURE.md from Aioverse-BrandNew
git checkout aioverse-new/main -- FOLDER_STRUCTURE.md

# 6. Commit all merged content
git add .
git commit -m "feat: unified brand merge - docs, fonts, design-system, gitbook config"
git push origin main
```

## Conflict Resolution Rules

| File | Resolution | Reason |
|---|---|---|
| `README.md` | Keep AIO3-HQ version (4,874 bytes) | Richer content; add GitBook badge |
| `FOLDER_STRUCTURE.md` | Use Aioverse-BrandNew version (1,557 bytes) | Larger, more complete |
| `tokens` | Keep existing (SHA `96d9b50` is identical across all repos) | No conflict |
| `AIOVERSE_ECOSYSTEM.md` | Keep existing (SHA `98954fe` is identical across all repos) | No conflict |
| All other `.md` docs | Keep AIO3-HQ versions as base; review diffs from Aioverse-BrandNew | Merge if newer content exists |

## Post-Merge Actions

- [ ] Archive `Shivansh9000/glowing-disco` (assets absorbed; name is confusing)
- [ ] Archive `Shivansh9000/Aioverse-BrandNew` (fork superseded)
- [ ] Keep `Shivansh9000/branding-system` as standalone but link from root README
- [ ] Set `AIO3-HQ/Aioverse-BrandNew-V--master-starter-` as the canonical source in Notion
- [ ] Add GitBook integration pointing to `SUMMARY.md`
- [ ] Create `BRAND_INDEX.md` and `CHANGELOG.md`
- [ ] Tag first unified release as `v1.0.0-unified`

## Copilot Tasks for This PR

Copilot is requested to:
1. Review this merge plan for completeness and flag any missed files or conflicts
2. Suggest a `BRAND_INDEX.md` template mapping each doc to its purpose and audience
3. Suggest a `CHANGELOG.md` initial entry for the unified merge
4. Review `FOLDER_STRUCTURE.md` and recommend updates to reflect the new `design-system/` and `docs/` additions
5. Flag any duplicate content between brand doc files that should be consolidated

---
*Created: March 11, 2026 | Aiotize Inc. Brand Team*

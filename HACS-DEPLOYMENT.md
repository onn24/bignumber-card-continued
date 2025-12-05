# HACS Deployment Checklist

This document outlines the steps to prepare and submit bignumber-card-continued to HACS (Home Assistant Community Store).

## Pre-Deployment Checklist

### Repository Setup

- [x] Repository is public on GitHub: https://github.com/sxdjt/bignumber-card-continued
- [x] Apache-2.0 License file present
- [x] README.md with installation and configuration instructions
- [x] hacs.json properly configured
- [ ] .github/workflows/validate.yml added for HACS validation
- [ ] About section on GitHub repo configured (description, topics, website)

### File Structure Requirements

HACS requires JavaScript files in one of these locations (searched in order):
1. `dist/` directory (not used - single file project)
2. Latest GitHub release (RECOMMENDED)
3. Repository root (current location)

Current structure:
```
bignumber-card-continued/
├── bignumber-card.js          # Main file (HACS will use this)
├── bignumber-card-beta.js     # Beta testing (in .gitignore, not distributed)
├── hacs.json                  # HACS configuration
├── README.md                  # User documentation
├── CHANGELOG.md               # Version history
├── LICENSE                    # Apache-2.0
└── .gitignore                 # Excludes beta and CLAUDE.md
```

### hacs.json Verification

Current configuration:
```json
{
  "name": "Big Number Card - Continued",
  "filename": "bignumber-card.js",
  "render_readme": true,
  "homeassistant": "2021.3.0"
}
```

Requirements:
- [x] `name` matches card display name
- [x] `filename` matches actual JavaScript file
- [x] `render_readme` set to true
- [x] `homeassistant` minimum version specified

## GitHub Actions Setup

### 1. Create HACS Validation Workflow

Create `.github/workflows/validate.yml`:

```yaml
name: Validate

on:
  push:
  pull_request:
  schedule:
    - cron: "0 0 * * *"
  workflow_dispatch:

permissions: {}

jobs:
  validate-hacs:
    runs-on: "ubuntu-latest"
    steps:
      - name: HACS validation
        uses: "hacs/action@main"
        with:
          category: "plugin"
```

This workflow:
- Runs on every push
- Runs daily via cron
- Can be manually triggered
- Validates HACS compatibility

### 2. Run Validation

- [ ] Push workflow file to GitHub
- [ ] Wait for workflow to complete (check Actions tab)
- [ ] Verify all checks pass (no errors)
- [ ] Document successful workflow run URL

## Version Preparation

### Decide Version Number

Options:
- `0.0.7-continued` - Next increment from original v0.0.6
- `1.0.0-continued` - Fresh start for continuation

Recommended: **`1.0.0-continued`** for clarity

### Update Version References

Version appears in these locations:
- [ ] hacs.json (if version field is added - currently not present)
- [ ] CHANGELOG.md (update [Unreleased] to [1.0.0-continued])
- [ ] README.md (add version badge if desired)

### CHANGELOG.md Update

Change from:
```markdown
## [Unreleased]
```

To:
```markdown
## [1.0.0-continued] - 2025-12-05

### Added
...

## [Unreleased]

(empty for now)
```

## Create GitHub Release

### 1. Tag the Release

```bash
git tag -a v1.0.0-continued -m "Release v1.0.0-continued: Community continuation with locale formatting, custom fonts, tap actions, and critical bug fixes"
git push origin v1.0.0-continued
```

### 2. Create GitHub Release

On GitHub (https://github.com/sxdjt/bignumber-card-continued/releases):

1. Click "Create a new release"
2. Choose tag: `v1.0.0-continued`
3. Release title: `v1.0.0-continued`
4. Description (use CHANGELOG.md content):

```markdown
# Big Number Card - Continued v1.0.0

Community continuation of the original bignumber-card, which has not been maintained since January 2022.

## What's New

### Added
- Locale-aware number formatting with automatic thousands separators
  - Uses browser locale (e.g., 19,578 in US, 19.578 in German)
  - Respects existing `round` configuration
- Customizable font sizes and padding
  - New `title_font_size`, `value_font_size`, `card_padding` options
  - Decouple card dimensions from font sizing
- Configurable tap actions
  - Support for: more-info, toggle, call-service, navigate, url, none
  - Fully backwards compatible (defaults to more-info)

### Fixed
- None/NaN detection bug (checks numeric vs formatted value)
- Fixed typo: `nonestring` → `noneString`
- Added error handling for missing/undefined entities
- Card gracefully handles non-existent entities with console warning

### Changed
- Updated documentation with comprehensive examples
- Added extensive code comments for maintainability

## Credits

Original card by [@ciotlosm](https://github.com/ciotlosm) and contributors. This continuation maintains their excellent work while incorporating community improvements.

## Installation

Install via HACS or manually copy `bignumber-card.js` to `/config/www/`.

Full documentation: https://github.com/sxdjt/bignumber-card-continued
```

5. Attach files: `bignumber-card.js` (drag and drop the file)
6. Click "Publish release"

### 3. Verify Release

- [ ] Release appears at: https://github.com/sxdjt/bignumber-card-continued/releases
- [ ] `bignumber-card.js` is attached to the release
- [ ] Tag is visible in repository tags

## HACS Submission

### Prepare Submission Information

1. **Current Release URL**:
   - Example: `https://github.com/sxdjt/bignumber-card-continued/releases/tag/v1.0.0-continued`

2. **HACS Validation Run URL** (from GitHub Actions):
   - Go to: https://github.com/sxdjt/bignumber-card-continued/actions
   - Find successful "Validate" workflow run
   - Copy URL (e.g., `https://github.com/sxdjt/bignumber-card-continued/actions/runs/...`)
   - MUST NOT have `ignore` key in hacs.json

3. **Repository Topics** (set in GitHub):
   - `home-assistant`
   - `hacs`
   - `lovelace-card`
   - `homeassistant-custom-card`

### Submit to HACS

DO NOT submit until ALL items above are complete.

1. Fork https://github.com/hacs/default
2. Add repository to `plugin` file
3. Create pull request using template
4. Fill out ALL checklist items
5. Provide ALL required links
6. DO NOT request reviews (automated process)

Pull request template: https://raw.githubusercontent.com/hacs/default/master/.github/PULL_REQUEST_TEMPLATE.md

### Post-Submission

- [ ] PR passes automated checks
- [ ] PR is merged by HACS maintainers
- [ ] Card appears in HACS default repository
- [ ] Test installation via HACS in Home Assistant

## Repository Settings

### GitHub About Section

Set these on GitHub repository homepage:

- **Description**: "Community-maintained continuation of bignumber-card for Home Assistant - Display sensor values with large numbers, severity colors, and progress bars"
- **Website**: `https://github.com/sxdjt/bignumber-card-continued`
- **Topics**: `home-assistant`, `hacs`, `lovelace`, `custom-card`, `homeassistant-lovelace`

### Branch Protection (Optional but Recommended)

- Protect `main` branch
- Require status checks (validate workflow)
- Require pull request reviews for contributions

## Post-Deployment Maintenance

### Future Releases

1. Update CHANGELOG.md with changes
2. Commit all changes
3. Create new tag: `git tag -a vX.Y.Z-continued -m "Release message"`
4. Push tag: `git push origin vX.Y.Z-continued`
5. Create GitHub release with `bignumber-card.js` attached
6. HACS automatically detects new releases

### Monitoring

- Watch GitHub Issues for bug reports
- Monitor HACS discussions/forums
- Keep dependencies updated (none currently)
- Review and merge community PRs

## Troubleshooting

### HACS Validation Fails

Common issues:
- `hacs.json` has syntax errors → validate JSON
- JavaScript file missing from release → attach to release
- Workflow has `ignore` key → remove from hacs.json
- File naming mismatch → ensure `filename` in hacs.json matches actual file

### Release Not Detected

- Ensure tag starts with `v` (e.g., `v1.0.0-continued`)
- Verify JavaScript file is attached to release
- Check file is in root directory (not in subdirectory)
- Clear HACS cache in Home Assistant

## Reference Documentation

- HACS Plugin Publishing: https://www.hacs.xyz/docs/publish/plugin
- HACS Action: https://github.com/hacs/action
- PR Template: https://github.com/hacs/default/blob/master/.github/PULL_REQUEST_TEMPLATE.md
- Boilerplate Card: https://github.com/custom-cards/boilerplate-card

## Ready to Deploy?

Complete ALL checkboxes above before submitting to HACS.

Current Status: **NOT READY** (missing GitHub Actions workflow)

Next Steps:
1. Create `.github/workflows/validate.yml`
2. Push and verify workflow passes
3. Create v1.0.0-continued release
4. Submit to HACS

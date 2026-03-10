# Changelog - Academic Website Development

## 2026-03-10
### Added
- **GitHub Actions Workflow**: Created `.github/workflows/hugo.yml` for automated site deployment to GitHub Pages.
- **Theme Support**: Integrated the **Blowfish** theme as a git submodule to ensure the build environment on GitHub has access to the theme files.
- **Modern Hugo Support**: Upgraded Hugo version to **0.145.0** in GitHub Actions to fix template parsing errors (`try` function deficiency in older versions).

### Changed
- **Deployment Trigger**: Updated workflow to listen on the `master` branch instead of only `main` to match the repository's configuration.
- **Remote Configuration**: Successfully pushed the local project to `https://github.com/SuuTTT/dingli-su.github.io.git`.

### Fixed
- Fixed deployment crash caused by missing theme files (`modules/blowfish not found`).
- Fixed Hugo build error: `function "try" not defined` by upgrading Hugo.
- Established successful connection between local repository and GitHub.

---

## Planned (TODO)
- Enhance the index layout visual for a more professional PhD landing page.
- Create a 'Wiki' section with a tutorial on Setting up Hugo with GitHub Actions.
- Update the Publications page with authentic links from Google Scholar.
- Fix the 404 issue on the CV page for PDF viewing/downloading.

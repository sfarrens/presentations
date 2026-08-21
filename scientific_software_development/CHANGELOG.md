# Changelog

All notable changes to these slides will be documented in this file.

## [v2026.08]

### Added

- New "Type Checking" section covering `mypy`, inserted between Linting and Documentation
- "Quality Control" grouping (Unit Testing, Linting, Type Checking) and "Further Considerations" grouping (Additional Tests) in the overview
- Licence slide in Deployment (MIT, with community-standard alternatives noted in a footnote)
- GitHub community-standards slide (`CONTRIBUTING.md`, `CODE_OF_CONDUCT.md`, `SECURITY.md`, issue/PR templates, `CITATION.cff`)
- Version badge and "Report an issue" button on the cover slide
- New "Profiling" section (`timeit`, `cProfile`, `line_profiler`, `memray`) under Further Considerations
- New "Using AI" section: coding assistants, vibe coding, precautions, and community standards around AI-assisted contributions
- "Release" grouping (Publishing, Reproducible Research) in the overview
- Two-column table-of-contents layout on the overview slide, so it no longer needs a tiny font or a split across multiple slides

### Changed

- Modernized the whole workflow around `uv`: dependency groups, `uv add`/`uv sync`/`uv run`, `uv build`/`uv publish`
- Replaced the unmaintained `pytest-pydocstyle` with `ruff`'s docstring rules
- Verification tests (Additional Tests) moved to a dedicated `verify` dependency group and excluded from the default `pytest` run
- CI lint jobs (GitHub Actions and GitLab CI) now also run `mypy`
- Footnotes renumbered to match their order of appearance in the slides
- "Deployment" renamed to "Publishing", to avoid confusion with CI/CD's "continuous deployment"
- Unit Testing now ignores `.coverage` before committing, rather than only mentioning it in a footnote

### Fixed

- Unit Testing's deliberate-bug demo was missing the step to revert the bug afterwards
- Documentation's `hubble` docstring is now a raw string (`r"""`), needed for the embedded LaTeX to render correctly
- Several missing commands (e.g. `touch .github/workflows/cd.yml`) and missing `git commit`/branch steps (Deployment, Reproducible Research) that left the repository in an inconsistent state partway through those sections
- Various typos and stale or duplicated footnote content
- Documentation's `hubble`/`critical_density` docstrings now use the numpydoc-required plural "Examples" heading — the singular "Example" is silently dropped from the built docs
- Stale cross-references between sections left behind after later content was inserted

# Changelog

All notable changes to **Arabic Slug Schema Guard** are documented here. The
format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/), and the
project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.1.1]

### Changed
- Documentation and inline-comment cleanup in the README and the plugin source. No functional changes.

## [1.1.0]

### Added
- Layer-2 self-test: the slug-generation fork now diffs itself against core's
  live `sanitize_title_with_dashes()` on short fixtures (once per core version),
  so a future core rewrite of that function is caught immediately instead of
  drifting silently.
- `ASG_L2_FAILSAFE` constant: on detected drift, fall back to core's generator
  (slugs cap at 200) until the fork is re-synced, rather than run a copy that is
  known to have diverged.

### Changed
- The Layer-2 install is now conditional on the drift status, and the
  `asg_l2_status` option is autoloaded so the fail-safe is read on every request.

## [1.0.0]

### Added
- Initial release. A WordPress must-use plugin that stops core database upgrades
  from truncating long Arabic (and other non-Latin) slugs:
  - **Layer 1 (prevention):** a `dbdelta_create_queries` filter that keeps
    `wp_posts.post_name` and `wp_terms.slug` at `VARCHAR(1024)`, so dbDelta never
    emits a destructive `CHANGE COLUMN`.
  - **Layer 2 (generation):** a copy of `sanitize_title_with_dashes()` that
    raises the byte cap, so new slugs are not cut at 200 bytes.
  - **Tripwire:** post-update verification of the real column widths (logs and
    optionally emails on a revert) plus a `wp asg verify` WP-CLI command.

[1.1.1]: https://github.com/mantekio/arabic-slug-schema-guard/releases/tag/v1.1.1
[1.1.0]: https://github.com/mantekio/arabic-slug-schema-guard/releases/tag/v1.1.0
[1.0.0]: https://github.com/mantekio/arabic-slug-schema-guard/releases/tag/v1.0.0

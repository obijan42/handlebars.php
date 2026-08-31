# Changelog

All notable changes to this project are documented here.
This project adheres to [Semantic Versioning](https://semver.org/).

## [1.0.0] - 2026-08-30

First release of this maintained fork of the abandoned
[`xamin/handlebars.php`](https://github.com/XaminProject/handlebars.php)
(based on its `develop` branch). Runs cleanly on PHP 7.2 through 8.x.

### Security
- **Default escaping now uses `ENT_QUOTES` instead of `ENT_COMPAT`.** Single
  quotes are now escaped, closing an XSS hole for values interpolated into
  single-quoted HTML attributes. This matches the behavior of Handlebars.js.
- `FilesystemLoader` now enforces that a resolved template/partial path stays
  inside the configured base directory, preventing `../` path traversal.
- The `Disk` cache now calls `unserialize()` with `['allowed_classes' => false]`,
  removing an object-injection sink when the cache directory is writable.

### Fixed
- Fatal error on PHP 7/8 in the `APC` cache: switched from the removed `apc_*`
  functions to `apcu_*` (APCu).
- `TypeError` on PHP 8 when rendering a `{{#section}}` over a non-`Countable`
  `Traversable` (e.g. a `Generator`); `count()` is now guarded.
- Removed the reserved `\Handlebars\String` class fatal on PHP 7+.
- `continue` inside `switch` warnings in the tokenizer (PHP 7.3+).
- Various PHP 8.1+ deprecation notices: null passed to string functions,
  out-of-bounds string offset reads, and `list()` on short `explode()` results.

### Changed
- **BREAKING:** the long-deprecated `\Handlebars\String` class was removed.
  Use `\Handlebars\StringWrapper` instead.
- Autoloading migrated from PSR-0 to PSR-4.
- Modernized the test suite to PHPUnit 11 and added GitHub Actions CI
  (PHP 7.4–8.4).

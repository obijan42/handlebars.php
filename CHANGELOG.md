# Changelog

All notable changes to this project are documented here.
This project adheres to [Semantic Versioning](https://semver.org/).

## [1.0.3] - 2026-08-31

### Fixed
- Removed `implements \Stringable` from `BaseString` and `Arguments`. The
  `\Stringable` interface only exists on PHP 8.0+, so v1.0.0–v1.0.2 fatally
  errored on PHP 7.2–7.4 despite the `>=7.2` requirement. On PHP 8 a class with
  `__toString()` implements `Stringable` implicitly, so behavior is unchanged.
- CI now runs the test suite on PHP 8.1–8.4 (the suite requires PHPUnit 10/11)
  and separately lint + smoke-tests the library on PHP 7.2–8.0, so the full
  supported range is actually exercised.

## [1.0.2] - 2026-08-30

### Changed
- Declared `replace: { "xamin/handlebars.php": "*" }` so this fork transparently
  satisfies any dependency on the abandoned `xamin/handlebars.php`, preventing
  both packages (which share the `Handlebars\` namespace) from being installed
  side-by-side.

## [1.0.1] - 2026-08-30

### Changed
- Added `.gitattributes` with `export-ignore` rules so the Composer dist
  tarball no longer ships tests, CI config, and other development-only files.

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

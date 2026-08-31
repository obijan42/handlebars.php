# Security Policy

## Reporting a vulnerability

Please report suspected vulnerabilities privately via GitHub Security Advisories
("Report a vulnerability" on the repository's Security tab) rather than opening a
public issue.

## Notes on the threat model

This library renders templates and data to HTML/text. Keep the following in mind:

- **Escaping.** `{{ value }}` HTML-escapes with `ENT_QUOTES` by default (both
  single and double quotes). `{{{ value }}}` and `{{& value }}` intentionally do
  **not** escape — only use them with trusted content. `\Handlebars\SafeString`
  also suppresses escaping by design.
- **Untrusted templates.** If you render templates authored by untrusted users,
  be aware that a template can invoke public zero-argument methods on objects
  present in the render data (e.g. `{{user.someMethod}}`). Do not pass objects
  with sensitive or side-effecting zero-arg methods into untrusted templates.
- **FilesystemLoader.** Template/partial names are constrained to the configured
  base directory; `../` traversal outside it is rejected.
- **Disk cache.** Cached data is deserialized with object instantiation disabled
  (`allowed_classes => false`). Still, treat the cache directory as trusted.

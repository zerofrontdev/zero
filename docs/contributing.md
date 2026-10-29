# Contributing

Zero is open source (OSL-3.0 or AFL-3.0, the same as Magento Open Source).
Bug reports, fixes, compat modules for popular extensions and documentation
are all welcome.

## Where

Every package has its own repository at **github.com/zerofrontdev**: the
theme in `theme-frontend-zero`, each module in `module-zero-<name>`, the
metapackage in `zero`. Open issues and pull requests on the repository of
the package concerned. The packages are released together from one
development repository, so a merged change ships in the next release of
every package it touches.

## A good issue

- Magento and PHP versions, and Zero's: `composer show 'zerofront/*'`.
- What you did, what you expected, what happened; the URL path and a
  screenshot for anything visual.
- The output of `bin/magento zero:compat:report` when another extension is
  involved.

Security problems: don't open a public issue. Report them privately to the
maintainer (Roman Pasichnyk, on GitHub or LinkedIn) and allow time for a
release with the fix.

## A good pull request

- One change per pull request, with the reason in its description.
- Follows how the code around it is written; Magento's coding standard
  (`vendor/bin/phpcs --standard=Magento2`) with no errors.
- Storefront changes keep the rules in [getting-started.md](getting-started.md):
  server-rendered markup, HTMX before JavaScript, no inline scripts,
  visitor-specific content as fragments, WCAG 2.2 AA.
- Unit tests for logic (`Test/Unit` in the module), run with Magento's
  `dev/tests/unit/phpunit.xml.dist`.
- No measurable loss in the numbers [performance.md](performance.md) tracks.

## Compat modules

A compat module makes someone else's extension work on Zero without
touching it ([extensions.md](extensions.md) §2). If you made one for a
widely used extension, publish it as its own Composer package and open an
issue on `zero` with the link: popular ones get listed in the docs.

## Documentation

These pages live in the theme's `docs/` folder and on zerofront.dev/docs.
Corrections are pull requests on `theme-frontend-zero`.

# Upgrading Zero

Zero's packages share one version number and follow semantic versioning:
a patch release (1.3.2 → 1.3.3) fixes things, a minor release (1.3 → 1.4)
adds modules and features without breaking what you built on, a major
release (2.0) may change templates, settings or extension points and says
what to do in its release notes.

## Update

```bash
composer update 'zerofront/*' --with-dependencies
bin/magento setup:upgrade
bin/magento cache:flush
```

In production mode, the usual `setup:di:compile` and
`setup:static-content:deploy` after it.

`zerofront/zero` requires its packages with `^1.x`, so `composer update`
stays within the major version. To see what would change first:

```bash
composer outdated 'zerofront/*'
```

The changes of each release are on each package's GitHub page
(github.com/zerofrontdev), under Releases and in the commit history.

## After updating

- **Rebuild the CSS** if your store adds classes the shipped CSS doesn't
  have: a child theme with its own Tailwind classes, or modules (yours or
  compat modules) with `view/frontend/zero/tailwind.css`. The theme's
  packaged `web/css/app.css` knows only Zero's own templates.

  ```bash
  cd vendor/zerofront/theme-frontend-zero && npm ci && npm run build
  ```

  [build.md](build.md) has the details.

- **Compare your template overrides.** A child theme's copy of a Zero
  template doesn't receive the fixes made to the original. After an
  upgrade, diff your overrides against the new originals:

  ```bash
  for f in $(cd app/design/frontend/<Vendor>/<theme> && find . -name '*.phtml'); do
      orig="vendor/zerofront/theme-frontend-zero/$f"
      [ -f "$orig" ] && diff -u "$orig" "app/design/frontend/<Vendor>/<theme>/$f" > /dev/null || echo "check $f"
  done
  ```

  Templates overridden from a module (`Zero_Checkout/templates/...`) live
  in `vendor/zerofront/module-zero-<name>/view/frontend/templates/`.

- **Run the compat report again** (`bin/magento zero:compat:report`): a
  new Zero release can cover a module you adapted or rendered with Luma.

## Magento upgrades

Zero supports Magento Open Source 2.4.7–2.4.9. Before upgrading Magento,
check that the Zero version you run supports the target version (each
package's README states it); upgrade Zero first when it doesn't.

## Going back

Composer can pin the previous version:

```bash
composer require 'zerofront/zero:1.3.2' --update-with-dependencies
bin/magento setup:upgrade
```

Database changes the newer release made (new tables, settings, content)
stay in place, and an earlier version of the same major release runs with
them. Going back across a major version: follow its release notes.

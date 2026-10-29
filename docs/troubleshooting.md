# Troubleshooting

The problems people meet in their first weeks with Zero, what causes each,
and the fix.

## A module's block shows, but clicking it does nothing

The module draws its storefront with Luma's JavaScript (`x-magento-init`,
`data-mage-init`, Knockout). Zero doesn't load RequireJS, so those widgets
never start. `bin/magento zero:compat:report` shows it in the "Luma JS"
column. Fix: a compat module, or render the module's pages with Luma for now
([extensions.md](extensions.md) §2–§3).

## New classes in my template have no effect

Tailwind generates only the classes it finds in the files it scans. A class
used for the first time in your template isn't in the built CSS until the
build sees that template:

- in a child theme: rebuild the CSS ([child-themes.md](child-themes.md) §4);
- in a module: add `view/frontend/zero/tailwind.css` with an `@source` line
  for its templates, then rebuild ([build.md](build.md)).

Using classes the theme already uses works without a build.

## The page looks unstyled after a deploy

The theme's CSS wasn't built or wasn't deployed. Check that
`web/css/app.css` exists in the theme and in `pub/static/frontend/Zero/…`.
In production mode run `setup:static-content:deploy` after building. The
build must run inside the Magento project, where `app/etc/config.php` and
`vendor/` are: it reads the module list from them.

## The mini cart, the account link or prices for a group are wrong for some visitors

Something visitor-specific got into the page cache. On Zero, anything that
depends on the visitor must be a fragment loaded after the page
([extensions.md](extensions.md) §4), never rendered into the cached HTML. A
third-party block marked `cacheable="false"` also turns the whole page
uncached: remove that and make it a fragment instead.

## The browser console shows "Refused to execute inline script" or a blocked host

Zero runs under a strict Content Security Policy. Move inline code into a
static file loaded with `<script src="…" defer>`, pass data in `data-`
attributes or `<script type="application/json">`, and add any outside host
to the module's `etc/csp_whitelist.xml`.

## A payment method is missing at checkout

Zero Checkout lists only methods it can complete. A third-party method
needs Zero support, or keep the checkout on Luma for now: turn Zero Checkout
off and add `checkout/*` to Pages on Another Theme
([migrating.md](migrating.md) §4).

## Product images are slow the first time

Zero makes resized JPEG and WebP copies on the first request for each size.
Make them ahead of time after an import or a deploy:

```bash
bin/magento zero:images:warm
```

If no WebP copies appear, PHP's GD lacks WebP
(`php -r 'var_dump(function_exists("imagewebp"));'`).

## A page on Luma still shows the Zero header (or the other way round)

Pages on Another Theme matches `front name/controller/action` and the URL
path, one rule per line, `*` as a wildcard. Check the rule against the
page's route: its layout handle is the same three parts joined by `_`
(`rma/guest/view` is `rma_guest_view`). Then flush the cache: the theme is
chosen before the page is cached.

## The score in PageSpeed Insights dropped

Usually a new script, a large image above the fold, a font, or a page that
fell out of the cache. [performance.md](performance.md) has what each number
rests on and how to find what changed.

## Still stuck

Open an issue on the package's repository at github.com/zerofrontdev with
the Magento and PHP versions, Zero's version (`composer show 'zerofront/*'`),
what you did and what you saw: [contributing.md](contributing.md).

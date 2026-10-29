# Getting started

From nothing to a running Zero store you can change, and the few ideas
that explain how every page works. Thirty minutes on a machine that already
runs Magento.

## What you need

- Magento Open Source **2.4.7, 2.4.8 or 2.4.9**, installed and working
  with Luma.
- **PHP 8.2–8.4** with GD built with WebP:
  `php -r 'var_dump(function_exists("imagewebp"));'` says `true`. Without it
  Zero serves the original images instead of WebP copies.
- **Node 20+** and npm, only to rebuild the CSS (the packages ship it built).
- Developer mode while you work: `bin/magento deploy:mode:set developer`.

## Install

```bash
composer require zerofront/zero
bin/magento setup:upgrade
bin/magento cache:flush
```

`zerofront/zero` brings the theme and the modules every store needs: the
checkout, search, SEO, CMS, banners, the blog, mail, abandoned carts and the
payment modules. Any other module installs on its own with
`composer require zerofront/module-zero-<name>`, now or later; the list is in
the package's README and on zerofront.dev/modules.

Then pick the theme: **Content > Design > Configuration**, your store view,
**Applied Theme: Zero**. Open the storefront.

Modules that need an account somewhere (payments, analytics, social login,
Amazon SES) wait under **Stores > Configuration > Zero** until you enter
their keys. Everything else is on.

To see what Zero looks like with a full catalog first, try the demo store:
https://demo.zerofront.dev.

## Where things are

| What | Composer install | Notes |
| --- | --- | --- |
| The theme | `vendor/zerofront/theme-frontend-zero` | registered as `Zero/default` |
| The theme's sources | `src/styles/*.css` in the theme | Tailwind v4 + daisyUI, built into `web/css/app.css` |
| The theme's JavaScript | `web/js/` in the theme | plain files, no bundler; htmx is one of them |
| Core storefront code | `vendor/zerofront/module-zero-core` | view models, HTMX fragments, the session request, compat registry |
| The checkout | `vendor/zerofront/module-zero-checkout` | one page, one form, no Knockout |
| Docs | `docs/` in the theme | the same pages as zerofront.dev/docs |

Your own work never goes into `vendor/`: a look of your own is a **child
theme** in `app/design/frontend/<Vendor>/<theme>` ([child-themes.md](child-themes.md)),
code is a module in `app/code` or your own package.

## How a page works

Four ideas cover almost everything.

**1. The server renders the page.** Templates are `.phtml` with Tailwind and
daisyUI classes. There is no RequireJS, jQuery or Knockout on the
storefront, and no `x-magento-init`. A page arrives complete; nothing is
assembled in the browser.

**2. Every page is cached; the visitor's parts come in after.** The full
page cache (Varnish or Magento's own) serves the same HTML to everyone. What
depends on the visitor (the mini cart count, "Hi, Anna", a wishlist heart)
is a small **fragment**: a controller that is never cached, loaded with
`hx-get` once the visitor's session is known. The theme fires
`zero:session-ready` when it is; other events are `cart:updated`,
`wishlist:updated` and `zero:consent`. How to write one:
[extensions.md](extensions.md) §4.

**3. HTMX before JavaScript.** Forms post and swap HTML with `hx-post` and
`hx-target`. A script is the last resort (a payment SDK, a gallery zoom): a
static file in your module's `view/frontend/web/js/`, loaded with
`<script src="…" defer>` and wired by `data-` attributes.

**4. A strict Content Security Policy.** No inline scripts, no `onclick=""`,
no `eval`. Data for a script goes in `data-` attributes or
`<script type="application/json">`. Hosts a module talks to are listed in
its `etc/csp_whitelist.xml`.

## Your first change

1. **Restyle with tokens, no build.** Colors, corners and fonts are CSS
   variables in a child theme: [child-themes.md](child-themes.md) §2.
2. **Override a template.** Copy it into your child theme under the same
   `Vendor_Module/templates/…` path and edit. Classes the theme already uses
   work at once.
3. **New Tailwind classes need a build.** Tailwind only generates classes it
   finds in the files it scans:

   ```bash
   cd vendor/zerofront/theme-frontend-zero   # or app/design/frontend/Zero/default
   npm ci
   npm run build        # once, and after enabling or disabling a module with styles
   npm run dev:css      # while editing: rebuilds on save
   ```

   Every command and when to run it: [build.md](build.md).

4. **Look at the components.** Every UI component (dialogs, menus, forms,
   badges) is rendered live with its markup in the admin under
   **Content > Design > UI Components** (the storefront's `/zero/ui/`):
   [components.md](components.md).

## Check a third-party module

```bash
bin/magento zero:compat:report
```

It lists every enabled module with a storefront and whether it works on
Zero, needs a compat module, or should render its pages with Luma for now.
What to do with each: [extensions.md](extensions.md). Moving a whole store:
[migrating.md](migrating.md).

## Going live

Production mode as usual, plus Zero's image copies made ahead of time:

```bash
bin/magento deploy:mode:set production
bin/magento zero:images:warm      # JPEG and WebP product image copies
```

Before a deploy, check the numbers: [performance.md](performance.md).

# Moving a store to Zero

A live store moves to Zero without a rewrite of its catalog, orders,
customers or admin: Zero is a theme and modules on the same Magento. What
changes is the storefront, and the parts of extensions that drew it with
Luma's JavaScript. This page is the order of work for one store, from a
copy to the switch and back if needed.

## In short

1. Work on a copy of the store.
2. Install Zero there; the live store keeps its theme.
3. Run the compat report and decide, module by module.
4. Check payments and shipping in Zero Checkout.
5. Rebuild the look as a child theme.
6. Test the paths shoppers take.
7. Switch the theme on the live store; switching back is one setting.

## 1. A copy of the store

A staging copy with the live database (customers' personal data scrubbed if
your rules ask for it), the same Magento version, the same modules. Zero
supports Magento Open Source 2.4.7–2.4.9; a store on an older version
upgrades Magento first, as a separate project.

## 2. Install Zero on the copy

```bash
composer require zerofront/zero
bin/magento setup:upgrade
```

Installing changes nothing a shopper sees until a store view uses the Zero
theme. Assign it on the copy only: **Content > Design > Configuration**.
Several store views can move one at a time.

## 3. The compat report

```bash
bin/magento zero:compat:report
```

For every enabled module with a storefront it shows how many templates it
has, how many use Luma's JavaScript (RequireJS, Knockout), and how it works
on Zero. Put each module in one of four groups:

| Group | Typical module | What to do |
| --- | --- | --- |
| **Works as is** | admin-only, back-end, feeds, ERP links, emails | nothing |
| **Restyle** | storefront markup without Luma JavaScript | add `view/frontend/zero/tailwind.css` or a compat module with Zero templates |
| **Rewrite the interactive part** | widgets on `x-magento-init`, Knockout UI components | a compat module: HTMX or a small script instead of the widget |
| **Not now** | rarely used pages, complex account sections | render those pages with Luma: **Pages on Another Theme** |

Everything about the four options, with examples: [extensions.md](extensions.md).
Zero already covers a lot that stores usually buy as extensions (layered
navigation, search, SEO, reviews, gift cards, reward points, returns,
quick order, store pickup…): a module whose job Zero does can often be
switched off instead of adapted. The list: zerofront.dev/modules.

## 4. Payments and shipping

Zero Checkout is one page with one form and no Knockout. It lists only the
payment methods it can complete:

- Magento's offline methods (check/money order, bank transfer, cash on
  delivery, purchase order) and the free method;
- Braintree, and through Zero's own modules: Stripe, PayPal, Authorize.Net,
  Adyen, Klarna, Mollie, Amazon Pay (plus regional ones).

**A payment module without Zero support** doesn't appear. Either:

- adapt it: it registers its method with Zero Checkout and, for an off-site
  page, hands over the URL to send the shopper to (Zero_Checkout README,
  "Add a payment method"), or
- keep the whole checkout on Luma for now: turn **Zero Checkout** off
  (**Stores > Configuration > Zero > Checkout**) and add `checkout/*` to
  **Pages on Another Theme**. The rest of the store stays on Zero. Zero's
  own payment methods are hidden on the Luma checkout.

Shipping carriers that only calculate rates work as they are. A carrier with
its own fields at checkout (a pickup point, a delivery date) adds them to
the `zero.checkout.shipping.after` container; checkout fields, validation
before the order and actions after it: the Zero_Checkout README.

Test each method with a real order on the copy: paid, refunded, cancelled.

## 5. The look

Don't port Luma's LESS. Rebuild the look as a Zero **child theme**: brand
colors, corners and fonts are tokens (no build), templates are overridden
one by one where the design differs: [child-themes.md](child-themes.md).
Most stores need tokens, the header, the footer and the home page; the
catalog, product and checkout pages already follow the tokens.

CMS pages and blocks keep working. Content written for Luma's classes or
Page Builder looks plainer on Zero until it is restyled; new pages can be
built with Zero CMS ([cms.md](cms.md)).

## 6. Test

Walk the paths shoppers take, on a phone first:

- home → category with filters → product with options → cart → checkout →
  paid order → order email → My Account;
- search with a typo, an out-of-stock product, a coupon, a gift card;
- sign in, register, reset a password, the wishlist;
- every page in **Pages on Another Theme**.

Then the numbers: [performance.md](performance.md) has the budgets and how to
check before a deploy.

## 7. The switch, and back

On the live store: deploy the same code and modules as the copy, then set
the store view's theme to your Zero child theme and flush the cache.
URLs, orders, customers and SEO settings don't change.

To go back, set the previous theme again and flush the cache. Keep the old
theme deployed for the first weeks.

## Coming from a custom theme

The steps are the same; what carries over depends on what the old theme is
built on.

| The store's theme | What carries over | What to redo |
| --- | --- | --- |
| **A Luma child theme** (most custom themes) | its look as a reference; layout XML that only moves or removes blocks often carries over | LESS → tokens and Tailwind classes; templates with `x-magento-init` or Knockout → Zero templates |
| **A purchased theme** (Porto, Ultimo, Claue…) | nothing of the theme itself | its look as a Zero child theme; its bundled modules (sliders, mega menus, popups) go through the compat report like any module, and many have a Zero module already (Banner, MegaMenu, QuickView) |
| **Hyvä** | Tailwind habits, much of the markup's structure | Alpine.js → HTMX or a small script; Tailwind v3 config → v4 tokens; Hyvä compatibility modules → Zero compat modules |
| **Headless / PWA** (PWA Studio, Vue Storefront, Next.js) | the back end and its data | the storefront is Magento's again: one deployment instead of two, nothing to keep in sync |

A rule of thumb for a custom theme: list its overridden templates
(`find app/design/frontend/<Vendor>/<theme> -name '*.phtml'`); the ones that
only change markup and text are quick on Zero, the ones with JavaScript are
where the time goes.

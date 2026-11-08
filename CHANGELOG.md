# Changelog: zerofront/zero

## 1.3.7

One new free module: Personalize, product recommendations from Amazon Personalize. The other packages only follow the version number. Update with `composer update 'zerofront/*' -W`, then `bin/magento setup:upgrade`.

Packages that changed:

- [`zerofront/module-zero-personalize`](https://github.com/zerofrontdev/module-zero-personalize/blob/main/CHANGELOG.md)

## 1.3.6

Fixes from a review of every module: payments, money, sign-in, privacy and stability. Several modules change behaviour on purpose: read the entries of the packages you use before updating, in particular Stripe (cards are authorised first and captured after the order is saved), Abandoned Cart (guests need a newsletter subscription), Social Login (an existing account is connected only after a password sign-in or an emailed link) and Checkout (a payment for a cancelled order is flagged, not booked).

The packages use each other's new code (Zero Checkout's payment settlement, for one), so each 1.3.6 package requires the others at `^1.3.6`: update them together, `composer update 'zerofront/*' -W`, then `bin/magento setup:upgrade`, which adds the new tables and columns.

New required Magento modules, for stores that `replace` core modules: Abandoned Cart needs Magento_Newsletter; Blog and Checkout Magento_Cron; Consent Magento_Email and Magento_Integration; Core Magento_Swatches; Index Doctor Magento_CatalogRuleConfigurable; Social Login Magento_Email, Magento_Integration and Magento_Persistent.

- `docs/extensions.md`: Google reCAPTCHA on Zero forms, "Request a Price" on product cards of your own, `main_control_html` for the product page's main control, `Zero\Core\Model\Pricing\Amounts` for amounts already in the display currency.
- `docs/child-themes.md`: the session bootstrap's `data-zero-fragment` placeholders are not to be overridden.
- `docs/social-login.md`: when a sign-in service connects to an existing account (a password sign-in first, or an emailed link).

Packages that changed:

- [`zerofront/theme-frontend-zero`](https://github.com/zerofrontdev/theme-frontend-zero/blob/main/CHANGELOG.md)
- [`zerofront/module-zero-core`](https://github.com/zerofrontdev/module-zero-core/blob/main/CHANGELOG.md)
- [`zerofront/module-zero-checkout`](https://github.com/zerofrontdev/module-zero-checkout/blob/main/CHANGELOG.md)
- [`zerofront/module-zero-cms`](https://github.com/zerofrontdev/module-zero-cms/blob/main/CHANGELOG.md)
- [`zerofront/module-zero-blog`](https://github.com/zerofrontdev/module-zero-blog/blob/main/CHANGELOG.md)
- [`zerofront/module-zero-search`](https://github.com/zerofrontdev/module-zero-search/blob/main/CHANGELOG.md)
- [`zerofront/module-zero-layered-navigation`](https://github.com/zerofrontdev/module-zero-layered-navigation/blob/main/CHANGELOG.md)
- [`zerofront/module-zero-brands`](https://github.com/zerofrontdev/module-zero-brands/blob/main/CHANGELOG.md)
- [`zerofront/module-zero-seo`](https://github.com/zerofrontdev/module-zero-seo/blob/main/CHANGELOG.md)
- [`zerofront/module-zero-reviews`](https://github.com/zerofrontdev/module-zero-reviews/blob/main/CHANGELOG.md)
- [`zerofront/module-zero-stock-alert`](https://github.com/zerofrontdev/module-zero-stock-alert/blob/main/CHANGELOG.md)
- [`zerofront/module-zero-promotions`](https://github.com/zerofrontdev/module-zero-promotions/blob/main/CHANGELOG.md)
- [`zerofront/module-zero-gift-card`](https://github.com/zerofrontdev/module-zero-gift-card/blob/main/CHANGELOG.md)
- [`zerofront/module-zero-quote-request`](https://github.com/zerofrontdev/module-zero-quote-request/blob/main/CHANGELOG.md)
- [`zerofront/module-zero-abandoned-cart`](https://github.com/zerofrontdev/module-zero-abandoned-cart/blob/main/CHANGELOG.md)
- [`zerofront/module-zero-product-feed`](https://github.com/zerofrontdev/module-zero-product-feed/blob/main/CHANGELOG.md)
- [`zerofront/module-zero-consent`](https://github.com/zerofrontdev/module-zero-consent/blob/main/CHANGELOG.md)
- [`zerofront/module-zero-social-login`](https://github.com/zerofrontdev/module-zero-social-login/blob/main/CHANGELOG.md)
- [`zerofront/module-zero-index-doctor`](https://github.com/zerofrontdev/module-zero-index-doctor/blob/main/CHANGELOG.md)
- [`zerofront/module-zero-cache-warmer`](https://github.com/zerofrontdev/module-zero-cache-warmer/blob/main/CHANGELOG.md)
- [`zerofront/module-zero-mail`](https://github.com/zerofrontdev/module-zero-mail/blob/main/CHANGELOG.md)
- [`zerofront/module-zero-paypal`](https://github.com/zerofrontdev/module-zero-paypal/blob/main/CHANGELOG.md)
- [`zerofront/module-zero-stripe`](https://github.com/zerofrontdev/module-zero-stripe/blob/main/CHANGELOG.md)
- [`zerofront/module-zero-authorize-net`](https://github.com/zerofrontdev/module-zero-authorize-net/blob/main/CHANGELOG.md)
- [`zerofront/module-zero-amazon-pay`](https://github.com/zerofrontdev/module-zero-amazon-pay/blob/main/CHANGELOG.md)
- [`zerofront/module-zero-adyen`](https://github.com/zerofrontdev/module-zero-adyen/blob/main/CHANGELOG.md)
- [`zerofront/module-zero-klarna`](https://github.com/zerofrontdev/module-zero-klarna/blob/main/CHANGELOG.md)
- [`zerofront/module-zero-mollie`](https://github.com/zerofrontdev/module-zero-mollie/blob/main/CHANGELOG.md)
- [`zerofront/module-zero-quick-view`](https://github.com/zerofrontdev/module-zero-quick-view/blob/main/CHANGELOG.md)
- [`zerofront/module-zero-redirects`](https://github.com/zerofrontdev/module-zero-redirects/blob/main/CHANGELOG.md)
- [`zerofront/module-zero-store-pickup`](https://github.com/zerofrontdev/module-zero-store-pickup/blob/main/CHANGELOG.md)
- [`zerofront/module-zero-product-labels`](https://github.com/zerofrontdev/module-zero-product-labels/blob/main/CHANGELOG.md)
- [`zerofront/module-zero-size-chart`](https://github.com/zerofrontdev/module-zero-size-chart/blob/main/CHANGELOG.md)
- [`zerofront/module-zero-product-attachments`](https://github.com/zerofrontdev/module-zero-product-attachments/blob/main/CHANGELOG.md)
- [`zerofront/module-zero-delivery-date`](https://github.com/zerofrontdev/module-zero-delivery-date/blob/main/CHANGELOG.md)
- [`zerofront/module-zero-quick-order`](https://github.com/zerofrontdev/module-zero-quick-order/blob/main/CHANGELOG.md)
- [`zerofront/module-zero-returns`](https://github.com/zerofrontdev/module-zero-returns/blob/main/CHANGELOG.md)
- [`zerofront/module-zero-free-shipping-bar`](https://github.com/zerofrontdev/module-zero-free-shipping-bar/blob/main/CHANGELOG.md)
- [`zerofront/module-zero-bought-together`](https://github.com/zerofrontdev/module-zero-bought-together/blob/main/CHANGELOG.md)

Earlier releases (1.3.5 and before): see the Git tags.

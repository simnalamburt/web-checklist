webpage checklist
========

- [k6](https://k6.io/): Load testing tool
  - [goose](https://book.goose.rs/): If you need a faster alternative
- [SSL Server Test](https://www.ssllabs.com/ssltest): Check HTTPS settings. You should get A+ and HSTS preload and Certificate Transparency should be enabled.
  - [SSL Configuration Generator](https://mozilla.github.io/server-side-tls/ssl-config-generator/): Useful if you have a problem with SSL configuration.
  - [gixy](https://github.com/yandex/gixy): Nginx configuration static analyzer. Useful if you have a problem with Nginx configuration.
- [securityheaders.com](https://securityheaders.com/): Check security headers. You should get A+.
- [Lighthouse](https://developer.chrome.com/docs/lighthouse/overview): Check your website's performance, accessibility, SEO, and best practices.
  - [PageSpeed Insights](https://pagespeed.web.dev/): Lighthouse + CrUX. Identical to Lighthouse if your website lacks traffic.
- [validator.w3.org](https://validator.w3.org/): Markup Validation Service (i.e. HTML5, SVG, MathML, etc)
  - [css-validator](https://jigsaw.w3.org/css-validator/): CSS Validation Service
- [Sharing Debugger](https://developers.facebook.com/tools/debug/): Check how your website looks when shared on Facebook. For X (formerly Twitter), [use Post Composer instead](https://devcommunity.x.com/t/card-validator-preview-removal/175006)
- [Google Analytics](https://developers.google.com/analytics): Track your website's traffic
  - [Mixpanel](https://mixpanel.com/): Hard-core analytics
  - [Cloudflare Web Analytics](https://www.cloudflare.com/web-analytics/): If you want to ditch Google
  - Opensource alternatives: [Plausible](https://plausible.io/), [GoAccess](https://goaccess.io/), [Swetrix](https://swetrix.com/)
- [Favicon checker](https://realfavicongenerator.net/checker): Check if your favicon is correctly installed. It checks not only the favicon in the browser's address bar but also how it will be displayed when you create a shortcut to the page.

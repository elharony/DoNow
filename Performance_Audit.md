According to Google Lighthouse Performance Audit tool, our competitor (http://todolistme.net/)[http://todolistme.net/] has the following key performance numbers:
- Performance: 88
- Best Practices: 71
- Accessibility: 38
- SEO: 78

Let's discuss both `Performance` and `Best Practices` a little more in-depth.

## 1) Performance
There are so many "bad practices" affects the website performance, here are some of them:
- Using too many blocking-external JS files. For example; They are using (Twitter Widgets)[http://platform.twitter.com/widgets.js], (Google Analytics)[https://www.google-analytics.com/ga.js], Google Ads [(1)[http://pagead2.googlesyndication.com/pagead/show_ads.js], (2)[http://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js]], (Facebook Integration)[http://connect.facebook.net/en_US/all.js#xfbml=1], and more!

**Solution:** Although most of them are essentials at some point in the application, we can use `async` and `defer` approaches. Basically, we will tell JS to wait, until the rest of the page loads, then those "non-critical" JS files will be loaded `Asynchronously`!

- Don't specify an expiration date for the files.

**Solution:** Set a Cache Expiration date for all external resources.

- Minify the JS files. Minifying means deleting all whitespaces/lines, and condense all the code. It's one of the most common "bad practices" to use unmified files on production!

**Solution:** We export a minified JS files for the production build of the website, and we always leave the unmified version for developers to maintain the code. It's very common to use any Task Runner (i.e. (Webpack)[https://webpack.js.org/], (Gulp)[https://gulpjs.com/], (Grunt)[https://gruntjs.com/]) to do this task!

- There are 20 requests to get very small images. Total website requests are 64 requests (According to (GTMetrix)[https://gtmetrix.com/reports/todolistme.net/KPFKwECz]), so it's 1/3 of the total website requests, and once fixed, it will dramatically improve the overall performance.

**Solution:** There is a technique called: `Combine images using CSS sprites`. We use it to combine tiny/small images into one single image and use the `background-position` CSS Property to get the exact icon/tiny image we need for each element. Now, we only 1 request, not 20. This technique is applied by - almost - all big companies. For example; Amazon wraps all Localization/Language switcher & Country Flags in one image, (check it out!)[https://m.media-amazon.com/images/G/01/AUIClients/InternationalCustomerPreferencesNavAssets-icp_sprite-0b528ccc99b2eed18447291de6df851bc2c6fe68._V2_.png]

## 2) Best Practices
Although `Best Practices` don't affect the Website Loading Time as the `Performance` issues do, but it really affects the overall User Experience... Here are some problems, and possible ways to solve:
- The competitor site doesn't have an SSL Certificate installed. Basically, all requests are sending via `HTTP` not `HTTPS` which isn't secured, and the users will notice the "Not Secure" label whenever they visit the website.

**Solution:** It needs a valid SSL Certificate, which could be either purchased via the Hosting Provider/3rd-party Website or get one for free using LetsEncrypt (I wrote an article about it a while ago, check it out & install SSL Certificate in 5 minutes!)[https://www.elharony.com/get-your-free-ssl-certificate-with-lets-encrypt/]

- It doesn't use `HTTPS` to get external resources. It looks like the same thing, right? Not really. Many websites have SSL Certificate Installed, but they still use `HTTP` resources!

**Solution:** After installing SSL Certificate on the website, make sure that ALL external sources (i.e. Images, CSS, JS) are using `HTTPS` not `HTTP`

- This website is using a Vulnerable Version of jQuery (jQuery@2.2.4). Which isn't secure anymore to use!

**Solution:** There are 2 possible solutions here; Get rid of jQuery, and convert the codebase to Vanilla JavaScript (Which can do pretty much everything now!). Or update the jQuery version to a recent one!


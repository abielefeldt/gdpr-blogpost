# Google Analytics and GDPR Compliance in Drupal 7

Let's say you're a site owner or a web developer. Has GDPR been on your mind lately? If you read the title and clicked on this blog post out of more than a passing curiosity, then you're most likely the target audience.

As of this writing, the May 25th deadline for implementing compliance with the EU’s General Data Protection Regulation is just around the corner. Numerous blog posts and thinkpieces have been published about GDPR and what it means for site owners and developers, but there isn’t a single definitive, authoritative source for figuring out whether a website needs to comply with GDPR, and if it does, what specifically needs to be done to become compliant. Those questions can generally only be confidently answered by a site owner’s legal counsel. (And yes, that absolutely means that this blog post is not authoritative either, merely informative. Talk to your lawyers!)

### What GDPR Means

The most visible manifestation of GDPR compliance on the web is the appearance of “cookie consent pop-ups.” If you’re like me, you probably found these to be an unnecessary annoyance and thought, well, of course this website uses cookies! Every website uses cookies! If you try to block all cookies in your browser then everything breaks!

However, the point of GDPR isn’t to regulate each and every cookie that a website uses. GDPR does not care about cookies that just get set to a 1 or a 0. Even session cookies used to track the fact that a user is logged in aren’t the focus of GDPR since logging into a website implies acceptance of a session cookie -- as long as the session cookie itself does not consist of personally identifiable information (PII) such as an email, name, or IP address.

The primary concern of GDPR in regards to cookies is *tracking*. If you’ve ever looked up a product on Amazon, and then logged in to Facebook to find an ad for that very product in your feed, that’s all due to tracking cookies. When a user “accepts cookies,” what they’re really doing is “consenting” to allow themselves to be tracked. Many websites which have cookie consent pop-ups aren’t actually compliant under GDPR as they assume that any user that uses their website consents to be tracked, and if they don't, they should just not visit, or turn off tracking in their browser, etc. This is not acceptable -- users need to be given the option to opt out of analytics tracking on the site itself.

### Google Analytics and Becoming GDPR Compliant

If your website is found to be subject to GDPR requirements, then one service that you probably use which is specifically covered by GDPR is Google Analytics tracking. Under GDPR, if you use Google Analytics, you most likely need to take specific steps towards compliance:

* Inform users that the site uses cookies to track users.
* Provide a transparent, easily-accessible method of opting out of tracking without restricting access to the site in any way.
* Provide a way for users to delete their own Google Analytics tracking data. (This will be provided by Google Analytics at some point in the hopefully very near future.)
* Avoid storing PII in cookies and tracking systems (IP addresses, emails, etc.)

### Becoming (Almost) Compliant With EU Cookie Compliance

If your site runs on Drupal, then there is a very good module which you can use to knock out the first task: The [EU Cookie Compliance module](https://www.drupal.org/project/eu_cookie_compliance). In fact, you may already have this module installed and configured.

However, this module does not come with any way for users to opt out of Google Analytics tracking out of the box. It provides functionality for developers to determine whether a user has accepted using cookies or not, but it does not actually block tracking without some extra work. The default configuration provided by this module also has many of the same mistakes that noncompliant websites end up making. If you need a cookie consent pop-up, then in order to be compliant, you can’t just use the default configuration for this module.

### Committing To Full Compliance

To address the issues with the EU Cookie Compliance module, I have created my own module for Drupal 7 which overrides the default configuration and will disable Google Analytics tracking for users who have not agreed to the cookie pop-up, as long as GA is provided by the google_analytics module.

[Screenshots for comparison]

This module disables Google Analytics by executing the following line of Javascript code in the `<head>` before the analytics code is included:

```
window['ga-disable-UA-xxxxxxx-yy'] = true; // UA-xxxxxxx-yy is your Google Analytics ID
```

Yes, that’s really it. If a user hasn’t consented to Google Analytics tracking, all that’s needed is to run this code before the ga() function is called. This is easy enough for a developer to do, and the project page for the EU Cookie Compliance module addresses this by linking to [a comment](http://drupal.org/node/1648286#comment-6145800) in the issue queue. But if you have to set up and configure this module on a lot of sites, then that's a lot of work that needs to be done.

If you're in need of a module that you can install and it will "just work," then go ahead and grab the module. See if it works for you. If it doesn't, you're encouraged to modify it to suit your needs. It's a "sandbox" module which means unless you manually download newer versions you'll never have to worry about your changes getting overwritten.

[View the F1 GDPR module page on Drupal.org](https://www.drupal.org/sandbox/caesius/2973123)

To download it, you'll need to clone the repository using git. See the [Version Control tab](https://www.drupal.org/project/2973123/git-instructions) for instructions.

### Workflow For Compliance And Consent

Our client, The German Marshall Fund of the United States ([GMFUS](http://gmfus.org/)), found that they needed to be GDPR compliant and were happy to be the test bed for the F1 GDPR module.

Once the module was installed, the only thing that needed to be updated was shortening "The German Marshall Fund of the United States" (their site name) to "GMFUS" in the pop-up text. Here's what the popup looks like on desktop and mobile:

![Desktop consent popup](https://puu.sh/AoAZ2.png)  
*The consent pop-up on desktop*

![Desktop accept popup](https://puu.sh/AoAZg.png)  
*The acceptance pop-up on desktop*

![Mobile consent popup](https://puu.sh/AoBfh.png)  
*The consent pop-up on mobile*

![Mobile accept popup](https://puu.sh/AoBa3.png)  
*The acceptance pop-up on mobile*

Note the differences between the desktop and mobile pop-ups. The reasoning is that the popup is not sufficiently intrusive on desktop to require a way to dismiss it immediately without accepting cookies. On mobile, however, the pop-up is very intrusive, so we need to allow mobile users to dismiss it without accepting cookies. In either case, users will need to be able to update their preference later. This functionality should be available on a Cookie Policy or Privacy Policy page that is linked from the pop-up and easily found in the site navigation, e.g. the footer.

The module comes with an HTML file that has code for embedding a button in a page for enabling and disabling tracking cookies. The module Readme has instructions for including this button.

![Cookie policy toggle button](https://puu.sh/AoBss.png)  
*The button for toggling Google Analytics on and off.*

### Compliance Goes Beyond Google Analytics

This blog post and accompanying module largely only covers the role of Google Analytics in GDPR compliance. Site owners are responsible for becoming familiar with GDPR and identifying other potentially noncompliant functionality. How else might you be using PII? Don't just throw a pop-up or module on your site and assume you're compliant. Assume the EU will take GDPR compliance seriously and that you are a "big enough fish" to get caught up in it.


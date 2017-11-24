---
template: chapter.pug
title: 'SitePoint' 
chaptertitle: 'Which Browsers Should Your Website Support?'
chapterauthor: Craig Buckler 
---


 

*This chapter is part of a series created in partnership with
[SiteGround](https://www.siteground.com/go/article-sp). Thank you for
supporting the partners who make SitePoint possible.* 

**The question *"which browsers should my website/app support?"* is
often raised by clients and developers. The simple answer is a list of
the top N mainstream applications. But has that policy become
irrelevant?**

What are the Most-used Browsers?
--------------------------------

The top ten desktop browsers according to
[StatCounter](http://gs.statcounter.com/) for May 2017 were:

1.  Chrome --- 59.37% market share
2.  Firefox --- 12.76%
3.  Safari --- 10.55%
4.  IE --- 8.32%
5.  Edge --- 3.42%
6.  Opera --- 1.99%
7.  Android (tablet) --- 1.24%
8.  Yandex Browser --- 0.48%
9.  UC Browser --- 0.41%
10. Coc Coc --- 0.33%

[Mobile now accounts for 54.25% of all web
use](http://gs.statcounter.com/#desktop+mobile-comparison-ww-monthly-201705-201705-bar)
so we also need to examine the top ten phone browsers:

1.  Chrome --- 49.23%
2.  Safari --- 17.73%
3.  UC Browser --- 15.89%
4.  Samsung Internet --- 6.58%
5.  Opera --- 5.03%
6.  Android --- 3.75%
7.  IEMobile --- 0.68%
8.  BlackBerry --- 0.26%
9.  Edge --- 0.15%
10. Nokia --- 0.12%

The worldwide statistics don't tell the whole story:

-   Patterns vary significantly across regions. For example, Yandex is
    the second most-used Russian browser (12.7% share). Sogou is the
    third most-used browser in China (6.5%). Opera Mobile/Mini has a 28%
    share in Africa.
-   New browser releases appear regularly. Chrome, Firefox and Opera
    receive updates every six weeks; it would be impractical to check
    versions going back more than a few months.
-   The same browsers can work differently across devices and operating
    systems. Chrome is available for various editions of Windows, macOS,
    Linux, Android, iOS and ChromeOS, but it's not the same application
    everywhere.
-   There is an exceedingly long tail of old and new, weird and
    wonderful browsers on a range of devices including games consoles,
    ebook readers and smart TVs.
-   Your site's analytics will never match global statistics.

Are Browsers So Different?
--------------------------

Despite the organic variety of applications, all browsers have the same
goal: *to render web pages*. They achieve this with a rendering engine
and there is some cross-pollination:

1.  Webkit is used in Safari on macOS and iOS.
2.  Blink is a fork of Webkit now used in Chrome, Opera, Vivaldi and
    Brave.
3.  Gecko is used in Firefox.
4.  Trident is used in Internet Explorer.
5.  EdgeHTML is an update of Trident used in Edge.

The majority of browsers use one of these engines. They're different
projects with diverse teams but the companies (mostly) collaborate via
the W3C to ensure new technologies are adopted by everyone in the same
way. Browsers are closer than they've ever been, and modern smartphone
applications are a match for their desktop counterparts. However, no two
browsers render in quite the same way. The majority of differences are
subtle, but they become more pronounced as you move toward cutting-edge
technologies. A particular feature may be fully implemented in one
browser, partially implemented in another, and non-existent elsewhere.

Can My Site Work in Every Browser?
----------------------------------

Yes. Techniques such as progressive enhancement (PE) establish a
baseline (perhaps HTML only) then enhance with CSS and JavaScript when
support is available. Recent browsers get a modern layout, animated
effects and interactive widgets. Ancient browsers may get unstyled HTML
only. Everything else gets something in between. PE works well for
content sites and apps with basic form-based functionality. It becomes
less practical as you move toward applications with rich custom
interfaces. Your new collaborative video editing app is unlikely to work
in the decade-old IE7. It may not work on a small screen device over a
3G network. Perhaps it's possible to provide an alternative interface
but the result could be a separate, clunky application few would want to
use. The cost would be prohibitive given the size of the legacy browser
user base.

Site Owner Recommendations
--------------------------

Site owners should appreciate the following fundamentals and constraints
of the web. **The web is not print!** Your site/app will not look
identical everywhere. Each device has a different OS, browser, screen
size, capabilities etc. **Functionality can differ** Your site can work
for everyone but experiences and facilities will vary. Even something as
basic as a date entry field can has a diverse range of possibilities
but, ideally, the core application will remain operable. **Assess your
project** Be realistic. Is this a content site, a simple app, a
desktop-like application, a fast-action game etc. Establish a base level
of browser compatibility. For example, it must work on most two-year-old
browsers with a screen width of 600 pixels over a fast Wi-Fi connection.
**Assess your audience** Don't rely on global browser statistics. Who
are the primary users? Are they IT novices or highly technical? Is it
individuals, small companies or government organisations? Do they sit at
a desk or are they on the move? No application applies to everyone ---
concentrate on the core users first. Examine the analytics of your
existing system where possible but appreciate the underlying data. If
your app doesn't work in Opera Mini, you're unlikely to have Opera Mini
users. Have you blocked a significant proportion of your market?
**Change happens** It's amazing that a web page coded twenty years ago
works today. It won't necessarily be pretty or usable but browsers
remain backward compatible. *(Mostly. The `<blink>` tag can stay dead!)*
However, technology evolves. The more complex your site or application,
the more likely it will require ongoing maintenance.

Web Developer Recommendations
-----------------------------

With a little care it's possible to support a huge variety of browsers.
**Embrace the web!** The web is a device-agnostic platform. Content and
simpler interfaces can work everywhere: a modern laptop, a feature
phone, a games console, IE6, etc. Learn the basics of progressive
enhancement. Even if you choose not to adopt it for your full
application, there will be pockets of functionality where it becomes
invaluable. **Adopt Defensive Development Techniques** Consider the
problem before reaching for the nearest pre-written module, library or
framework. Understand the consequences of that technology before you
start. Frameworks should provide a browser support list because they
have been tested in limited number of applications. Learn about browser
limits and quirks. For example, if you're considering an SVG chart, be
aware that it can look odd in IE9 to 11 and fail in IE8 and below. That
doesn't mean it's a binary choice of rejecting SVGs or abandoning IE
support. There are always compromises which do not incur significant
development. For example:

-   accept SVG rendering is weird but it remains usable
-   only show a table of data in IE, or
-   provide an SVG download which IE users can open elsewhere.

**Test early and test often** You cannot possibly test every device, but
developing for a single browser is futile. Continually test your project
in a variety of applications. Leaving testing to the end will have
catastrophic consequences. It's easy for us to blame tools and browser
inadequacies, but the majority of issues can be rectified during the
development process if they're spotted early. That's not to say
everything must work identically in every browser every time. Feature
regressions are inevitable. For example:

-   Progressive Web Apps do not work offline on iPhones and iPads ---
    but online operation is fine.
-   CSS Grid is not supported in IE --- but float, flexbox or full-width
    block fallbacks should be acceptable.
-   The desktop edition of Firefox does not show a calendar for date
    fields --- but users can still enter one.

Install a selection of browsers on your development PC. Mac and Linux
users can obtain Microsoft Edge and IE testing tools at
[developer.microsoft.com/microsoft-edge/](https://developer.microsoft.com/en-us/microsoft-edge/).
It's more difficult for Windows and Linux users to test Safari; online
test services such as [BrowserStack](https://www.browserstack.com/) are
the easiest option. Modern browsers have excellent mobile emulation
facilities, but use a few real devices to appreciate touch control and
performance on slower hardware and networks. **Use HTTPS on your end**
The web is gradually making HTTPS the preferred protocol, and this trend
is going to continue. Google Chrome is even starting to indicate that
non-HTTPS sites as insecure, which is a good reason for you to configure
your site to use HTTPS. Our web hosting partner,
[SiteGround](https://www.siteground.com/go/article-sp), for example,
made it easy for their clients to make the move to HTTPS. To do that,
they automated the installation of Let’s Encrypt SSL certificates for
all new WordPress accounts, and for existing ones, they made the switch
to HTTPS possible with just a click.

You Haven't Answered the Question!
----------------------------------

The question *"which browsers should you support?"* has become too
restrictive. Presume your answer was just "Chrome":

-   *which devices and OS is it running on?*
-   *what range of screen sizes will be supported?*
-   *which version are you referring to? The latest? Chrome 10 and
    above?*
-   *what happens when a new version of Chrome is released?*
-   *what will happen in other browsers when Chrome effectively becomes
    your application's runtime?*

Providing a browser support list has become impractical for
client-facing projects. Perhaps the best answer is: *"we'll develop your
project according to presumed demographics then test as in many devices,
OSes, browsers, and versions as possible according to budget and time
constraints"*. Even then, you'll miss that aging Blackberry the CEO
insists on using. Develop for the web --- *not browsers*.

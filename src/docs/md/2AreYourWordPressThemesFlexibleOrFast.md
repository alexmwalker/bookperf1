---
template: chapter.pug
title: 'Front End Performance' 
chaptertitle: 'Are Your WordPress Themes Flexible or Fast?'
chapterauthor: Maria Antonietta Perna
---

  

*This article is part of a series created in partnership with
[SiteGround](https://www.sitepoint.com/siteground-recommended-65-off).
Thank you for supporting the partners who make SitePoint possible.*

**If you've developed WordPress themes for mass distribution, you might
have come across the problem of finding the perfect balance between
building performant themes and building feature-rich, media-heavy
products for your customers.**

Let's look closer into what the tension might be all about and how you
can find ways to compromise between creating fast-loading themes and
giving users the flexibility and easy customization options they love
and expect.

Are Flexibility and Performance at Odds When Coding WordPress Themes? 
---------------------------------------------------------------------

I will start by saying that my discussion is not going to be about
performance in relation to an entire WordPress website, which might
include a number of different factors like finding a [great hosting
provider](https://www.siteground.com/wordpress-hosting.htm),
implementing caching mechanisms, leveraging both back-end and front-end
techniques, etc.

Also, the topic is not about the performance of WordPress themes you
code from scratch either for your own use or for a specific client. In
these particular cases you tailor your themes to the specific needs of
yourself or your individual client, which should make performance
optimizations easy to accommodate.

Rather, my focus will be on WordPress themes you code for the general
public, to be distributed on WordPress.org or an online marketplace. The
scenario is one where you, as the theme developer, have no control over
how your theme is going to be used and customized.

But why could coding for performance clash with coding themes for the
public?

On the whole, performance requires you to:

-   stick to simple designs
-   include a limited number of features into your themes (more features
    are likely to require more processing power and resources, all of
    which impacts on theme performance)
-   add the minimum number of page templates for a theme to function
    (fewer templates require fewer resources, which is good for
    performance)
-   perform as few database queries as possible (querying the database
    takes time)
-   limit the number and size of images and other media, which are
    notoriously heavy files
-   minimize the number of HTTP requests (each round trip to the server
    and back takes some time with obvious negative consequences on
    performance).

On the other hand, a considerable number of your theme's users are
likely more entrepreneurial than tech minded. Therefore, what they might
be looking for is a product they can turn into pretty much anything
without so much as one line of code, with lots of functionality out of
the box, breathtaking photo and video assets, super delightful parallax
and other animation effects, and such like.

To give theme users all the flexibility they would like could come with
some performance costs. But let's go into a few specific points that
illustrate how this can happen and how you can find some middle ground
for a successful outcome.

### Themes are for appearance, plugins for functionality 

Although a number of reputable WordPress experts have been throwing some
well-argued [criticisms against the so-called *kitchen sink
themes*](https://wptavern.com/envato-continues-to-rake-in-the-cash-from-wordpress-themes-packaged-as-complete-website-solutions)
--- that is, those multipurpose themes that can become anything to
anybody by offering any functionality under the sun --- the demand for
them is still going strong.

Themes that give users the ability to easily add social media buttons,
SEO features, contact forms, price tables, etc., while attracting a lot
of attention from buyers, don't come without some serious drawbacks.

In particular, this kind of WordPress theme comes tightly coupled with
plugins specifically built for it, or even integrated directly into the
theme. This practice is widely frowned upon for the following reasons:

-   Some of the plugins that come bundled with a theme can have
    vulnerabilities that put theme users at risk. If the theme doesn't
    come with specific plugins installed it's less open to such risks
-   A super important drawback of tight integration between themes and
    plugins is what [Ren Ventura identifies as *theme
    lock*](https://www.engagewp.com/why-i-no-longer-use-themeforest/).
    Here's his explanation of this problem:

    > Theme lock occurs when a WordPress user cannot change his or her
    > theme without gutting most of the site’s functionality. Once the
    > theme is deactivated, it deactivates things like shortcodes and
    > custom post types that were registered by the theme. Without these
    > features that the user has heavily incorporated into the site,
    > things fall apart.

    As a consequence, when you change theme, a lot of the stuff you
    thought it was part of your website content disappears with the old
    theme. For instance, if your theme offered the ability to add
    testimonials to your website, once you change theme all the content
    related to your testimonials will be gone. Not nice.

-   And of course, bloat and hence performace costs. Let's say you don't
    need a testimonials section on your website. Yet, all the code that
    makes that functionality work in the theme is still there: it takes
    up space on your server, which ultimately costs money.

A great maxim the Theme Review Team (i.e., the volunteers who review
themes submitted on the WordPress.org themes repo) rightly enforces is:
**keep functionality separate from appearance. Plugins deal with
functionality, themes with appearance.** Getting rid of all the
complication will improve theme performance and at the same time make it
easier for users to install and configure WordPress themes.

### Your users don't need tons of theme options (but they might not know this) 

Until not long ago, WordPress themes included rather complicated theme
option pages to enable users to make all sorts of modifications with a
button click.

These days, thanks to a momentous [decision taken by the Theme Review
Team on
WordPress.org](https://make.wordpress.org/themes/2015/04/22/details-on-the-new-theme-settings-customizer-guideline/)
in 2015, most themes offer theme options using the [WordPress
Customizer](https://developer.wordpress.org/themes/customize-api/),
which makes possible live previewing the changes as they're being made
by users.

Unfortunately, the kitchen sink mentality that ruled the old theme
option pages has started to migrate to the Customizer, which you can now
see as also starting to get filled up with all sorts of settings.

Although non technical customers love theme options to make changes to
their themes, sometimes too many options available bring more problems
than they seemingly solve:

-   Too many options can paralyze users who are not too familiar with
    core principles of website design.
-   It can take more time than expected to set up and configure a theme.
-   It's easy to make mistakes like choosing the wrong colors, making
    text unreadable, etc.
-   Users might add too many fonts to their themes, thereby spoiling the
    design and slowing down the website.
-   More options means adding more functionality to the theme, thereby
    impacting on its performance.

Although your customers might not accept this at first, you are the
theme designer, therefore you should be the one who is best placed to
make the important design decisions for your theme.

A well-calibrated number of theme options should be sufficient to let
your customers make some targeted and carefully predefined (by you)
modifications to personalize the theme and make it their own.

### Implement smart graphics optimization techniques 

Images are likely to put the heaviest weight on theme performance
compared to template files, scripts and styles. Just run any theme
through [Pingdom's Speed Test tool](https://tools.pingdom.com/) to
verify this.

However, a generous use of full-screen, bold images is hardly
surprising. Images add huge aesthetic value to themes and customers are
drawn to a theme largely on the basis of its visual impact.

Although it's hard to resist the power of great graphics, the guidelines
below can help you code faster themes without necessarily sacrificing
aesthetics (too much):

-   Using as few images as possible is always a good guiding principle.
-   Take advantage of CSS [blend
    modes](https://www.sitepoint.com/close-up-css-mix-blend-mode-property/),
    [filters](https://www.sitepoint.com/css-filter-effects-blur-grayscale-brightness-and-more-in-css/),
    and semi-transparent overlays on low-resolution images to mask
    pixelation and enhance their visual appearance. You can also try to
    experiment with CSS [blend
    modes](https://codepen.io/helmsmith/pen/xbBEWy) and [filters on HTML
    elements and text](https://codepen.io/commander-clifford/pen/PZJYBo)
    rather than images to create stunning visual effects wherever
    possible.
-   Make sure the images are of the [appropriate
    format](https://www.sitepoint.com/what-is-the-right-image-format-for-your-website/)
    and optimized for web.
-   Where appropriate, use [CSS
    sprites](https://www.sitepoint.com/css-sprites-sass-compass/) to
    combine your images into a larger image and therefore reduce HTTP
    requests.
-   Try implementing [lazy loading for
    images](https://www.sitepoint.com/five-techniques-lazy-load-images-website-performance/).
    Doing so will achieve faster page load, and if users don't scroll to
    the location of the `<img>` tag, the image won't need to be
    downloaded at all. To lazy load images in WordPress you can choose
    from a number of good plugins, such as [Lazy
    Load](https://wordpress.org/plugins/lazy-load/), [a3 Lazy
    Load](https://wordpress.org/plugins/a3-lazy-load/) and [Rocket Lazy
    Load](https://wordpress.org/plugins/rocket-lazy-load/).

### Make customer support and education your pillars of strength 

Instead of feeding your customers lots of confusing and time-consuming
theme options, try offering full support and education on how to use
your themes, image optimization, WordPress 101, etc.

You can do so using:

-   blog posts
-   downloadable guides
-   video tutorials
-   FAQs on your website
-   a ticket system for more specific issues that might arise while
    using your themes.

Sharing the basic know-how about your products and the platform you
build for is a great way of making your themes easy to use for your
customers and contributes to generate a high level of trust, which is at
the core of a healthy and successful business.

Sometimes Flexibility Wins over Performance
-------------------------------------------

There may be a limited number of cases when you don't think compromising
some flexibility for the sake of a small performance gain is a good
idea.

A couple of examples come to mind.

### When writing DRY code could make your customers unhappy 

The first example comes from [Tom
McFarlin](https://twitter.com/tommcfarlin), a well-known WordPress theme
developer and author.

In his blog he explains why he decided to include a long list of
[WordPress template
files](https://developer.wordpress.org/themes/template-files-section/partial-and-miscellaneous-template-files/),
thereby going against the
[DRY](https://en.wikipedia.org/wiki/Don't_repeat_yourself) coding
principle (and also impacting negatively on performance to some extent)
while developing his theme *Meyer*.

*Meyer* was targeted at bloggers and digital publishers without much
WordPress technical knowledge. Knowing that lack of coding experience
would not have stopped this audience from tinkering with his theme, Tom
McFarlin thought of a way to make things easier for them. He provided
his customers with a generous number of template files they could easily
copy and paste into a [WordPress child
theme](https://www.sitepoint.com/watch-create-your-first-wordpress-child-theme-quick-fast/)
for customization without hacking the original theme.

McFarlin writes:

> If the templates are defined in the base theme — rather than, say, a
> bunch of developer-ish conditionals — users are going to have an
> easier time basing their templates off of something that already
> exists …
>
> So if I had to sum this up in some concise way, I’d say that I — like
> many developers — want to keep my code as DRY as possible, but there’s
> a tradeoff that comes with doing so in the context of WordPress theme
> development.
>
> And I don’t think that staying DRY is the best way to go about it if
> you’re going to be marketing a product to a wide audience.
>
> [Know Your Customer — the
> Blogger](https://tommcfarlin.com/wordpress-theme-development-thinking-of-the-blogger/).

### Static source code is better for performance but don't use it in your public themes 

My second example of when not to compromise flexibility for some
performance gain in your WordPress themes is based on performance advice
from the Codex.

You'll find some great performance tips there, but there's one I
wouldn't recommend you use, at least if you're going to release your
theme to the general public.

It goes like this:

> -   Can static values be hardcoded into your themes? This will mean
>     you have to edit code every time you make changes, but for
>     generally static areas, this can be a good trade off.
>
>     -   For example, your site charset, site title, and so on.
>
>     -   Can you hardcode menus that rarely change? Avoiding functions
>         like `wp_list_pages()` for example.
>
> [Codex — WordPress Optimization/WordPress
> Performance](https://codex.wordpress.org/WordPress_Optimization/WordPress_Performance)

Avoiding unnecessary calls to the database and optimizing your SQL
queries are excellent practices every theme should follow.

If your theme runs a bit slow, try looking for problematic database
queries that take too long to execute. You can use a plugin like [Query
Monitor](https://wordpress.org/plugins/query-monitor/) in your
development environment to help you with this task.

However, hardcoding bits of information like site title, menu items,
etc., is out of the question. Even if your users never change that info
once it's set, they need to set it at least once and they rightly expect
to do so using WordPress dynamic capabilities.

Furthermore, even if your theme's users were prepared to dig into the
source code, they would have to hack the theme files to add the required
data. This is almost never a good idea, not least because all changes
would be overwritten on the next theme update.

Conclusion
----------

Coding WordPress themes for the general public may involve catering for
diverse and sometimes incompatible needs: those of your customer base,
of web performance, hence the needs of visitors of those websites that
use your themes, and even your own needs as WordPress theme designer and
creator.

In this article I've expressed my views on how to address the dilemma
WordPress theme developers might face between the demands of flexibility
and those of performance.

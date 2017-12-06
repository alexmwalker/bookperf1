---
template: chapter.pug
title: Front-end Performance
chaptertitle: 'Optimizing Web Fonts for Performance: the State of the Art'
chapterauthor: Maria Antonietta Perna
---



**[67% of webpages now use custom
fonts](http://httparchive.org/interesting.php#fonts). However,
popularity and widespread adoption haven't come without some performance
and user experience-related issues.**

In this article, I'll go through what's not so good about the way web
fonts are commonly used and loaded, as well as point you to well-tested
workarounds and future standards-based solutions.

Why Custom Web Fonts?  
---------------------

Users come to your website for content. Therefore, great typography is
crucial on the web: readability, legibility and well-crafted typographic
design are a must for brand recognition and the success of your message.

The best way to achieve beautiful typography is by loading custom web
fonts --- that is, font files that are not already installed on users'
devices.

Since browser support for the CSS `@font-face` rule has become
widespread, using custom web fonts in websites has increased by leaps
and bounds. However, fonts can have a heavy file size, and loading extra
resources on your website doesn't come without some negative impact on
performance.

Since file size can certainly be an issue, paying attention to how
custom web fonts are loaded comes to the forefront.

What Is the Flash of Invisible Text (FOIT) All About? 
-----------------------------------------------------

The most common way of using a custom web font is to specify the fonts
used inside a CSS `@font-face` declaration and leave the browser to its
default behavior. This is not ideal. Since information about fonts is
located in the CSS, by default browsers delay the loading of fonts until
the CSS is parsed. That's not all. As [Zach
Leatherman](https://www.zachleat.com/web/lazy-loading-webfonts/) has
made abundantly clear, to trigger a font download, a number of
conditions must be met besides a valid `@font-face` declaration:

-   an HTML node that uses the same `font-family` specified inside
    `@font-face`
-   in Webkit and Blink browsers, that node must not be empty
-   in [browsers that support the unicode range
    descriptor](http://caniuse.com/#feat=font-unicode-range) inside
    `@font-face`, the content must also match the specified unicode
    range.

If all the points above are satisfied, the browser starts downloading
the font. This means that the browser needs to have parsed not only the
CSS code but also a website's text content before it can trigger a
font's download. However, it's just when the **font starts loading that
web users are likely to experience the dreaded Flash Of Invisible
Text**, or **FOIT** for short.


![FOIT: Page at http://blog.instagram.com/ while fonts are
loading on Firefox v.52. Text is
invisible.](../images/1492627730foit-loading.png) 


![FOIT: Page at
http://blog.instagram.com/ after fonts have been loaded on Firefox v.52.
Text has become
visible](../images/1492627776foit-loaded.png)


In other words, as soon as browsers start downloading a font, all text
becomes invisible. So users look at a screen with no text for some time,
which under sub-par network conditions can feel like forever.
Furthermore, the way different browsers handle FOIT also varies:

-   Blink and Firefox browsers tackle FOIT by introducing a three-second
    timeout: the text disappears for up to three seconds during font
    loading. If the font hasn't loaded within this time frame, the
    website displays a fallback font, which is subsequently replaced
    with the custom font once this is fully loaded. This behavior gives
    rise to what is known as **FOUT** (another acronym): **Flash Of
    Unstyled Text**.
-   Safari, the default Android Browser and Blackberry don't use a
    timeout but display no text until the custom font is loaded. If
    anything goes wrong and the font doesn't get loaded, users are left
    with invisible text on the screen.
-   IE/Edge browsers don't hide text, but display the fallback font
    right away, which seems to be a better design decision by Microsoft.

Although having a sudden change in typeface when reading text on a
website is not the best user experience, staring at a blank screen while
the font is loading sounds much worse. Ideally, a working approach
should overcome both FOUT and FOIT. However, although it's an open
issue, there is some consensus among a number of experts that FOUT is
way better than FOIT.

Tackling the issues related to loading fonts involves working both on
optimizing the file size and taking control of the default browser
behavior so as to eliminate FOIT and minimizing the jarring impact of
FOUT.

Let's look closer into each task one at a time.

Tips on Optimizing Custom Font Files 
------------------------------------

There are a few steps you can take to make sure your font files are not
super heavy.

### \#1 Minimize the number of typefaces in your project 

Don't think that just because you can choose from tons of beautiful
typefaces, you need to weigh down your website with a multitude of
different fonts. The opposite is the case.

In most situations, a couple of judiciously paired typefaces can work
wonders for your design and have a lighter impact on website
performance.

### \#2 Provide web font formats based on browser support 

There are four main font formats:

-   **True Type Font (TTF)**, a common font format that's been around
    since the late 80s
-   **Web Open Font Format (WOFF)**, a format developed in 2009 which is
    Open Type or True Type, only with compression and further metadata
-   **Web Open Font Format (WOFF2)**, a better compressed format than
    WOFF
-   **Embedded Open Type (EOT)**, a format designed by Microsoft to be
    used for embedded fonts on the web.

Although you could specify all of them in your `@font-face` declaration,
you can get away with just two of them. Here's how:

```css 
@font-face { 
    font-family: 'Open Sans'; 
    src: local('Open Sans'), 
    local('OpenSans'), 
    url('fonts/open-sans.woff2')
    format('woff2'), 
    url('fonts/open-sans.woff') format('woff'); 
} 
```  

The first format you suggest to the browser is woff2, which gives you
the advantage of extra compression. If your browser doesn't support
woff2, it just selects woff, which has good compression and enjoys
[support by the latest versions of all major
browsers](http://caniuse.com/#search=woff). Only Opera Mini lacks
support for it, together with IE8 and older Android Mobile browsers. If
you use a progressive enhancement approach to web development, you could
simply let these older browsers fall back on a system font, e.g., Arial,
Verdana, etc.

### \#3 Load just the styles you need  

Fonts usually have different variants --- italic, bold, etc. Each font
variation adds weight to the file size, so try to avoid adding a font
variation to your `@font-face` code if you're not going to use it on
your website. Keep in mind that if you use an `<i>` tag to add an icon,
which is quite common, that is enough to get the browser to load the
italic font variant ([Slide 77 in Zack Leatherman's Santa Clara Velocity
talk of
2015](https://speakerdeck.com/zachleat/the-performance-and-usability-of-font-loading-velocity-santa-clara-2015)).
If that's the only instance of `<i>` in your page, using a `<span>` tag
for the icon or setting the `font-style` for the `<i>` element to
`normal` instead would spare the browser from having to download
unnecessary resources.

### \#4 Keep your character sets down 

Avoid loading all the characters, numbers and symbols your chosen font
makes available. Instead, select just the character sets you actually
use on your webpage.

You can learn everything about fonts subsetting in [Slash Page Load
Times With CSS Font
Subsetting](http://thenewcode.com/878/Slash-Page-Load-Times-With-CSS-Font-Subsetting),
by Dudley Storey.

Tackling FOIT 
-------------

Let's look at some alternatives available today for dealing with FOIT.
I've singled out the simplest approaches to implement among effective
and recommended practices. However, you can learn about far more
sophisticated ones in Zach Leatherman's [A Comprehensive Guide to Font
Loading
Strategies](https://www.zachleat.com/web/comprehensive-webfonts/).

### Don't use custom fonts 

If you can't stand the idea of your users staring at invisible text or
at font-swapping shifts, the most simple option is to give up on custom
fonts and rely solely on web-safe fonts.

These fonts are preinstalled on users' devices, so the browser doesn't
need to load them. Even when deciding to use custom fonts, web
safe-fonts are commonly added to the [font
stack](http://www.cssfontstack.com/) as a fallback.

Giving up on the perfect custom font for your design is not an exciting
approach, but it's perfectly viable --- especially if you consider this
as a transitory stance until cutting edge solutions, which are already
on the horizon, gain wide browser support.

Opting for this approach doesn't require you to use `@font-face`. You
just add your font stack to the CSS `font-family` property. Here's how
you could do this today:


```css
body { 
    font-family: -apple-system, BlinkMacSystemFont, 
    "Segoe UI", Roboto, Oxygen-Sans, Ubuntu, Cantarell,
    "Helvetica Neue", sans-serif; 
    } 
```

[The New System Font
Stack?](https://bitsofco.de/the-new-system-font-stack/)’, by Aderinokun,
provides more information on the modern font stack.

### The Web Font Loader 

One long-standing solution is the [Web Font
Loader](https://github.com/typekit/webfontloader), a JavaScript library
developed by Google and [Typekit](https://typekit.com/), which works
with fonts coming from multiple sources --- such as Google Fonts,
Typekit, your self-hosted font files, etc.

The Web Font Loader loads custom fonts as a background process while
displaying fallback fonts to users, thereby avoiding FOIT. You can set a
time limit of your choice within which the font must load. If the
request exceeds this limit, users will only see the text styled with a
fallback font. Once the custom font is loaded, the script swaps the
fallback font on the page with the custom font.

Here are two awesome tutorials to learn how to use the Web Font Loader:

-   [How to Improve Page Performance with a Font
    Loader](https://www.sitepoint.com/improve-page-performance-font-loader/)
-   [Loading Web Fonts with the Web Font
    Loader](https://css-tricks.com/loading-web-fonts-with-the-web-font-loader/)

### The CSS Font Loading API 

The [CSS Font Loading API](https://drafts.csswg.org/css-font-loading/)
is a standards-based way of loading and taking control of web fonts.

The overall recommended approach remains that of avoiding FOIT and
managing FOUT as best as possible. This API can keep track of every
stage of font loading. You can use this information to style text using
web-safe fonts while fonts are not available and style text using custom
fonts once these are fully loaded.

At the time of writing, the latest versions of [Chrome, Firefox, Safari
and Opera support this
API](http://caniuse.com/#search=css%20font%20loading), while IE/Edge
doesn't offer support yet. However, you can use a small JavaScript
library, the [Font Face
Observer](https://github.com/bramstein/fontfaceobserver), to polyfill
the native CSS Font Loading API, or simply let non-supporting browsers
degrade gracefully to web-safe fonts.

For a short tutorial on how to use the CSS Font Loading API, head over
to [Getting started with CSS Font
Loading](https://medium.com/@matuzo/getting-started-with-css-font-loading-e24e7ffaa791)
by Manuel Matuzovic and have a look at his [CodePen
demo](http://codepen.io/matuzo/pen/jrzyYA?editors=0011).

The Future: The CSS `font-display` Property 
-------------------------------------------

Since their first appearance, you've handled custom fonts using CSS
`@font-face` and `font-family`. It's only fair to expect CSS to solve
any issue related to downloading custom fonts. But at present, only
JavaScript-based solutions are usable.

However, this won't be so indefinitely. A smart CSS property is around
the corner, `font-display`, which is part of the [CSS Font Rendering
Controls Module Level
1](https://tabatkins.github.io/specs/css-font-display/).

`font-display` lets you:

-   Decide if you want to show text using a fallback font or hide it
    while the font is loading.
-   Control what you want to do once the font has loaded, i.e., continue
    to show the fallback font or replace it with the custom font. You
    can also do so on a per-element basis.
-   Specify your own timeout values for each font and even for a
    specific element.

Here's what this property looks like in code:

```css 
@font-face { font-family: Lato; src:
    url('/web/css/fonts/lato/lato-regular-webfont.woff2') format('woff2'),
    url('/web/css/fonts/lato/lato-regular-webfont.woff') format('woff');
    font-weight: 400; 
    font-style: normal; /* This value replaces fallback when font has loaded */ 
    font-display: swap; 
} 
body { 
    font-family: Lato, sans-serif; 
    font-weight: 400; 
    font-style: normal; 
} 
```

Accepted values for this property are:

-   **auto**: browsers just implement their default behavior.
-   **block**: browsers draw invisible text while the font is loading
    and replace it with the desired font as soon as it's loaded.
-   **swap**: browsers use a fallback font while the custom font isn't
    available, but swap the custom font in as soon as it loads.
-   **fallback**: similar behavior to `swap`. However, if too much time
    passes until the custom font loads, browsers use the fallback font
    for the duration of the page's lifetime.
-   **optional**: this super smart value lets browsers use the custom
    font only if it's already available. If it isn't, browsers use the
    fallback for the duration of the page's lifetime. The custom font
    may still be downloaded in the background for use on subsequent page
    loads. However, if browsers detect limited bandwidth on users'
    devices, they might not load the custom font at all.

From these short descriptions, you can already see how `swap` and
`optional` could be super useful.

[Browser support](http://caniuse.com/#search=font-display) for
`font-display` is non-existent at this time, but it can be enabled in
Chrome (via the Experimental Web Platform features" flag) and Firefox
(via the `layout.css.font-display.enabled` flag).

What About FOUT? 
----------------

Although all the approaches outlined above make possible the elimination
of FOIT, either by swapping in the custom font at a later time or by
displaying just the fallback font, they can do little against FOUT.

Although FOUT makes text still available to users, it can very well be
annoying, especially when the fallback font is wider and/or higher than
the custom font it is temporarily replacing, which often causes a shift
in the page content.

You can mitigate the jarring effect of the swap by adjusting the
fallback font's
[x-height](https://www.fonts.com/content/learning/fontology/level-1/type-anatomy/x-height)
and width so as to match the desired custom font's x-height and width as
closely as possible.

You can do this in a number of ways, but I find this little [Font Style
Matcher tool](https://meowni.ca/font-style-matcher/) by Monica
Dinculescu really helpful.

Conclusion
----------

In this article I've outlined the issues of file size and Flash of
Invisible Text related to using custom fonts on websites.

I've presented the latest approaches and pointed you to further
resources where you can read more and learn how to implement those
techniques in your projects.

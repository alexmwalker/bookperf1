---
template: chapter.pug
title: Performance
chaptertitle: Five Techniques to Lazy Load Images for Website Performance
chapterauthor: Maria Antonietta Perna  
---
 

*This article is part of a series created in partnership with
[SiteGround](https://www.siteground.com/sitepoint-recommended?afcode=97a975da3502771c04e59cbae092b1dd&campaign=lazy-load).
Thank you for supporting the partners who make SitePoint possible.*

**With images making up a whopping
[65%](http://httparchive.org/interesting.php?a=All&l=Mar%2015%202017#bytesperpage)
of all web content, page load time on websites can easily become an
issue.**

Even when [properly
optimized](https://www.sitepoint.com/saving-bandwidth-by-using-images-the-smart-way/),
images can weigh quite a bit. This can have a negative impact on the
time visitors have to wait before they can access content on your
website. Chances are, they get impatient and navigate somewhere else,
unless you come up with a solution to image loading that doesn't
interfere with the perception of speed.

In this article, you will learn about five approaches to lazy loading
images that you can add to your web optimization toolkit to improve the
user experience on your website.

What Is Lazy Loading? 
---------------------

Lazy loading images means loading images on websites asynchronously ---
that is, after the above-the-fold content is fully loaded, or even
conditionally, only when they appear in the browser’s viewport. This
means that if users don't scroll all the way down, images placed at the
bottom of the page won't even be loaded.

A number of websites use this approach, but it's especially noticeable
on image-heavy sites. Try browsing your favorite online hunting ground
for high-res photos, and you'll soon realize how the website loads just
a limited number of images. As you scroll down the page, you'll see
placeholder images quickly filling up with real images for preview. For
instance, notice the loader on [Unsplash.com](https://unsplash.com/):
scrolling that portion of the page into view triggers the replacement of
a placeholder with a full-res photo:

![Lazy loading in action on
Unsplash.com](../images/1492178800lazy-loading-unsplash.png)

Why Should You Care About Lazy Loading Images?
----------------------------------------------

There are at least a couple of excellent reasons why you should consider
lazy loading images for your website:

-   If your website uses JavaScript to display content or provide some
    kind of functionality to users, loading the DOM quickly becomes
    critical. It's common for scripts to wait until the DOM has
    completely loaded before they start running. On a site with a
    significant number of images, lazy loading --- or loading images
    asynchronously --- could make the difference between users staying
    or leaving your website.
-   Since most lazy loading solutions work by loading images only if the
    user has scrolled to the location where images would be visible
    inside the viewport, those images will never be loaded if users
    never get to that point. This means considerable savings in
    bandwidth, for which most users, especially those accessing the web
    on mobile devices and slow-connections, will be thanking you.

Well, lazy loading images helps with website performance, but what's the
best way to go about it?

There is no perfect way.

If you live and breathe JavaScript, implementing your own lazy loading
solution shouldn't be an issue. Nothing gives you more control than
coding something yourself.

Alternatively, you can browse the web for viable approaches and start
experimenting with them. I did just that and came across these five
interesting techniques.

#1 David Walsh's Simple Image Lazy Load and Fade 
-------------------------------------------------

David Walsh has proposed his own custom script for lazy loading images.
Here's a simplified version:

The `src` attribute of the `img` tag is replaced with a `data-src`
attribute in the markup:

```html 
    <img data-src="image.jpg" alt="test image"> 
```

In the CSS, `img` elements with a `data-src` attribute are hidden. Once
loaded, images will appear with a nice fade-in effect using CSS
transitions:

```css 
    img { opacity: 1; transition: opacity 0.3s; }
    img[data-src] { opacity: 0; 
    } 
```

JavaScript then adds the `src` attribute to each `img` element and gives
it the value of their respective `data-src` attributes. Once images have
finished loading, the script removes the `data-src` attribute from `img`
elements altogether:

```js
    .forEach.call(document.querySelectorAll('img[data-src]'),
    function(img) { 
        img.setAttribute('src', 
        img.getAttribute('data-src'));
        img.onload = function() { 
        img.removeAttribute('data-src'); 
        }; 
    });
``` 

David Walsh also offers a fallback solution to cover cases where
JavaScript fails, which you can find out more about on [his
blog](https://davidwalsh.name/lazyload-image-fade).

The merit of this solution: it's a breeze to implement and it's
effective.

On the flip side, this method doesn't include loading on scroll
functionality. In other words, all images are loaded by the browser,
whether users have scrolled them into view or not. Therefore, you get
the advantage of a fast loading page because images are loaded after the
HTML content. However, you don't get the saving on bandwidth that comes
with preventing unnecessary image data from being loaded when visitors
don't view the entire page content.

#2 Robin Osborne's Progressively Enhanced Lazy Loading 
-------------------------------------------------------

Robin Osborne suggests a super ingenious solution based on [progressive
enhancement](https://en.wikipedia.org/wiki/Progressive_enhancement). In
this case, lazy loading itself, which is achieved using JavaScript, is
considered the enhancement over regular HTML and CSS.

Why progressive enhancement? Well, if you display images using a
JavaScript-based solution, what happens if JavaScript is disabled or an
error occurs which prevents the script from working as expected? In this
case, without progressive enhancement, users are likely to see no images
at all. Not cool.

You can see the details of a basic version of Osborne's solution in this
[Pen](http://codepen.io/rposbo/pen/ONmgVG), and a more comprehensive
one, which takes into account the case for *broken JavaScript*, in this
other [Pen here](http://codepen.io/rposbo/pen/EKmXvo).

This technique has a number of advantages:

-   The progressive enhancement approach ensures users always have
    access to content.
-   Not only does it cater for a situation where JavaScript is not
    available, but also for those cases when JavaScript is *broken*: we
    all know how error-prone scripts can be, especially in an
    environment where a significant number of scripts are running.
-   It lazy loads images on scroll, so not all images will be loaded if
    users don't scroll to their location in the browser.
-   It doesn't rely on any external dependencies, hence no frameworks or
    plugins are necessary.

You can learn all the details of Robin Osborne's approach on [his
blog](https://www.robinosborne.co.uk/2016/05/16/lazy-loading-images-dont-rely-on-javascript/).

#3 Lazy Load XT jQuery Plugin
------------------------------

A quick and easy alternative for implementing lazy loading of images is
to let a JavaScript/jQuery plugin do the heavy lifting for you.

Lazy Load XT is a feature-packed jQuery plugin. You can opt for a
simplified version called `jquery.lazyloadxt.js`, which lets you just
lazy load images. Alternatively, you can use
`jquery.lazyloadxt.extra.js`, which is an extended version of the
plugin. With the extended version, you can lazy load iframes, videos,
and generally all tags that use a `src` attribute.

To include Lazy Load XT in your project, at the bottom of your HTML page
before the closing `</body>` tag, add the [jQuery
library](http://jquery.com/), followed by one of the two Lazy Load XT
flavors mentioned above. For instance:

```html 
    <script src="jquery.js"></script>
    <script src="jquery.lazyloadxt.js"></script> 
```

If you don't want to use jQuery, Lazy Load XT offers a much lighter
option, a [small script called
`jqlight.lazyloadxt.min.js`](https://raw.githubusercontent.com/ressio/lazy-load-xt/master/dist/jqlight.lazyloadxt.min.js):

```html 
    <script src="jqlight.lazyloadxt.js"></script> 
    <script src="jquery.lazyloadxt.js"></script> 
```

In your HTML document, mark up images using a `data-src` attribute
instead of the regular `src` attribute, like this:

```html 
    <img data-src="lazy.jpg" width="100" height="100" alt="test image"> 
```


You can then leave the plugin to initialize itself, or you can manually
initialize it yourself. For instance, to initialize a selection of
elements write:

```js 
    $(elements).lazyLoadXT(); 

```

This plugin makes tons of add-ons for extra functionality available. To
mention just a couple:

-   By adding `jquery.lazyloadxt.spinner.css`, you can display an
    animated spinner as the image is loading.
-   You can add all the
    [Animate.css](https://github.com/daneden/animate.css) fun effects
    for image loading if you just include `animate.min.css` in your
    project and write this line of code in your JavaScript file:
    `$.lazyLoadXT.onload.addClass = 'animated bounceOutLeft';`. Of
    course, you can replace `bounceOutLeft` with any of the effects
    Animate.css provides.

Among the advantages of Lazy Load XT and its add-ons are:

-   CDN hosting support so you don't need to download Lazy Load XT
    scripts to your server.
-   Wide browser support, including IE6-11 and Opera Mini.
-   You can lazy load images on the page, in scrollable containers, in
    both vertical and horizontal scroll layouts, and in infinite
    scrolling scenarios.
-   Using add-ons you can create great transition effects, implement
    support for retina screens, lazy load background images, and much
    more.
-   You can lazy load different media types.
-   The documentation shows how you can counteract the eventuality of
    browsers with JavaScript disabled.
-   You don't need to include the full jQuery library to use this plugin
    for lazy loading images.

For a full list of options and add-ons, visit the [Lazy Load XT repo on
GitHub](https://github.com/ressio/lazy-load-xt).

#4 bLazy.js — Vanilla JavaScript Plugin 
----------------------------------------

![bLazy jQuery plugin for lazy loading
images.](../images/1492179640blazy-plugin.png)

bLazy is a smart vanilla JavaScript plugin for lazy loading images. More
specifically:

> bLazy is a lightweight lazy loading image script (less than 1.2KB
> minified and gzipped). It lets you lazy load and multi-serve your
> images so you can save bandwidth and server requests. The user will
> have faster load times and save data loaded if he/she doesn't browse
> the whole page.
>
> [bLazy website](http://dinbror.dk/blazy/)

This small, dependency-free plugin lets you:

-   lazy load both embedded images and background images
-   serve different images according to screen size and high resolution
    screens
-   lazy load everything with a `src` attribute, e.g., iframes, HTML5
    videos, scripts, unity games etc.
-   lazy load images inside a scrolling container
-   support legacy browsers, back to IE7 and IE8
-   use a CDN, so you don't need bLazy hosted on your server.

Here's a basic implementation.

The markup:

```html 
<img class="b-lazy" src="placeholder.gif" data-src="image.jpg" alt="test image"> 
```

You need to modify the regular `img` tag as follows:

-   Add a class of `.b-lazy`.
-   Use a placeholder image as value for the `src` attribute. To save
    HTTP requests, you can also use an inline [base64-encoded
    transparent
    gif](http://probablyprogramming.com/2009/03/15/the-tiniest-gif-ever).
    But beware, doing so won't have the benefits of caching on
    subsequent pages where you use the same image.
-   The `data-src` attribute points to the image you want to lazy load.

The JavaScript: enter a simple call to bLazy and fine-tune with a map of
options:

```js 
    var bLazy = new Blazy({ 
        //options here 
    });
```

To learn more on bLazy and its available options, follow these links:

-   [bLazy.js – A lazyload image
    script](http://dinbror.dk/blog/blazy/?ref=demo-page)
-   [bLazy.js Website](http://dinbror.dk/blazy/)
-   [bLazy.js Examples](http://dinbror.dk/blazy/examples/?ref=blog)
-   [bLazy.js Project on GitHub](https://github.com/dinbror/blazy/).

#5 Lazy Loading with Blurred Image Effect
------------------------------------------ 

If you are a [Medium](https://medium.com/) reader, you have certainly
noticed how the site loads the main image inside a post.

The first thing you see is a blurred, low-resolution copy of the image,
while its high-res version is being lazy loaded:

![Blurred placeholder image on Medium website.](../images/1492179905medium-blurred.png)
 
 Blurred placeholder image on Medium website.

![High-res, lazy loaded image on Medium website.](../images/1492179959medium-regular.png)

High-res, lazy loaded image on Medium website.

You can lazy load images with this interesting blurring effect in a
number of ways.

My favorite technique is by Craig Buckler. Here's all the goodness of
this solution:

-   Performance: only 463 bytes of CSS and 1,007 bytes of minified
    JavaScript code
-   Support for retina screens
-   Dependency-free: no jQuery or other libraries and frameworks
    required
-   Progressively enhanced to counteract older browsers and failing
    JavaScript

You can read all about it in [How to Build Your Own Progressive Image
Loader](https://www.sitepoint.com/how-to-build-your-own-progressive-image-loader/)
and download the code on the project's [GitHub
repo](https://github.com/craigbuckler/progressive-image.js).

Conclusion
----------

And there you have it --- five ways of lazy loading images you can start
to experiment with and test out in your projects.

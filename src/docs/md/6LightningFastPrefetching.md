---
template: chapter.pug
title: Performance
chaptertitle: Lightning Fast Websites with Prefetching
chapterauthor: Maria Antonietta Perna
---
 

*This article is part of a series created in partnership with
[SiteGround](https://www.siteground.com/go/article-sp). Thank you for
supporting the partners who make SitePoint possible.*

**In this article, I'll discuss prefetching, what it is, and how
developers can use it to wow visitors with high-performance websites.**

What Is Prefetching? 
--------------------

Although optimizing a website for initial page load is great, for the
highly interactive websites users expect today, this might not be
enough.

What if browsers knew which link users were about to click, or which
page they were about to visit next, so that content could automagically
appear on their screen at the speed of light?

Well, browsers *can* do that now. In fact, some major browsers are smart
enough to make these kinds of guesses based on visitors' browsing
patterns, document markup and structure, the user's device,
connectivity, etc. Therefore, browsers already try to anticipate the
resources needed to build the page visitors are likely to access, get
those resources ready and display the page at warp speed when users
request it. However, developers can use their knowledge of the website
or web application to help browsers single out those crucial resources
more accurately.

This is what prefetching is, a hint for browsers to foresee users' every
browsing wish and make it come true in a snap.

You can find prefetching in the **[Resource
Hints](https://w3c.github.io/resource-hints/)** specification led by
Ilya Grigorik.

In what follows, I'm going to discuss:

-   DNS-prefetching
-   Link/Resource prefetching
-   Prerendering (Page prefetching)

DNS-Prefetching
---------------

The internet is a network of computer-readable [IP
addresses](http://whatismyipaddress.com/ip-address) (e.g., 87.87.87.87)
that are referenced by human-readable hostnames (e.g., yoursite.com).
DNS is the protocol that converts hostnames into IP addresses.

Each time the browser makes an HTTP request for a resource on a
different domain, it can spend a few milliseconds to resolve the domain
to the associated IP address.

If a website has a Twitter or Facebook widget, Google Analytics, and
perhaps one or two custom web fonts, then it must have links to other
domains. This makes DNS lookups unavoidable.

DNS prefetching helps to decrease waiting time for visitors because the
browser performs DNS lookups before it sends a request for resources
located on other domains.

So, let's say developers know a website will make a request to
somewidget.example.com. They can drop a hint for the browser suggesting
it to go ahead and prefetch that hostname's DNS by adding a `rel`
attribute to the link with the value of `dns-prefetch`, like this:

```html
    <link rel="dns-prefetch" href="//somewidget.example.com"> 
```

Now, when the request to somewidget.example.com takes place, the browser
has already performed the DNS lookup ahead of time and users get the
results delivered a bit sooner.

[Browser support for
DNS-prefetch](http://caniuse.com/#search=dns-prefetch) at this time is
present in all major desktop browsers. When support for DNS-prefetching
is lacking, browsers simply retrieve resources in the usual way. No big
deal.

What if the browser never requests the DNS-prefetched resource?
Thankfully, DNS-prefetching is not a costly operation, since it doesn't
send more than just a few hundred bytes over the network. Once again, no
harm done.

Link Prefetching
----------------

According to the [spec](https://w3c.github.io/resource-hints/#prefetch),
link prefetching …

> is used to identify a resource that might be required by the next
> navigation, and that the user agent SHOULD fetch, such that the user
> agent can deliver a faster response once the resource is requested in
> the future.

In other words, if developers have grounds to assume users are likely to
visit a specific web page and know the browser needs some critical
resources to deliver it, they can use the `prefetch` directive. This
points the browser towards the resources it should fetch before users
actually navigate to that page.

Keep in mind that prefetching only works with cacheable resources, such
as JavaScript, images and so on. Here's what the code looks like:

```html
<link rel="prefetch" href="//example.com/future-image.jpg"> 
```

The browser now knows that the `future-image.jpg` image is going to be
needed soon, so it can prefetch it and store it locally in the cache. At
that point, because the browser will have already done the
time-consuming work of downloading the resource in the background, the
benefit of this technique consists in super-fast rendering as soon as
users click to access the relevant page requiring `future-image.jpg`.

Link prefetching is great at creating the perception of a fast-loading
website, but it's a much more expensive operation than DNS-prefetching.
If users never access the page which needs the prefetched asset, the
browser will have downloaded unnecessary data and clogged up its cache.

At the time of writing, link prefetching is [supported by the latest
versions of Chrome, Firefox, IE/Edge and
Opera](http://caniuse.com/#search=prefetch). Non-supporting browsers
will simply ignore the hint.

Page Prefetching/Prerendering 
-----------------------------

Implementing page prefetching or prerendering is just a matter of adding
the `prerender` directive inside a link's `rel` attribute, like this:

```html
<link rel="prerender" href="//example.com/future-page.html"> 
```

Prerendering goes all the way and literally creates an invisible version
of the entire page users are likely to go to next, including applying
CSS and executing JavaScript. As soon as users click on the relevant
link, the ghost page instantly materializes before their eyes, replacing
the old content.

If link prefetching can be an expensive operation, prerendering is even
more so. In this case, browsers download an entire web page and all its
resources on the basis of an expectation that users will visit that
page, which ultimately may or may not happen.

At this time, prerendering is [supported in IE/Edge, Chrome and
Opera](http://caniuse.com/#search=prerender).

Use Cases for Link Prefetching and Prerendering 
-----------------------------------------------

Since both link prefetching and (to a greater extent) prerendering don't
come cheap, having a definite idea of when it's suitable to implement
them on a web page is important.

Devs who plan on using resource hints should do so when they have solid
grounds for assuming which resources to prefetch or prerender based on
the website's content, users' behavior, analytics, etc.

The Resource Hints spec indicates a number of [use
cases](https://w3c.github.io/resource-hints/#use-cases) where link
prefetching could benefit user experience:

-   Search results — It's reasonable to assume users are interested in
    those results, therefore it's likely they're going to click the
    links leading to the content they're after.
-   Paginated content — Once users are reading the first page of a
    multi-page article, it's not too far-fetched to assume they're going
    to click the link that lands them on the next page.
-   Image galleries — Google devs use prefetching on Picasa Web Albums.
    If users are viewing a photo, developers are justified in guessing
    that the next photo is also going to be accessed, so they instruct
    the browser to download it as soon as possible.

Similarly, Santiago Valdarrama offers some [helpful
tips](https://alistapart.com/article/one-step-ahead-improving-performance-with-prebrowsing)
on when developers could reasonably consider implementing prerendering
on a website:

> When deciding whether to prerender entire pages ahead of time,
> consider that Google prerenders the top results on its search page,
> and Chrome prerenders pages based on the historical navigation
> patterns of users. Using the same principle, you can detect common
> usage patterns and prerender target pages accordingly. You can also
> use it, just like resource prefetching, on questionnaires or surveys
> where you know users will complete the workflow in a particular order.

Resources
---------

If you'd like to dig deeper, the following resources are a must-read:

-   [Resource Hints
    Specification](https://w3c.github.io/resource-hints/)
-   [Web Performance Tricks — Beyond the
    Basics](https://www.sitepoint.com/web-performance-tricks-beyond-basics/)
-   [Prefetching
    resources](https://developers.google.com/speed/articles/prefetching)
-   [Prefetching, preloading,
    prebrowsing](https://css-tricks.com/prefetching-preloading-prebrowsing/)
-   [One Step Ahead: Improving Performance with
    Prebrowsing](https://alistapart.com/article/one-step-ahead-improving-performance-with-prebrowsing)
-   [Front-end performance for web designers and front-end
    developers](https://csswizardry.com/2013/01/front-end-performance-for-web-designers-and-front-end-developers/)

Conclusion
----------

DNS-prefetching, link prefetching and prerendering are powerful
optimization techniques. If **implemented responsibly**, on the basis of
the knowledge developers have of web content and user behavior,
prefetching can considerably improve user experience.

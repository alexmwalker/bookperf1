---
template: chapter.pug
title: Performance
chaptertitle: 'Optimizing CSS: ID Selectors and Other Myths'
chapterauthor: Ivan Čurić 
---

 
**In today’s typical scenario, where the average website ships 500KB of
gzipped JavaScript and 1.5MB of images, running on a midrange Android
device via 3G with a 400ms round trip time, CSS selector performance is
the least of our problems.** 

Still, there’s something to be said about the topic, especially to weed
out some of the myths and legends surrounding them. So let’s dive right
in.

The Basics of CSS Parsing 
-------------------------

First, to get on the same page --- this article isn’t about the
performance of CSS properties and values. What we’re covering here is
the performance cost of the selectors themselves. I'll be focusing on
the Blink rendering engine, specifically Chrome 62.

The selectors can be split into a few groups and (roughly) sorted from
the least to most expensive:

<table>
<thead>
<tr>
<th>rank</th>
<th>type</th>
<th>example</th> 
</tr>
</thead>
<tbody>
<tr>
<td>1.</td>
<td>ID</td>
<td><code>#classID</code></td>
</tr>
<tr>
<td>2.</td>
<td>Class</td>
<td><code>.class</code></td>
</tr>
<tr>
<td>3.</td>
<td>Tag</td>
<td><code>div</code></td>
</tr>
<tr>
<td>4.</td>
<td>General and adjacent sibling</td>
<td><code>div ~ a</code>, <code>div + a</code></td>
</tr>
<tr>
<td>5.</td>
<td>Child and descendant</td>
<td><code>div &gt; a</code>, <code>div a</code></td>
</tr>
<tr>
<td>6.</td>
<td>Universal</td>
<td><code>*</code></td>
</tr>
<tr>
<td>7.</td>
<td>Attribute</td>
<td><code>[type="text"]</code></td>
</tr>
<tr>
<td>8.</td>
<td>Pseudo-classes and elements</td>
<td><code>a:first-of-type</code>, <code>a:hover</code></td>
</tr>
</tbody>
</table>



Does this mean that you should only use IDs and classes? Well … not
really. It depends. First, let’s cover how browsers interpret CSS
selectors.

Browsers read CSS from right to left. The rightmost selector in a
compound selector is known as the **key** selector. So, for instance, in
`#id .class > ul a`, the key selector is `a`. The browser first matches
all key selectors. In this case, it finds all elements on the page that
match the `a` selector. It then finds all `ul` elements on the page and
filters the `a`s down to just those elements that are descendants of
`ul`s --- and so on until it reaches the leftmost selector.

Therefore, the shorter the selector, the better. If possible, make sure
that the key selector is a class or an ID to keep it fast and specific.

Measuring the Performance
-------------------------

Ben Frain created [a series of
tests](https://benfrain.com/css-performance-revisited-selectors-bloat-expensive-styles/)
to measure selector performance back in 2014. The test consisted of an
enormous DOM comprising 1000 identical elements, and measuring the speed
it took to parse various selectors, ranging from IDs to some seriously
complicated and long compound selectors. What he found was that the
delta between the slowest and fastest selector was \~15ms.

However, that was back in 2014. Things have changed a lot since then,
and memorizing rules is all but useless in the ever-changing browser
landscape. Always remember to do your own tests, especially when
performance is concerned.

I went to do my own tests, and for that I used Paul Lewis' test
mentioned in [Paul Irish's
comment](https://alistapart.com/comments/quantity-queries-for-css#338752)
expressing concern over the useful, yet convoluted "[quantity
selectors](https://alistapart.com/article/quantity-queries-for-css)”:

> These selectors are among the slowest possible. \~500 slower than
> something wild like “div.box:not(:empty):last-of-type .title”. Test
> page http://jsbin.com/gozula/1/quiet

The test was bumped up a bit, to 50000 elements, and you can [test it
out yourself](https://codepen.io/ivancuric/pen/ZaWxqV). I did an average
of 10 runs on my 2014 MacBook Pro, and what I got was the following:

<table>
<thead>
<tr>
<th>Selector</th>
<th>Query Time (ms)</th>
</tr>
</thead>
<tbody>
<tr>
<td><code>div</code></td>
<td>4.8740</td>
</tr>
<tr>
<td><code>.box</code></td>
<td>3.625</td>
</tr>
<tr>
<td><code>.box &gt; .title</code></td>
<td>4.4587</td>
</tr>
<tr>
<td><code>.box .title</code></td>
<td>4.5161</td>
</tr>
<tr>
<td><code>.box ~ .box</code></td>
<td>4.7082</td>
</tr>
<tr>
<td><code>.box + .box</code></td>
<td>4.6611</td>
</tr>
<tr>
<td><code>.box:last-of-type</code></td>
<td>3.944</td>
</tr>
<tr>
<td><code>.box:nth-of-type(2n - 1)</code></td>
<td>16.8491</td>
</tr>
<tr>
<td><code>.box:not(:last-of-type)</code></td>
<td>5.8947</td>
</tr>
<tr>
<td><code>.box:not(:empty):last-of-type .title</code></td>
<td>8.0202</td>
</tr>
<tr>
<td><code>.box:nth-last-child(n+6) ~ div</code></td>
<td>20.8710</td>
</tr>
</tbody>
</table>

The results will of course vary depending on whether you use
`querySelector` or `querySelectorAll`, and the number of matching nodes
on the page, but `querySelectorAll` comes closer to the real use case of
CSS, which is targeting all matching elements.

Even in such an extreme case, with 50000 elements to match, and using
some really insane selectors like the last one, we find that the slowest
one is \~20ms, while the fastest is the simple class at \~3.5ms. Not
really that much of a difference. In a realistic, more "tame" DOM, with
around 1000–5000 nodes, you can expect those results to drop by a factor
of 10, bringing them to sub-millisecond parsing speeds.

What we can see from this test is that it's not really worth it to worry
over CSS selector performance. Just don't overdo it with pseudo
selectors and really long selectors. We can also see how Blink improved
in the last two years. Instead of the stated \~500x slowdown for a
"quantity selector" (`.box:nth-last-child(n+6) ~ div`) compared to an
"insanity selector" (`.box:not(:empty):last-of-type .title`), we only
see a \~1.5x slowdown. That's an amazing improvement, and we can only
expect browsers to get better, making CSS selector performance even less
impactful.

You *should*, however, stick to using classes whenever possible, and
adopt some sort of namespacing convention like BEM, SMACSS or OOCSS,
since it will not only help your website's performance but vastly help
with code maintainability. Overqualified compound selectors, especially
when used with tag and universal selectors --- such as
`.header nav ul > li a > .inner` --- are extremely brittle and a source
of many unforeseen errors. They are also a nightmare to maintain,
especially if you inherit the code from someone else.

Quality over Quantity 
---------------------

A bigger problem of simply having expensive selectors is having *a lot*
of them. This is know as "style bloat", and you've probably seen the
problem a lot. Typical examples are sites which import entire CSS
frameworks like Bootstrap or Foundation, while using less than 10% of
the transferred CSS. Another example is seen in old, never-refactored
projects whose CSS has devolved into, as I like to call them,
"Chronological Style Sheets" --- CSS with a ton of appended classes to
the end of the file as the project has changed and grown, now looking
more like an overgrown garden full of weeds.

Not only does a large CSS file take longer to transfer, (and network is
the *biggest* bottleneck in website performance), they also take longer
to parse. As well as constructing the DOM from your HTML, the browser
needs to construct a CSSOM (CSS Object Model) to compare it with the DOM
and match the selectors.

So, keep your styles lean and DRY, don't include everything and the
kitchen sink, load what you need and when you need it, and use
[UNCSS](https://github.com/giakki/uncss) if you need to.

If you want to dig more into how the browsers parse CSS, [check out
Nicole Sullivan's post on
Webkit](https://calendar.perfplanet.com/2011/css-selector-performance-has-changed-for-the-better/),
[Ilya Grigorik's article on how Blink does
it](https://developers.google.com/web/fundamentals/performance/critical-rendering-path/constructing-the-object-model),
or [Lin Clark's article on Mozilla's new Stylo CSS
engine](https://hacks.mozilla.org/2017/08/inside-a-super-fast-css-engine-quantum-css-aka-stylo/).

The Elephant in the Room: Style Invalidation 
--------------------------------------------

What we've covered so far is fine, but we've only discussed a single
rendering pass. Today's websites are no longer static documents, but
resemble apps with dynamic content users can interact with.

This complicates things, since parsing CSS is only a single step in the
browser rendering pipeline. Here's a render-oriented view of how a
browser renders a single frame to the screen (source:
[Google](https://developers.google.com/web/fundamentals/performance/rendering/images/intro/frame-full.jpg)):

![The browser rendering
pipeline](../images/1510297945frame-full-1024x156.jpg)

We won't be going into JavaScript performance and compositing, but will
focus instead on the purple part --- style parsing and laying out the
elements.

After constructing the DOM and CSSOM, the browser needs to combine the
two into a render tree before finally painting it on the screen. In that
step, the browser needs to figure out the computed CSS for each matching
element. You can see this yourself in the **Elements &gt; Styles** panel
of the developer tools. It takes all the matching styles, the cascade,
and browser-specific user agent styles to construct the final, computed
CSS for the element.

It can then proceed to the layout (also known as reflow) step, where it
computes the geometry, and constructs the box model of the page, placing
each element on its respective position on the viewport. Layout is the
most computationally intensive part of this process.

Finally, the browser converts each node in the render tree to actual
pixels on the screen in the paint stage.

Now, what happens when we mutate the DOM by *changing* some classes on
the page, adding or removing some nodes, modifying some attributes, or
in any way messing with the HTML ([or the stylesheets
themselves](https://developer.mozilla.org/en-US/docs/Web/API/CSSStyleSheet))?

We invalidate the computed styles and the browser needs to invalidate
*everything* down the tree of the matched selectors. While today's
browsers are much smarter, it used to be the case that if you changed a
class on the `body` element, all the descendant elements needed to have
their computed styles recalculated.

One way to avoid this issue is to reduce the complexity of your
selectors. Instead of writing `#nav > .list > li > a`, use a single
selector, like `.nav-link`. That way, you reduce the scope of style
invalidation, since if you modify anything inside the `#nav`, you won't
trigger recalculations for the entire node.

Another way is to reduce the scope --- such as the number of invalidated
elements. Be specific with your CSS. Keep this in mind especially during
animations, where the browser has only \~10ms to do all the work
required.

If you want to get down to the nitty gritty details of style
invalidation, I recommend reading [Style Invalidation in
Blink](https://docs.google.com/document/d/1vEW86DaeVs4uQzNFI5R-_xS9TcS1Cs_EUsHRSgCHGu8/edit).

Conclusion
----------

To sum it up, you shouldn't worry about selector performance, unless you
*really* go overboard. While the topic was all the rage in 2012,
browsers have gotten *a lot* faster and smarter since. Even Google
doesn't worry about it anymore. If you check out Google's [Page Speed
Insights page](https://developers.google.com/speed/docs/insights/rules),
you won't see the rule "Use efficient CSS selectors" [which was removed
in
2013.](https://web.archive.org/web/20130330123924/https://developers.google.com/speed/docs/insights/rules)

Instead, focus on making your CSS maintainable and readable. Your
colleagues and your future self will thank you for it. Try to optimize
the CSS delivery by including only the used styles. And after that, get
to know the rendering pipeline. Unlike selectors, styles themselves
*can* be expensive, and [the difference between a janky site and a
smooth
one](https://www.sitepoint.com/check-css-animation-performance-with-the-browsers-dev-tools/)
can often be found in how the CSS is implemented.

And as a final note: always do your own tests.

Don't just believe what someone wrote on the internet a few years ago.
The landscape is changing drastically and at an incredible pace. What's
relevant today might become obsolete sooner than you know.

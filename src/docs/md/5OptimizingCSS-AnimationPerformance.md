---
template: chapter.pug
title: Performance
chaptertitle: 'Optimizing CSS: Tweaking Animation Performance with DevTools'
chapterauthor: Maria Antonietta Perna
---
 
 
*This article is part of a series created in partnership with
[SiteGround](https://www.sitepoint.com/siteground-recommended-65-off).
Thank you for supporting the partners who make SitePoint possible.*

**CSS animations are known to be super performant. Although this is the
case for simple animations on a few elements, if you didn't code your
animations with performance in mind and add more complexity, website
users will soon take notice and possibly get annoyed.**

In this article, I introduce some useful browser DevTools features that
will enable you to check what happens under the hood when animating with
CSS. This way, when an animation looks a bit choppy, you'll have a
better idea why and what you can do to fix it.

Developer Tools for CSS Performance 
-----------------------------------

Your animations need to hit 60 fps (frames per second) to run fluidly in
the browser. The lower the rate, the worse your animation will look.
This means the browser has no more than about 16 milliseconds to do its
job for one frame. But what does it do during that time? And how would
you know if your browser is keeping up with the desired framerate?

I think nothing beats user experience when it comes to assessing the
quality of an animation. However, developer tools in modern browsers,
while not always 100% reliable, have been getting smarter and smarter,
and there's quite a bit you can do to review, edit and debug your code
using them.

This is also true when you need to check framerate and CSS animation
performance. Here's how it works.

Exploring the Performance Tool in Firefox 
-----------------------------------------

In this article I use the Firefox Performance tool. The other big
contender is the Chrome Performance Tool. You can pick your favorite, as
both browsers offer powerful performance features.

To open the developer tools in Firefox, choose one of these options:

-   Right-click on your web page and choose *Inspect Element* in the
    context menu
-   If you use the keyboard, press Ctrl + Shift + I on Windows and Linux
    or Cmd + Opt + I on macOS.

Next, click on the *Performance* tab. Here, you'll find the button that
lets you start a recording of your website's performance:

![Performance panel in Firefox Developer
Tools.](../images/1494616829devtools-performance-panel.png)

Press that button and wait for a few seconds or perform some action on
the page. When you're done, click the *Stop Recording Performance*
button:

![Stop recording browser activity in the Performance
panel.](../images/1494616877stop-recording.png)

In a split second, Firefox presents you with tons of well-organized data
that will help you make sense of which issues your code is suffering
from.

The result of a recording inside the *Performance* panel looks something
like this:

![Results of the recording in the Performance
panel.](../images/1494616916complete-recording-screenshot.png)

The *Waterfall* section is perfect for checking issues related to CSS
transitions and keyframe animations. Other sections are the *Call Tree*
and the *JS Flame Chart*, which you can use to find out about
bottlenecks in your JavaScript code.

The Waterfall has a summary section at the top and a detailed breakdown.
In both, the data is color-coded:

-   Yellow bars refer to JavaScript operations.
-   Purple bars refer to calculating HTML elements’ CSS styles
    (recalculate styles) and laying out your page (layout). Layout
    operations are quite expensive for the browser to perform, so if you
    animate properties that involve repeated layouts (also known as
    *reflows* --- such as `margin`, `padding`, `top`, `left`, etc. ---
    the results could be janky.
-   Green bars refer to painting your elements into one or more bitmaps
    (Paint). Animating properties like `color`, `background-color`,
    `box-shadow`, etc., involves costly paint operations, which could be
    the cause of sluggish animations and poor user experience.

You can also filter the type of data you want to inspect. For instance,
I'm interested only in CSS-related data, so I can deselect everything
else by clicking on the filter icon at the top left of the screen:

![Filter feature inside Firefox Performance Dev
Tools.](../images/1494616970performance-panel-filter.png)

The big green bar below the Waterfall summary represents information on
the framerate.

A healthy representation would look quite high, but most importantly,
consistent --- that is, without too many deep gaps.

Let's illustrate this with an example.

### The Performance tool in action 

This is a simple [CSS
animation](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Animations/Using_CSS_animations)
using the `@keyframes` keyword. The test page looks like this:

![Demo page with rectangular box
visible.](../images/1494617022demo-page-visible-box.png)

![Demo page with
rectangular box about to slide
out.](../images/1494617057demo-page-disappearing-box.png)

The rectangular purple box slides in and out of view in an infinite
cycle.

I've done this by animating the `margin-left` property of the `<div>`
element that represents the rectangular box on the screen. Here's what
the `@keyframes` animation block looks like:

```css 
@keyframes slide-margin { 
100% { 
    margin-left: 0;
    } 
} 
```

The performance data I get from this animation looks like this:

![Recording of animating with margins in Firefox Performance panel of
the developer
tools.](../images/1494617156slide-margin-recording.png)

The framerate visual looks a bit jagged and the framerate is an average
of 44.82 fps, which is a bit low.

Also, notice all the layout and paint operations that take place during
the animation. These are costly operations the browser performs on its
main thread, which has a negative impact on performance.

Finally, if you access the *Inspector* tool, click in the *Animation*
section and hover over the animation name, an information box pops up
with all the relevant data about the current animation. If your
animation were optimized, there would be a message stating the fact. In
this case, there is no message:

![Animation panel inside the Inspector tool with info about the current
animation.](../images/1494617204animation-panel-nonoptimized.png)

Now, I'm going to change my code and make a new recording as the browser
animates the CSS `translate3d()` property using this `@keyframes` block:

```css 
@keyframes slide-three-d { 
100% { transform: translate3d(0, 0, 0); 
    } 
} 
```

This is what the recording of the performance looks like:

![Recording of animation done using
translate3d.](../images/1494617268translate-recording.png)

Now the framerate is higher (56.83 fps) and the waterfall is showing no
costly layout and paint operations.

Also, if you open the *Inspector* tab of the developer tools, access the
*Animation* panel and hover the mouse over the name of the animation,
you can see something like this:

![Animation panel in the Inspector of Firefox Developer
Tools.](../images/1494617317animation-panel.png)

The info box relative to the animation name states that all animations
are optimized, which is good news for your website visitors.

Only Animate CSS *Opacity*, *Transforms* and *Filters* 
------------------------------------------------------

You've probably heard this piece of advice before, but just in case,
it's worth going over it again: if you want your animations to run
smoothly, animate only CSS opacity, transforms and filters. Animating
everything else will put your browser under pressure to perform costly
tasks in very little time, which often doesn't yield the best results.

As the Performance tool in your browser confirms, repeated layout and
paint operations are not your friend.

However, each browser handles CSS properties a bit differently. If you
want to know exactly which browser triggers layout and paint operations
for which properties (especially when updating values for those
properties, which is what is involved in web animation), head over to
[CSS Triggers](https://csstriggers.com/).

To ensure performant animations, a popular approach is to force the
browser to hand over the work of changing some properties to the GPU
(Graphical Processing Unit), which relieves the browser's main thread of
some pressure and takes advantage of hardware acceleration. You can do
so by using the [`will-change` CSS
property](https://developer.mozilla.org/en/docs/Web/CSS/will-change), or
the `translateZ(0) ` and `translate3d(0,0,0)` hacks. All of these tricks
will work, but if you overdo it you could actually get exactly what
you're trying to avoid, i.e., janky animations.

I'm not going into the details of hardware acceleration for web
animation performance, but if you'd like to dig deeper, take a look at
the resources listed below.

Resources
---------

-   [High Performance
    Animations](https://www.html5rocks.com/en/tutorials/speed/high-performance-animations/)
    by Paul Lewis and Paul Irish
-   [CSS animations and transitions performance: looking inside the
    browser](http://blogs.adobe.com/webplatform/2014/03/18/css-animations-and-transitions-performance/)
    by Max Vujovic
-   [Animations and
    Performance](https://developers.google.com/web/fundamentals/design-and-ui/animations/animations-and-performance)
    by Paul Lewis and Sam Thorogood
-   [Stick to Compositor-Only Properties and Manage Layer
    Count](https://developers.google.com/web/fundamentals/performance/rendering/stick-to-compositor-only-properties-and-manage-layer-count)
    by Paul Lewis
-   [Tricks for GPU Composited
    CSS](https://dev.opera.com/articles/css-will-change-property/) by
    Sara Soueidan
-   [An Introduction to the CSS will-change
    Property](https://www.sitepoint.com/introduction-css-will-change-property/)
    by Nick Salloum
-   [Animating CSS
    properties](https://developer.mozilla.org/en-US/docs/Tools/Performance/Scenarios/Animating_CSS_properties)
    by MDN


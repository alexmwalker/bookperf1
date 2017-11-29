---
template: chapter.pug
title: 'Front End Performance' 
chaptertitle: '7 Performance Tips for Jank-free JavaScript Animations'
chapterauthor: Maria Antonietta Perna 
---

The role of web animation has evolved from mere decorative fluff to [serving concrete purposes][1] in the context of user experience — such as providing visual feedback as users interact with your app, directing users' attention to fulfill your app's goals, offering visual cues that help users make sense of your app's interface, and so on.

To ensure web animation is up to such crucial tasks, it's important that motion takes place at the right time in a fluid and smooth fashion, so that users perceive it as aiding them, rather than as getting in the way of whatever action they're trying to pursue on your app.

One dreaded effect of ill-conceived animation is **jank**, which is explained on [jankfree.org][2] like this:

> Modern browsers try to refresh the content on screen in sync with a device's refresh rate. For most devices today, the screen will refresh 60 times a second, or 60Hz. If there is some motion on screen (such as scrolling, transitions, or animations) a browser should create 60 frames per second to match the refresh rate. Jank is any stuttering, juddering or just plain halting that users see when a site or app isn't keeping up with the refresh rate.

If animations are janky, users will eventually interact less and less with your app, thereby negatively impacting on its success. Obviously, nobody wants that.

In this article, I've gathered a few performance tips to help you solve issues with JavaScript animations and make it easier to meet the 60fps (frame per second) target for achieving smooth motion on the web.

## 1. Avoid Animating Expensive CSS Properties

Whether you plan on animating CSS properties using [CSS Transitions][3]/[CSS keyframes][4] or JavaScript, it's important to know which properties bring about a change in the geometry of the page (layout) — meaning that the position of other elements on the page will have to be recalculated, or that painting operations will be involved. Both layout and painting tasks are very expensive for browsers to process, especially if you have several elements on your page. As a consequence, you'll see animation performance improve significantly if you avoid animating CSS properties that trigger layout or paint operations and stick to properties like transforms and opacity, because modern browsers do an excellent job of optimizing them.

On [CSS Triggers][5] you'll find an up-to-date list of CSS properties with information about the work they trigger in each modern browser, both on the first change and on subsequent changes.

![CSS Triggers Website][6]

Changing CSS properties that only trigger composite operations is both an easy and effective step you can take to optimize your web animations for performance.

## 2. Promote Elements You Want to Animate to Their Own Layer (with Caution)

If the element you want to animate is on its own compositor layer, some modern browsers leverage hardware acceleration by offloading the work to the [GPU][7]. If used judiciously, this move can have a positive effect on the performance of your animations.

To have the element on its own layer, you need to **promote** it. One way you can do so is by using the [CSS will-change][8] property. This property allows developers to warn the browser about some changes they want to make on an element, so that the browser can make the required optimizations ahead of time.

However, it's not advised that you promote too many elements on their own layer or that you do so with exaggeration. In fact, every layer the browser creates requires memory and management, which can be expensive.

You can learn the details of how to use `will-change`, its benefits and downsides, in [An Introduction to the CSS will-change Property][9] by Nick Salloum.

## 3. Replace setTimeOut/setInterval with requestAnimationFrame

JavaScript animations have commonly been coded using either [setInterval()][10] or [setTimeout()][11].

The code would look something like this:

```js    
var timer;
function animateElement() {
  timer = setInterval( function() {
    // animation code goes here
  } , 2000 );
}

// To stop the animation, use clearInterval
function stopAnimation() {
  clearInterval(timer);
}
```    

Although this works, the risk of jank is high, because the callback function runs at some point in the frame, perhaps at the very end, which can result in one or more frames being missed. Today, you can use a native JavaScript method which is tailored for smooth web animation (DOM animation, canvas, etc.), called [requestAnimationFrame()][12].

`requestAnimationFrame()` executes your animation code at the most appropriate time for the browser, usually at the beginning of the frame.

Your code could look something like this:

```js   
function makeChange( time ) {
  // Animation logic here

  // Call requestAnimationFrame recursively inside the callback function
  requestAnimationFrame( makeChange ):
}

// Call requestAnimationFrame again outside the callback function
requestAnimationFrame( makeChange );
```

[Performance with requestAnimationFrame][13] by Tim Evko here on SitePoint offers a great video introduction to coding with `requestAnimationFrame()`.

## 4. Decouple Events from Animations in Your Code

At 60 frames per second, the browser has 16.67ms to do its job on each frame. That's not a lot of time, so keeping your code lean could make a difference to the smoothness of your animations.

Decoupling the code for handling events like scrolling, resizing, mouse events, etc., from the code that handles screen updates using `requestAnimationFrame()` is a great way to optimize your animation code for performance.

For a deep discussion of this optimization tip and related sample code, check out [Leaner, Meaner, Faster Animations with requestAnimationFrame][14] by Paul Lewis.

## 5. Avoid Long-running JavaScript Code

Browsers use the main thread to run JavaScript, together with other tasks like style calculations, layout and paint operations. Long-running JavaScript code could negatively impact on these tasks, which could lead to frames being skipped and janky animations as a consequence. Therefore, simplifying your code could certainly be a good way to ensure your animations run smoothly.

For complex JavaScript operations that don't require access to the DOM, consider using [Web Workers][15]. The worker thread performs its tasks without impacting on the user interface.

## 6. Leverage the Browser’s DevTools to Keep Performance Issues in Check

Your browser's developer tools provide a way to monitor how hard your browser is working to run your JavaScript code, or that of a third-party library. They also provide useful information about frame rates and much more.

You can access the Chrome DevTools by right-clicking on your web page and selecting _Inspect_ inside the context menu. For example, recording your web page using the Performance tools will give you an insight into the performance bottlenecks on that page:

![Performance tab in Chrome dev tools][16]

Click the _record_ button, then stop the recording after a few seconds:

![Once the animation is stopped][17]

At this point, you should have tons of data to help you analyze your page's performance:

![Data from the Performance dev tools in Chrome][18]

This [Chrome DevTools Guide][19] will help you get the most out of DevTools for analyzing performance and lots of other kinds of data in your Chrome browser. If Chrome isn't your browser of choice, it's no big deal, as most modern browsers nowadays ship with super powerful DevTools that you can leverage to optimize your code.

## 7. Use an Off-screen Canvas for Complex Drawing Operations

This tip relates specifically to optimizing code for [HTML5 Canvas][20].

If your frames involve complex drawing operations, a good idea would be to create an off-screen canvas where you perform all the drawing operations once or just when a change occurs, and then on each frame just draw the off-screen canvas.

You can find the details and code samples relative to this tip and lots more in the [Optimizing Canvas][21] article on MDN.

## Conclusion

Optimizing code for performance is a necessary task if you don't want to fail users' expectations on the web today, but it's by no means always easy or straightforward. There might be several reasons why your animations aren't performing well, but if you try out the tips I listed above, you'll go a long way towards avoiding the most common animation performance pitfalls, thereby improving the user experience of your website or app.

[ ![Maria Antonietta Perna][22]][23]

Maria Antonietta Perna is co-Editor of the HTML/CSS Channel at SitePoint and a front-end web developer. She enjoys tinkering with cool CSS standards and is curious about teaching approaches to front-end code. When not coding for the web or not writing for the web, she enjoys philosophy books, long walks and good food.

[1]: https://www.smashingmagazine.com/2017/01/how-functional-animation-helps-improve-user-experience/
[2]: http://jankfree.org/
[3]: https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Transitions/Using_CSS_transitions
[4]: https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Animations/Using_CSS_animations
[5]: http://csstriggers.com/
[6]: https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2017/11/1511232875csstriggers.png
[7]: https://en.wikipedia.org/wiki/Graphics_processing_unit
[8]: https://developer.mozilla.org/en-US/docs/Web/CSS/will-change
[9]: https://www.sitepoint.com/introduction-css-will-change-property/
[10]: https://developer.mozilla.org/en-US/docs/Web/API/WindowOrWorkerGlobalScope/setInterval
[11]: https://developer.mozilla.org/en-US/docs/Web/API/WindowOrWorkerGlobalScope/setTimeout
[12]: https://developer.mozilla.org/en-US/docs/Web/API/Window/requestAnimationFrame
[13]: https://www.sitepoint.com/watch-performance-with-requestanimationframe/
[14]: https://www.html5rocks.com/en/tutorials/speed/animations/
[15]: https://developer.mozilla.org/en-US/docs/Web/API/Web_Workers_API/Using_web_workers
[16]: https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2017/11/1511232943chrome-tools-performance-access.png
[17]: https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2017/11/1511233012chrome-dev-tools-stop.png
[18]: https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2017/11/1511233059chrome-dev-tools-data.png
[19]: https://developers.google.com/web/tools/chrome-devtools/
[20]: https://developer.mozilla.org/en-US/docs/Web/API/Canvas_API
[21]: https://developer.mozilla.org/en-US/docs/Web/API/Canvas_API/Tutorial/Optimizing_canvas
[22]: https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2015/01/1422003380perna-96x96.jpg
[23]: https://www.sitepoint.com/author/mperna/

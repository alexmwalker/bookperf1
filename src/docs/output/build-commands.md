# Build a Book from HTML chapters

## Rewriting Links in MD to be local

To re-write the image URLs in a single MD file (let's say 'test.md'), move to the 'md' folder in terminal and type:

```shell
    gsed -i 's/\(!\[.*\](\).*\/\([^)]*\)\()\)/\1..\/images\/\2\3/g' test.md
```

To hit the entire folder of MD docs:

```shell
gsed -i 's/\(!\[.*\](\).*\/\([^)]*\)\()\)/\1..\/images\/\2\3/g' *.md
```

## Use CAT in terminal to concatenate the HTML files

In the end, this works best. We have 'book-head.html' and 'book-foot.html' that will _bookend_ your CAT command.

The very end of the commend has your output file. Something like `..lastinputfile.html > ../output/book.html`

> cat book-head.html cover.html 0front-matter.html 1WhichBrowsersShouldYourWebsiteSupport.html 2AreYourWordPressThemesFlexibleOrFast.html 3LazyLoadImages.html 4OptimizingCSS-IDSelectorsMyths.html 5OptimizingCSS-AnimationPerformance.html 6LightningFastPrefetching.html 7OptimizingWebfonts.html 8JSPerfOptTips.html  9JavaScriptPerfOpTips-JankFreeAnimation.html 10WhatIsaCDN.html book-foot.html > ../output/book.html

### This CSS SHOULD be in the 'book-head.html' file. Check it is.

```html
    <link rel="stylesheet" href="../assets/css/book.css">
```

### This JS should be in the 'book-foot.html'. Check is it.
```html
    <script src="../assets/js/book.js"></script>
    <script src="../assets/js/prism.js"></script>
```

### PRINCE conversion to PDF - sexy mutha..

    prince book.html -o frontend-performance.pdf --javascript
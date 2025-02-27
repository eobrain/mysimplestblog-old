---  
slug: swipe1
title: Adding swipe actions to a web site.
created: 2019-09-30 07:00:00+00:00
---  
If you are reading this article on a phone or other touch device, did you know you can swipe left and right to get to other articles? Go on, try it now. I'll wait.

[*Edit 2023-02-07: Actually you will need to try this on the [old version of this blog][2].*]

This turned out to be easy to implement thanks to [Hammer.js][1].  Here's how I did it.

1. I downloaded the Hammer.js library into my JavaScript directory:

   ```sh
   wget https://hammerjs.github.io/dist/hammer.min.js
   ```

   and creates a new file called `swipe.js` in that directory.
2. I added the following to the HTML template of my article pages:

   ```html
   <script src="/js/hammer.min.js" defer></script>
   <script src="/js/swipe.js" defer></script>
   ```

   This loads the Hammer library and my application code into the web page. As an optimization I add the `defer` attribute so that the script loading does not block the initial page load: after all this is optional stuff that should not slow sown the normal functionality of the page.
3. I already had the following navigational structure in the article pages for going to the previous and next pages:

   ```html
   <nav>
       <a id="prev" href="...">&laquo; ...</a>
       <a id="next" href="...">... &raquo;</a>
   </nav>
   ```

   The JavaScript can pluck out the destination URLs from the `href` attributes of the `#t` and `#prev` elements.
4. I added the following to `swipe.js`:

   ```js
   const mc = new Hammer(document.body)

   mc.on('swipe', (ev) => {
     const dir = (ev.deltaX < 0) ? 'next' : 'prev'
     const a = document.getElementById(dir)
     if (a) {
       window.location = a.href
     }
   })
   ```

   This code initializes Hammer.js and sets up a swipe lister. When a swipe is detected the code looks at the `deltaX` event property to determine if this was a left or right swipe, pulls out the URL from the `href` of the corresponding link, and navigates to that link.

This simple solution works fine, but there is no feedback to the user to show that a swipe has happened. In the next article I'll show how to add animation to improve the UX.

[1]: http://hammerjs.github.io/
[2]: https://old.eamonn.org/programming/2019/09/30/swipe.html


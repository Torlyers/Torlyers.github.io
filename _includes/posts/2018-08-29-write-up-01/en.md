> Solution Configration & Customized Fragment Shader


## Summary

Meanwhile, we still leverage [Vue.js](http://vuejs.org/) to boost our productivity. You may have heard of Vue as a rival of React or Angular, but Vue's lightweight and performance make it also a perfect replacement of traditional "jQuery/Zepto + template engine" stack when engineering a Multi-page app. We built every component as [Single File Components](http://vuejs.org/v2/guide/single-file-components.html) so they can be easily shareable between pages. The declarative-ness plus reactivity Vue offered help us manage both code and data flow. Oh, did I mention that [Vue is progressive](https://www.youtube.com/watch?v=pBBSp_iIiVM)? So things like Vuex or Vue-Router can be incrementally adopted if our site's complexity scales up, like...migrating to SPA again? (Who knows...)
 
In 2017, PWA seems to be all the rage, so we embark on exploring how far can our Vue-based Multi-page PWAs actually go.

## Static Library, References & Dependency

I love [PRPL pattern][1] because it gives you a high-level abstraction of how to structure and design your own PWA systems. Since we are not rebuild everything from scratch, we decided taking implementing PRPL as our migration goal:

![](/img/in-post/post-eleme-pwa/Lighthouse-before.png)

**In [Lighthouse](https://developers.google.com/web/tools/lighthouse/) simulation (3G & 5x Slower CPU), we made Time-To-Interactive around 2 seconds,** and this was benchmarked on our HTTP1 test server. 

The first visit is fast. The repeat visit with Service Worker is even faster. You can check out this video to see the huge difference between with or without Service Worker:

<iframe width="560" height="315" src="https://www.youtube.com/embed/mbi_WnunJa8" frameborder="0" allowfullscreen></iframe>

## Platform & Project Property Sheet

## Project Files

## Shader Cross OpenGL & DirectX3D

This article is much longer than I could imagine. I am really appreciated if you could get here. So what can we learn from it?


---

## Appendix. Architecture Diagram

![](/img/in-post/post-eleme-pwa/Architecture.png)

[1]: https://developers.google.com/web/fundamentals/performance/prpl-pattern/

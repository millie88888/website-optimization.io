## Website Performance Optimization portfolio project

Your challenge, if you wish to accept it (and we sure hope you will), is to optimize this online portfolio for speed! In particular, optimize the critical rendering path and make this page render as quickly as possible by applying the techniques you've picked up in the [Critical Rendering Path course](https://www.udacity.com/course/ud884).

To get started, check out the repository and inspect the code.

### Getting started
Click <a href="https://github.com/udacity/frontend-nanodegree-mobile-portfolio">HERE</a> to download the project.

####Part 1: Optimize PageSpeed Insights score for index.html
Open the index.html on browser and check the website score on <a href="https://developers.google.com/speed/pagespeed/">Google PageSpeed</a>.
We will see the scores are low, so let's fix from the feedback.

####Part 2: Optimize Frames per Second in pizza.html
Open the views/pizza.html on google developer tool. First we use the timeline to record the function and check the fps.

### How to optimize the project

Part 1: Getting Rid of jank.
Optimizate views/js/main.js 
We see some values are not useful in "function updatePositions". so here is what I fixed.

 ``` bash
function updatePositions() {
// combine the perormance.mark with "measure_frame_duration", "mark_start_frame", "mark_end_frame"
window.performance.mark("measure_frame_duration", "mark_start_frame", "mark_end_frame");

  var items = document.querySelectorAll('.mover');
  var pizzas = items.length;
  var s = document.body.scrollTop / 1250; //pizza moving when scrolling
  var phase; //var "phase" for the pizzas moving at background.
  
  for (var i = 0; i < items.length; i++) {
    phase = Math.sin(s + (i % 5));
    items[i].style.left = items[i].basicLeft + 100 * phase + 'px';
  }
  
}
 ``` 

Part 2: Reduced quantity of pizzas that run as background.
 ``` bash
 // Generates the sliding pizzas when the page loads.
document.addEventListener('DOMContentLoaded', function() {
  window.addEventListener('scroll', updatePositions);
  var cols = 8;
  var s = 256;
  for (var i = 0; i < 31; i++) {
    var elem = document.createElement('img');
    elem.className = 'mover';
    elem.src = "images/pizza.png";
    elem.style.height = "100px";
    elem.style.width = "73.333px";
    elem.basicLeft = (i % cols) * s;
    elem.style.top = (Math.floor(i / cols) * s) + 'px';
    document.querySelector("#movingPizzas1").appendChild(elem);
  }
  updatePositions();
});
 ```

 Part 3: Resize pizzas is less than 5 ms using the pizza size slider on the views/pizza.html.

 ``` bash
  function changePizzaSizes(size) {
    var i = 0;
    var dx = determineDx(document.querySelectorAll(".randomPizzaContainer")[i], size);
    var newwidth = (document.querySelectorAll(".randomPizzaContainer")[i].offsetWidth + dx) + 'px';
    var all = document.querySelectorAll(".randomPizzaContainer");

    for (var i = 0; i < all.length; i++) {
      all[i].style.width = newwidth;
    }
  }
  changePizzaSizes(size);
 ``` 

 Part 4: Fix the index.html to achieves PageSpeed least 90 for mobile and desktop.

* Move the stylesheets to the bottom of index.html and pizza.html.
* Resize image of pizzeria.jpg. The original size was way to big for website.
* From <a href="https://developers.google.com/speed/pagespeed/insights/">PageSpeed Insights</a>. We will see the feedback for some of images that need to compress the size. 


### Optimization Tips and Tricks
* [Optimizing Performance](https://developers.google.com/web/fundamentals/performance/ "web performance")
* [Analyzing the Critical Rendering Path](https://developers.google.com/web/fundamentals/performance/critical-rendering-path/analyzing-crp.html "analyzing crp")
* [Optimizing the Critical Rendering Path](https://developers.google.com/web/fundamentals/performance/critical-rendering-path/optimizing-critical-rendering-path.html "optimize the crp!")
* [Avoiding Rendering Blocking CSS](https://developers.google.com/web/fundamentals/performance/critical-rendering-path/render-blocking-css.html "render blocking css")
* [Optimizing JavaScript](https://developers.google.com/web/fundamentals/performance/critical-rendering-path/adding-interactivity-with-javascript.html "javascript")
* [Measuring with Navigation Timing](https://developers.google.com/web/fundamentals/performance/critical-rendering-path/measure-crp.html "nav timing api"). We didn't cover the Navigation Timing API in the first two lessons but it's an incredibly useful tool for automated page profiling. I highly recommend reading.
* <a href="https://developers.google.com/web/fundamentals/performance/optimizing-content-efficiency/eliminate-downloads.html">The fewer the downloads, the better</a>
* <a href="https://developers.google.com/web/fundamentals/performance/optimizing-content-efficiency/optimize-encoding-and-transfer.html">Reduce the size of text</a>
* <a href="https://developers.google.com/web/fundamentals/performance/optimizing-content-efficiency/image-optimization.html">Optimize images</a>
* <a href="https://developers.google.com/web/fundamentals/performance/optimizing-content-efficiency/http-caching.html">HTTP caching</a>


### Customization with Bootstrap
The portfolio was built on Twitter's <a href="http://getbootstrap.com/">Bootstrap</a> framework. All custom styles are in `dist/css/portfolio.css` in the portfolio repo.

* <a href="http://getbootstrap.com/css/">Bootstrap's CSS Classes</a>
* <a href="http://getbootstrap.com/components/">Bootstrap's Components</a>



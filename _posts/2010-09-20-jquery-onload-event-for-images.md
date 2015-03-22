---
title: "jQuery onload event for images"
---

When you want to perform some action when images finish to load, you may be surprised sometimes as <em>onload</em> event is not called when image is already cached.

To make it work for all images you simply need also to check <code>this.complete</code> and <code>this.readyState</code>. 

Here's example for jQuery:

<pre>
jQuery.fn.whenLoaded = function(fn){
  return this.each(function(){
    // if already loaded call callback
    if (this.complete || this.readyState == 'complete'){
      fn.call(this);
    } else { // otherwise bind onload event
      $(this).load(fn);
    }
  });
}

$('img').whenLoaded(function(){
  console.log(this.src + ' loaded');
});
</pre>

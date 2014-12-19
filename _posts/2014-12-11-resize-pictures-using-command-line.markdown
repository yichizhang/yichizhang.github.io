---
layout: post
title:  "Resize photos with 'sips' and  Mac OS X's Terminal"
date:   2014-12-11 17:41:00
---

Last month, I decided to sell my old MacBook on eBay. So I took pictures of it. When I was about to upload them, I realised that the photos take a lot of space - the largest is 4.5MB! That's going to take a while to upload.

![Very large image files]({{ site.url }}/assets/photos_before_resize.png)

I found a way to resize all the photos quickly with 'sips' by executing a simple command.

Just open Terminal, change the directory to where your photos are located, then run this command:

{% highlight console %}
sips -Z 1024 *.jpg
{% endhighlight %}

-Z: Maintain original image's aspect ratio;
640: Maximum height and width
*.jpg: Files to be resized. In this case, *.jpg means 'all the files in the directory that has "jpg" as its extension'

Result:

![sips downsized all my images]({{ site.url }}/assets/photos_after_resize.png)
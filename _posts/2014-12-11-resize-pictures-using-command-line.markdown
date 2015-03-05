---
layout: post
title:  "Resize photos with 'sips' and  Mac OS X's Terminal"
date:   2014-12-11 17:41:00
---

Last month, I decided to sell my old MacBook on-line. So I took pictures of it. When I was about to upload them, I realised that the photos took a lot of space - the largest being 4.5MB! That's going to take a while to upload, if the application does not downsize the files before upload.

Very large image files:

![Very large image files]({{ site.url }}/assets/2014/02/photos_before_resize.png)

There is an easy way to down size all of them, with 'sips'.

'sips' is a command line tool that you can use to manipulate images.

In this case, we want to make all the images smaller. Just open Terminal, change the directory to where your photos are located, then run this command:

{% highlight console %}
sips -Z 1024 *.jpg
{% endhighlight %}

Meaning of the parameters:

-Z: Maintain original image's aspect ratio;

640: Maximum height and width

*.jpg: Files to be resized. In this case, *.jpg means 'all the files in the directory that has "jpg" as its extension'



Result:

![sips downsized all my images]({{ site.url }}/assets/2014/02/photos_after_resize.png)
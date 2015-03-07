---
title: "Sleek File Uploads with JavaScript"
layout: post
tags: JavaScript HTML
---

Uploading files in HTML documents using JavaScript has gotten easier, but is still not as simple as it could be.
<!--more-->

<!-- {% gist rchowe/820fa7d3388ec390c6a8 %} -->

For my [Raspberry Pi Music Server]({{ site.baseurl }}/2015/01/04/raspberry-pi-music-server.html), I wanted to have a
dead simple way of uploading files, similar to the one seen on [Imgur](http://www.imgur.com). Imgur's file upload dialog
looks like this:

![Imgur Upload Page]({{ site.baseurl }}/assets/2015-01-28-imgur-upload.png)

It offers four options:

1. A button which triggers the system file selection dialog.
2. Pasting a URL and downloading the image from the URL.
3. Dragging and dropping to anywhere on the page.
4. Pasting an image onto the page.

Once an image is obtained using any of these methods, it adds it to a growing list of images to upload below the file
picker, so that images can be previewed and multiple images can be uploaded at once.

Since I wanted to make it very easy to get songs into the web frontend, I attempted to provide all of these options,
with mixed success. My first pass at an upload form looks like this:

![My Upload Page]({{ site.baseurl }}/assets/2015-01-28-upload-dialog.png)

### File Selection Button

The file selection button was one of the easiest. HTML already has a very simple way to select files:

{% gist rchowe/f101aa93fdd0e8da3afd %}

This produces a file selection button, styled in the standard system way. CSS styles cannot do too much to the selection
dialog itself, and the button looks somewhat ugly. However, Imgur manages to achieve this using a styled button, so I
set out to do the same. For backwards compatibility purposes with older browsers, I left the working upload form alone
in the HTML source and set out to have a script replace the button when it was loaded. I have a script which hides the
file upload button by setting its opacity to 0% and prepends a normal Bootstrap button to it which I can style. I then
added an event to the new button so that when it was clicked it programmatically clicked the old file upload button.
This was achievable in five lines of CoffeeScript:

{% gist rchowe/a4aa5024fa68b1e0194b %}

This gave me a styleable button instead of the ugly, non-styleable file picker. Since the other two main components of
the file picker are going to be implemented elsewhere (drag and drop and previewing files) and without JavaScript the
file picker gracefully degrades to a normal HTML form, I did not feel bad about hiding the native element from the user.
If you look carefully, you can see where the 0% opacity file picker is: you can still click on the file picker (it has
the same effect as clicking on the button) and interact with it even though it's invisible. I'm going to have to
experiment with using `display: none` to hide it and see how browsers react. I think my biggest gripe with it is that
it seems to be disrupting the table alignment.

![The 0% opacity file picker]({{ site.baseurl }}/assets/2015-01-28-file-picker.png)

### Get File from URL

Allowing users to request the URLs of arbitrary files is a tricky problem: it is a very bad idea to allow your server to
download the file, as this could be used to DDOS some other server or to access other servers on an internal network.
However, most browsers do not allow easily loading remote content. Imgur seems (I'm guessing) to get around this by
exploiting most browsers' willingness to download images to provide the previews, however this does not generalize well
to arbitrary files.

I think that this might be a feature that is more trouble than it is worth for non-images, especially considering that
the majority of MP3 download sites (A) are illegally hosting MP3s and (B) provide temporary links that are only active
once.

### Drag and Drop

Drag and drop was relatively easy. The JavaScript File API adds `dragover` and `drop` events to DOM objects, and
provides access to local files using the File API. When a user drags a file over the window, I show a hidden `div` which
is styled to cover the screen with `position: fixed`. This serves two purposes: first, to prevent the user from dragging
the file into a search box, and second, to provide some visual feedback that files can be dropped there, and second, to
set the `event.dataTransfer.dropEffect` parameter to `copy` to make the cursor go into copy mode. The code to do this is
very simple:

{% gist rchowe/ea0bb0b6fe50b74b8950 %}

In Safari, which I am testing this in first, the <code>dragover</code> event does not trigger until the file is above
an element, even though the event handler is defined on the body. I believe that Imgur handles this by having a large
amount of their landing page covered in elements, but I am not sure of that.

### Copy/Paste

Copy/Paste doesn't work on all platforms on Imgur (try changing your user agent to IE) so I figured I would give it a
best-effort try. I found a [StackOverflow answer](http://stackoverflow.com/a/6338207) about how Google does it, so I
figured I'd give it a try. However, it seems to be the same code Imgur uses, since I have only had it work in Google
Chrome.

{% gist rchowe/ba117e897e0953cb4e09 %}

### The Final Picker UI

Removing the URL download option, I changed the file picker to be a list instead of a table, and added icons (from the
standard bootstrap glyphicons) for each of the three choices. I decided not to make the entire "Upload from Computer"
element a button, since I decided for a minimum viable product it doesn't need to be.

![The Final File Picker UI]({{ site.baseurl }}/assets/2015-01-28-final-file-picker.png)

Additionally, when a file is input through any of the three methods, it shows up in a file upload list below the upload
options. I haven't started playing with a JavaScript API to read the metadata out of the original files (to get the
title, artist, album, and album art for a preview), though it is something that I would like to implement.

However, the file upload dialog works well enough in the browsers I am testing it in. In the future, I want to add in
some feature detection beyond just looking for the File API and remove options based on what features are present in the
API. The dialog is probably not incredibly stable in all browsers, but for now it's nice to have.

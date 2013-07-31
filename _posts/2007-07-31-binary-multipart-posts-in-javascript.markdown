--- 
wordpress_id: 75
title: Binary multipart POSTs in Javascript
date: 2007-07-31 22:32:57 +00:00
wordpress_url: http://blog.footle.org/2007/08/01/binary-multipart-posts-in-javascript/
layout: post
---
We recently released a very slick <a href="http://blog.wesabe.com/index.php/2007/07/25/the-wesabe-firefox-uploader/">Firefox extension</a> at <a href="https://www.wesabe.com">Wesabe</a>. It was written by my colleague Tim Mason, but I helped figure out one small piece of it&mdash;namely, how to do binary multipart POSTs in Javascript&mdash;and since it involved many hours of hair-pulling for both of us, I thought I'd share the knowledge. (Tim says I should note that "this probably only works in Firefox version 2.0 and greater _and_ that it uses Mozilla specific calls that are only allowed for privileged javascript&mdash;basically only for extensions.")

One of the cool features of the plugin is the ability to take a <a href="http://blog.wesabe.com/index.php/2007/07/25/one-more-thing-browser-snapshot-and-file-attachments/">snapshot</a> of a full browser page and either save the snapshot to disk or upload it to Wesabe (so you can, for example, save the receipt for a web purchase along with that transaction in your account). The snapshot is uploaded to Wesabe via a standard multipart POST, the same way that a file is uploaded via a web form.

Tim was having trouble getting the POST to work with binary data at first, and he had other things to finish, so he wanted to just base-64-encode it and be done with it. I was reluctant to do that, as the size of the upload would be significantly larger (about <a href="http://en.wikipedia.org/wiki/Base64">137%</a> of the original). Also, Rails didn't automatically decode base-64-encoded file attachments. But Tim had other bugs to fix, so I submitted a <a href="http://dev.rubyonrails.org/ticket/9016">patch</a> to Rails to do the base-64 decoding. I was pretty proud of this patch until it was pointed out to me that <a href="http://www.w3.org/Protocols/rfc2616/rfc2616-sec19.html#sec19.4.5">RFC 2616</a> specifically disallows the use of Content-Transfer-Encoding in HTTP. Doh. They also realized that it is a colossal waste of bandwidth.

Since Tim was cramming to meet a hard(-ish) deadline set for the release of the plugin, I offered to lend my eyeballs to the binary post problem. This could be a very long story, but I'll just get to the point: you can read binary data in to a Javascript string and dump it right out to a file just fine, but if you try to do any concatenation with that string, Javascript ends up munging it mercilessly. I'm not sure whether it is trying to interpret it as UTF8 or if it terminates it as soon as it hits a null byte (which is what seemed to be happening), but regardless, doing <code>"some string" + binaryData + "another string"</code>, as is necessary when putting together a mutipart post, just does not work.

The answer required employing Rube Goldbergian system of input and output streams. The seed of the solution was found on <a href="http://developer.taboca.com/cases/en/XMLHTTPRequest_post_utf-8/">this post</a>, although that didn't explain how to mix in all of the strings needed for the post and MIME envelope. So here it is, in all it's goriness:

<script src="https://gist.github.com/1223431.js"> </script>

<strong>Update:</strong> Spaces removed from multipart boundary per Gijsbert's suggestion in the comments (thanks!).

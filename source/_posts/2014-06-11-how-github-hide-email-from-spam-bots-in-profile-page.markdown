---
layout: post
title: "How github hide email from spam bots in Profile page"
date: 2014-06-11 14:29:29 +0530
comments: true
author: Fizer Khan
categories: ["Github"]
---

As a i am a developer, i uses Github a lot. I love it more than any site.
I used to see my Github profile page because i feel very happy to see a heat chart with
lot of green dots. Since my network is somewhat slow, all the time i could
see `{email}` instead of my email address. I thought it is due to client
side rendering. But it does not happen for all other fields like `name`, `location`
and `website`. Only `email` field is rendered in client side.

`Why?` I google it and I found that it is to avoid `Spam Bots` to crawl the email address.
Basically, The spam bots crawls the site and check for email address and then used them
to send ads and other unwanted mails. You might see email address in the site like
`kumar [at] gmail [dot] com`. This method is also used in the past to confuse spam bots.

Lets see how Github might implemented this method. Following is the html code
returned from server for Github Profile page(eg: [https://github.com/visionmedia](https://github.com/visionmedia))

```
 <a
     class="email js-obfuscate-email"
     data-email="%74%6a%40%76%69%73%69%6f%6e%2d%6d%65%64%69%61%2e%63%61"
     href="mailto:{email}">
       {email}
 </a>
```

You can see `{email}` in the place of email address and another field `data-email`.
[fajarkoe](http://stackoverflow.com/users/2151331/fajarkoe) explains how it works

```
The content of data-email is just the hexadecimal version of your email address
"tj@vision-media.ca".

It is a sequence of hexadecimal characters, where each character is of the
form %XY, where X and Y are hexadecimal digits (0-f). For example,
the first two hexadecimal characters in your case are %66 and %69.
If you look at the ASCII table (http://en.wikipedia.org/wiki/ASCII),
the symbol that corresponds to ASCII with hexadecimal number 66 is "f",
while for hexadecimal number 69 is "i".

You can use play around with this tool http://www.asciitohex.com/.
```

Once the page is rendered in the browser, hexa decimal value is converted to `ascii` as follows

```
function hex2a(hexx) {
  var hex = hexx.toString();//force conversion
  var str = '';
  for (var i = 0; i < hex.length; i += 2)
      str += String.fromCharCode(parseInt(hex.substr(i, 2), 16));
  return str;
}

var $email = $('a.js-obfuscate-email');
var hexaEmail = $email.data('email');
hexaEmail = hexaEmail.replace(/%/g, '')
var email = hex2a(hexaEmail);
```

Once you get the email, the email is updated in DOM element as follows

```
var $email = $('a.js-obfuscate-email');
$email.attr('href', 'mailto:' + email);
$email.text(email);
```

That’s All Folks. Hope this tutorial helped you in understanding method to hide email from spam bots.
Please share your thoughts in the comments below.

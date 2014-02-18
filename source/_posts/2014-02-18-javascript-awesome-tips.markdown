---
layout: post
title: "Javascript awesome tips"
date: 2014-02-18 14:42:08 +0530
comments: true
categories: Javascript
---

### Append an array to another array

    var a = [4,5,6];
    var b = [7,8,9];
    Array.prototype.push.apply(a, b);

    uneval(a); // is: [4, 5, 6, 7, 8, 9]

### Closures by example

    function CreateAdder( add ) {

      return function( value ) {

        return value + add;
      }
    }

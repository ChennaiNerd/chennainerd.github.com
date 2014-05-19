---
layout: post
title: "I am a C programmer. Why i love AngularJS?"
date: 2014-02-22 19:01:42 +0530
comments: true
author: Yogeswaran
categories: ["angularjs", "javascript", "html"]
---

The intended audience for this blog are not the people who have profound knowledge in HTML and Javascript.  This is only for those who are in inertia to experiment with web development.  Also, please be aware that this is not beginner's tutorial for AngularJS.

I have this habit of trying some web related stuff rarely once in a year.  Couple of years back on a fine morning started to play with jQuery and was impressed by its power.  But at the end of the day thought there is so much to APIs to remember or at least be aware of. Then decided, it's not going to work out for me and that was the last time I tried anything on the web.

Last week I had a chance to play with AngularJS and my immediate reaction was "WoW!!!". After my first encounter with AngularJS I thought this is not going to be just one day affair and fell in love with AngularJS. During the course of this blog I'll walk you through the features of AngularJS which impressed me.

#### Framework and library:

First of all, AngularJS is not library like jQuery, it is a framework.  So there is no onus of remembering the APIs.

#### Empowering HTML:

HTML was born when the intention of web was only to display static content.  As the name suggests that is only a markup language - nothing wrong, nobody to blame. The people who designed that had only scoped for that.  But now, the web is not that simple.
And being a C programmer I wished if there is anyway to add if condition, loop, block scope in HTML.  AngularJS comes as a prosthetic limb for the handicapped HTML.

To display an element only when a condition is met (i.e.,) having a if construct. This is possible in AngularJS using `ng-if` or `ng-show` directive.

```
<input type="text" ng-model="customer.name" ng-if="customer.edit">
<span ng-bind="customer.name" ng-if="!customer.edit"></span>
```

If customer.edit is set true, customer.name will be displayed in text box, else will be displayed in span.


To iterate through a array of items and create a `<ul>` or `<li>` or even populate a
`<table>`.  `ng-repeat` directive does the job.

```
<table>
    <thead>
        <tr>
            <th>Name</th>
            <th>Amount</th>
            <th>ROI</th>
        </tr>
    </thead>
    <tbody>
        <tr ng-repeat="customer in customers">
            <td ng-bind="customer.name"></td>
            <td ng-bind="customer.amount"></td>
            <td ng-bind="customer.roi"></td>
        </tr>
    </tbody>
</table>
```

To restrict the scope of variables and functions within a block, `ng-controller` can be used.
Though the use of `ng-controller` is more than that.

```
<script>
    angular.module("bank", []).
    controller('BankController',
        function BankController($scope) {
            $scope.customers = [name:'kansamy', amount:'1000', roi:'9', edit:0}, {name:'munsamy', amount:'300', roi:'2', edit:0}];
            $scope.customer = {name:'', amount:'', roi:'', edit:0};
        });
<script>
<body ng-app="bank" ng-controller="BankController">
...
</body>
```

The scope of bank controller is throughout the `<body>`.
This scope can be restricted to any `<div>` or any other element by setting
`ng-controller` accordingly. `$scope` variable has a special meaning mapping to
the scope of the controller.

There is much more talk to about AngularJS - what I have captured here is only the picture of love at first sight.

I have hosted a simple no use application exercising these features of AngularJS
[https://brilliant-fire-1717.firebaseapp.com/bank.html](https://brilliant-fire-1717.firebaseapp.com/bank.html)

No wonder, these features can one day become integral part of HTML.


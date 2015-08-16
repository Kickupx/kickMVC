# kickMVC
MVC which uses pure HTML/XML to allow web designers to write websites fast. (Under development)

Why
---

I think todays CMS and MVC frameworks offer way to little flexibility unless you have full understanding of the internal workings. This is an attempt to build the base for a Web framework which separates settings in a way which makes it very easy to allow automatic generation of admin pages in a fully featured CMS which is directly built for the user.

Goals
-----
The ulimate goal is to provide a flexible framework which to 95% can do this:
* Describe a meaningful part of a web platform in one html page (like a blog)
* Offers automatic html admin page generation (and then it happens, it becomes an CMS)
* As HTML compliant as possible
* Almost automatic escaping. Both from the backend guy and the frontend guys point of view

How
---
The basic idea is to let all information which is only relative under the development of the website to be expressed in one html file. This includes information to the backend about how long usernames should be or declaring a part of a document as a tree of settings and anything in between. Because information declared in the html file is flowing not only towards the frontend but also to the backend a normal template engine is not suitable. Since the information in those is usually only flowing in one direction (to the frontend). So the html file is no longer a raw piece of data which is sent to the user. It also contains information about how the backend will behave (eg. login pages) and also describes GUI in (to me) nice way.

What it will not handle
-----------------------
* AJAX
* Other nonHTML requests
* Backends which magically work

The GUI part
------------
In a normal CMS we often think about settings which the user will be able to change in terms of a single setting, a list or a tree. And there are no difference here, but because we also want the page to be as xml compliant as possible there are no for loop or anything like that. Instead it uses attributes.

```html
<body>
  <p kick_name="setting"></p>
  <div kick_name="aList" list>
    <p kick_name="setting_inList"></p> <!-- All subelements of a list is a type. Or in this case a list type. This is                                                    required in the setting part later -->
    <p kick_name="setting"></p> <!-- Settings in list and settings in a "global scope" can have equal names -->
  </div>
  
  <div kick_name="aTree" tree>
    <div kick_name="inTree"> <!-- Tree type -->
      <div insertpoint></div> <!-- All tree types require an insertpoint. Inside this html node you can stick in other tree                                     types -->
    </div>
  </div>
</body>
```
Notice you can also have trees in trees and trees in lists if you would like. Like normal HTML it is very flexible.
Okey, now have enough to manage the most common settings and stuff you want the end user to be able to change.
But how about the real features. Like blogs and loginpages. How are they handled?

The backend-part
----------------
The base of everything in the backend-world is called the RouteBase. The RouteBase is bound to a static base url route 
and also provides all the different routes below the base. But it also feeds keys and backend-types to the framework.
Keys are keywords which does an action when they are accounted in the html file. Keys are intended to be used for handling
login and simular. Backend-types is basicly a helper to the Key or RouteBase. They give information to the backend is 
intentionally made to represent settings for a loginpage or other.
Remark to be able to use a single html file for the whole routebase the current route decides what keys are in use
or activated and there the not activated keys are. Are the that html node and all it's children removed and never sent.

This can be seen like a hierarchy:
* RouteBases (Eg. the route root of a blog)
  * Routes (All the different routes the pagetype supports)
    * Activated keys (The keys which are used for a request)
  * Keys (Denotes a part of a html document to be interesting for a backend operation like a login-page)
  * Backend-options(Information which flows from the html page and then either to a key or the route base)


TODO
----
*Add test cases
*Add escaping
*Fix tree native key

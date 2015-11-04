---
title: The Model View Presenter Architecture
layout: default
---
# The MVP (Model View Presenter) Architecture #
## and how it relates to the MVC and all his variants ##

In his primordial form the MVC pattern have the main intent of modularize a graphical application or a graphical component in three main sub-units each with a separate set of concern. The value of such division is to improve code clearness of intent and thus a higher maintainability.

In the earlier MVC we had a complete circle in which the controller processed the input and fiddled the model to change its state, this change is messaged to the view that update itself and waits for further interaction from the user which will be passed to the controller and so another cycle begins.

I would argue that even if the system is now more modular, the three components are still coupled. In fact they have a cyclic dependency that theoretically is a bad symptom if you are looking for low coupling.

With time the MVC evolved, especially thanks to the web and the Struts 2 framework it morphed into a horseshoe, lowering coupling between the model and the view through the use of more generic ways for the view to retrieve data from the model, adding a layer of indirection between the two. This way you can switch between views and expect a common interface.

The MVP architectural pattern was born about 20 years later and featured a lot of 

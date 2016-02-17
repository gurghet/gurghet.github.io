---
title: Domain Driven Design for Web Apps an Illustrative Example
layout: default
---
## Domain Driven Design for Web Apps an Illustrative Example

### Nowadays the overall tendency of webapps is to move (finally!) to a more service oriented back-end. DDD gives you the power to fully exploit that!

Our example application is something we are all familiar with in some way. I’m going to illustrate how to design a simple e-commerce application that sells fashion articles for highly glamorous people of accomplished status.

I’m being specific as much as I can because much of the decisions will have to be driven by domain knowledge. And no, there is no escape there. As much as you’d love to delve into the code we have to stress that everyone in the team has to share a good amount of domain knowledge in the vector that Eric Evans calls *Ubiquitous Language*.

A simple architecture that I love using is an evolution of MVC the Model View Presenter. It fits well our needs because the Presenter does not “control” the system, it merely “presents” it, peeling off from all domain responsibilities and returning them to the *Model*.

Let's get started with a simple presenter for a listing of shoes:

![class ShoeListingPresenter constructor]({{ site.url }}/assets/listing-constructor.png)

Let’s start slow. What did I just code?

What you see is the ShoeListing class with just the constructor implemented. There are two things to note already. The class name is a narrow specification of what the class is in charge to do: present a shoe listing. The other thing is we are *injecting* a service. This is an access service we are going to use to make sure the user is logged in and can access a specific resource.

We are not injecting the model or the view. Weird ah? But think about it.

 - It makes the class a little lighter when *unit testing*
 - If you are using more than one repository it gives you more flexibility and a *slimmer constructor*
 - It encourages *view modularization*. Especially if you are not using a framework (and especially if you *are* using a framework!)

Here is our first method in the ShoeListingPresenter

![show method]({{ site.url }}/assets/show-method.png)

What I’m going to do instead is a bottom-up approach to describe all the little details.

 - The name of the function is just ‘_show_’, it’s pretty *generic*. That’s because the name of the class is *explicative* enough.
 - The real parameter is the *color*. The repository and the view are just pluggable.
 - The method of the access service is *notLoggedIn*, it’s easier to read than a boolean ! at the beginning.
 - There is a little bit of *procedural style code* to add objects to the grid, but that’s ok because this is part of the presentation logic.
 - You can see some more presentation logic to *calculate* the number of rows for the grid.

As a side note, we assume that the shoe object is ready to be displayed but that may not be the case. The presenter should extract from the shoe the necessary attributes to display it, like an image path or a picture width.

---

Let me now expand a little bit.

The Bounded Context we are operating in right now is the front-end. A shoe listing might well has its own baggage of implied information in it. Shoes are to a person like frame is to a canvas, so designers decided to show them in little wooden frames (how imaginative.) Their representation is different enough that it’s simply wrong to write the difference in a ‘if-else’ statement. That’s why they deserve their own presentation class.
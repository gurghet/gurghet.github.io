---
title: The Model View Presenter Architecture
layout: default
---
# The MVP (Model View Presenter) Architecture

## and how it relates to the MVC and all his variants

In his primordial form the MVC pattern have the main intent of modularize a graphical application or a graphical component in three main sub-units each with a separate set of concern. The value of such division is to improve code clearness of intent and thus a higher maintainability.

In the earlier MVC we had a complete circle in which the controller processed the input and fiddled the model to change its state, this change is messaged to the view that update itself and waits for further interaction from the user which will be passed to the controller and so another cycle begins.

I would argue that even if the system is now more modular, the three components are still coupled. In fact they have a cyclic dependency that theoretically is a bad symptom if you are looking for low coupling.

With time the MVC evolved, especially thanks to the web and the Struts 2 framework it morphed into a horseshoe, lowering coupling between the model and the view through the use of more generic ways for the view to retrieve data from the model, adding a layer of indirection between the two. This way you can switch between views and expect a common interface.

The MVP architectural pattern was born about 20 years later and featured a lot of further modularization of the MVC beyond separating the model and the view even further.

It originally replaced the controller with the presenter and added Commands, Selections and the Interactor in the data flow. The presenter is seen as a collection point of data and events whereas the controller is more connection point between the two. The Interactor is placed between the view and the presenter and is a filter that turn raw interface events like "user clicked the save button" into user intentions by contextualising them. The Commands box is placed between the presenter and the Selections. It specifies the available operations one can perform on a specified selection. Selections, placed after Commands but before the model, answer to the question "how do I specify my data?" This using a DDD repository would translate for example in a method like `findByColour` so I can retrieve a set of object to operate on all of the same colour.

Now, the original paper put commands before selections, but then says that commands are performed based on a previous selection. I assume this is an error and they would have to be switched, but apart from that, it also says a couple of things that I don't see in real systems and another couple of things I would tweek to better adhere to DDD.

The first thing to note is that the Interactor immediately becomes part of our model if it has to really contextualize raw user events. When I click a toggle button I may want to either switch it on or off. The Interactor should change the label of the toggle button accordigly, interpret the click as the right action, and also ask the user if he is sure he wants to switch on the lights, say if there is some photographic development in progress and turning on the light could interfere. While the first two concerns are purely view-related, the last one imply knowledge about the domain. I would avoid putting domain knowledge in the Interactor and I would see it more as a place to put complex view logic or ensemble logic for more views.

The second thing is that, according to this early model, the view still communicates with the model. Of course using a distributed notification system is nice but it still requires a data access *from* the view. This means? Yep, you guessed it, coupling between the model and the view. This is sooo practical right? I want the name of a user displayed on the screen, I just write

{% highlight java %}
public void displayUser() {
   print(model.getUserFullName());
}
{% endhighlight %}

and don't forget we also have mobile user who don't want to see the full name because they have small screens so it'll be enough for them to print the first name.

{% highlight java %}
public void displayUser() {
    print(model.getUserFirstName());
}
{% endhighlight %}

Right? Except now we support Chinese, Spanish and Russian and the API for getting the name changed. The concept for name is more generalized and specialized at the same time: there is longName, shortName, surname, patrnonimic, name[0], name[3], etc. We update the mobile view but we forget to update the web view and so our view is partly broken. The interface guys try to fix it but they don't have permission to do so because displaying a legal name in the invoices is not part of their knowledge, it's part of the domain knowledge. We mixed some domain knowledge in the views. What if we did

{% highlight java %}
public void displayUser(String name) {
    print(name);
}
{% endhighlight %}

we would have a view with most of the specification left at deeper level, so that one would have to write at least an `if` statement to distinguish between different view types. This would achieve full decoupling but would leave some view concerns inside the model. A good compromise could be to establish a `Name` concept at the presentation level and have it shared as Ubiquitous Language term.

{% highlight java %}
public void displayUser(Name name) {
    print(name.short());
}
{% endhighlight %}

and in the invoice view:

{% highlight java %}
public void displayUser(Name name) {
    print(name.legal());
}
{% endhighlight %}

This way a reasonable amount of knowledge is expected to be shared by everyone, leaving much of the detail on how a legal name is formed to some domain class.
Now the model can change abruptively and the only thing that gets changed would be the presenter, views can remain completely unchanged.

Selectors are really some facilities built upon repositories like the aforementioned `findByColour` that could be something like this:

{% highlight java %}
public List findByColour(String aColour) {
    Colour colour = new Colour(aColour);
    List allIceCreams = getIceCreamRepository().fetchAll();
    return  allIceCreams.stream()
        .filter(iceCream -> iceCream.colour.equals(colour)
        .collect(Collectors.toList());
}
{% endhighlight %}

Commands in their simplest form are a list of things I can do with the data I gathered. When I have all my red coloured ice-creams I want to put them in the truck. I extend a Command interface for doing this:

{% highlight java %}
public class LoadIceCreamCommand implements Command {
    private final List iceCreams;
    private final Truck truck;

    // some factory methods to create a proper command
    // and populate the attributes above...
    
    @Override
    public void execute() throw IceCreamLoadingException {
        truck.load(iceCreams);
    }
    
    @Override
    public void undo() throw UndoException {
        truck.unload(iceCreams);
    }
} 
{% endhighlight %}

Commands are thus a finite set of classes each of which contains domain knowledge. They directly manipulate domain classes and can be passed around sufficiently easily. It's worth to note that the command does not contain logic to *select* data from the database. Whether we chose the ice-creams by color or by flavour is totally irrelevant to the command. `LoadIceCreamCommand` can load *any* set of ice-creams into *any* truck. A cons is that we have to make explicit whether these arguments are already validated (or we can decide to double-check.)

Let's put everything together to see how the whole system works and let's also try to interpret this pattern with DDD.

Iscream is a food firm that sells ice-creams, it's business mission statement is to scare his customer in as many ways as possible. One of the things this company does is to sell ice-creams of one flavour instead of the requested flavour, and it tricks customers by swapping flavours with the same colour like mint and pitachio or alemond and butter pecan.

The presenter is a receptacle of business methods, and is also at application level, so is a natural place to initiate things like `practicalJokeFlavourExchange`

{% highlight java linenos %}
public class IscreamPresenter implements UserEventListener {

    @Override
    public void practicalJokeFlavourExchange(String aUser,
                                             String aFlavour) {
        User user = new User(aUser);
        Flavour flavour = new Flavour(aFlavour);
        Color color = new Color(flavour.color());

        List<IceCream>
    }

}
{% endhighlight %}

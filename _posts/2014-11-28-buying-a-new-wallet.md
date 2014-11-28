---
layout: default
---

# It’s time for a new wallet! #

Your current wallet is old, worn out, bloated with torn business cards and other stuff.  You need a new wallet! You deserve it, in fact you should get a new one for free, but how do you choose?  In fact, there’s plenty of choice.  I have my personal secret when it comes to evaluate the best wallet: money!  _The first thing you should check for in a new wallet is if there is money in it_.  Make sure to check for coins too.  Now, for the other criterions... well my time is running out, I’m not sure if I can write this in the short time window I have.  I’ll try.

## The old trick of the japanese photographer ##

You don’t have to do all the work by yourself.  Do you know all the japanese people in museums taking pictures of every single piece?  Why do they do that?  Obviously for art sake.  There is a painting in the Ermitage, a painting that’s kept secret, underground in a separate building, away from all the asian people who would like to take a picture of it.  _It’s a painting portraying japanese people taking a picture of that very same painting!_  If some asian guy were to actually photograph it, I can’t even imagine what would happen!

## So, about that wallet? ##

Right right, right on track.  To get the best wallet with the best money, you *would* have to steal it.  Now, since this is *technically* illegal, you would have to find a workaround.  First of all, you should learn about [Bitcoin](http://bitcoin.org).  It’s a new monetary system based on computers. At the time of reading 1 Bitcoin has a value of $<span id="btc-price">0</span>, if you want to get a free wallet with Bitcoins in it, you should... damn it! I don’t have time for the complete explanation.  Time is running out.  We have to take a shortcut.

## Everyone has a superpower ##

My superpower is being able to blow my nose in the curtains while no one is watching, but that’s a story for next time, what’s really important is that you can exploit your superpower to express the best of yourself!  If I could choose my super power, well, I love pesto so it  would be that _everything I touch turned into pesto_.  Except that even my mother would turn into pesto.  I would touch her softly and when he suddenly turn into delicious green goo I’d go: “Nooo!” then I’d look againg and I’d go “Nooo!”.

### My superpower ###

At this point I hear you saying ‘My superpower will be having a free Bitcoin wallet with Bitcoins in it!’.  We are almost there!  There is a thing you should do for me first.  Sounds like a scam now? Like I just want money from you and than I give you rubbish?  No, it’s not, and I can prove it to you.  Do you remember that time you went to dance  wearing that silly Marylin Monroe t-shirt, and all the girl seemed into you? But then, after bringing one girl home you discovered she was lesbian and she really wanted to do was _getting into your t-shirt and make out with Marylin_? What a bummer, uh?

## Conclusion ##

Well that was easy, I hope you’ll enjoy your new wallet and you will _add new money to the money already in it_. If you have any question plese comment below. Oh look, I made it only 2.5 minutes late.

[comment]: <> (Script to display bitcoin price current)
<script src="//code.jquery.com/jquery-1.11.0.min.js"></script>
<script>
jQuery(document).ready(function($) {
$.getJSON("https://api.bitcoinaverage.com/ticker/global/USD/", function(data) {
$("#btc-price").text(data['24h_avg']);
});
});
</script>
[comment]: <> (End of bitcon script)

**Index**

* **Absolute Basics**
* **Easy Vanilla**
* **Medium Vanilla**
* **Advanced Vanilla**


## Absolute Basics

If you've never touched PD before, check out some of the links in the sidebar. [These tutorials helped me a lot.](http://write.flossmanuals.net/pure-data/introduction2/)

If you know basic pd but just haven't really made a GUI before, these tips should help you get started making the simplest GUIs.

**Terms:**

* *subpatch*: A [pd] object that contains a pd patch. Ex: [pd my-subpatch-name] This is only contained within the parent patch. Each subpatch is separate, even if two subpatches have the same name, changes to one do not affect the other.
* *abstraction*: Similar to subpatch, but it is it's own file saved on your computer in the same folder of your parent patch. You use abstractions by creating a new object with the name of your abstraction. Ex: [my-abstraction] You can create as many copies of the abstraction that you want within a patch, but when you edit one of them, the changes get applied to all of them.

**Tips:**

* If you want to create an object with a GUI [like this](http://imgur.com/1xQEf7P.png) right click inside the window of your subpatch / abstraction, select Properties, then [check the box named "Graph-on-parent" and click Apply](http://i.imgur.com/CkKDiTj.png). There should now be a red box inside of the window. This red box defines the area within this patch that will contain the visible GUI in the parent patch. You can edit the size and position of the box in the properties window. 
* Any object that is not completely contained within the red graph-on-parent box will not appear within the visible GUI. [You can have an object's bounds overlap with the red lines like this](https://i.imgur.com/XNYP2IM.png), but as soon as you [go one pixel outside of the red bounds like this](https://i.imgur.com/tGY6SyF.png), the object will be invisible in the GUI in the parent patch.
* If you want to hide the name of the patch, in the properties window check "Hide object name and arguments". This also allows you to make really small GUIs, since it doesn't have to fit the name in the graph-on-parent window. You can make it as small as 1x1 pixels!
* To make GUI creation easier, be sure to use the arrow keys to nudge objects by 1 pixel at a time. You can also hold shift + arrow keys to nudge 10 pixels at a time. I like to use a 10 pixel spacing between objects to make lining everything up easy.
* Subpatches / abstractions can contain other subpatches / abstractions !


## Easy Vanilla

Lets say you've made a vanilla PD patch and made a simple GUI with graph-on-parent.

Something like this. 

![](http://imgur.com/1xQEf7P.png)

The easiest way to enhance a GUI is to add a canvas object to the background. When you add a canvas to an existing patch, it displays on top of everything else. To get around this: 

Select the whole patch \(CTRL+A\), shift click the new canvas object to deselect it, cut, and then paste everything back. 

![](http://i.imgur.com/BLNsnim.gif)

Now you can place this canvas wherever you want, but if you want it to show up in the graph-on-parent GUI, the little blue box needs to be inside the red graph-on-parent rectangle. It's pretty easy to color the whole background this way: 

Set the size of the of canvas object to the size of the graph on parent window, then place it in the top left corner using the arrow keys to nudge it along one pixel at a time. 

![](https://i.imgur.com/VgB7z5g.mp4)

Be careful because the canvas object can extend outside of the graph on parent bounds on the right and bottom, as long as the blue box is inside the graph bounds. Speaking of going out of bounds, this is what our new gui looks like:

![](https://i.imgur.com/Dvois5V.png)

Sleek and stylish! Very modern. However, we lost the border, name, and the inlets / outlets. Maybe you don't need any of these, but if you want them back:

For the border: Subtract 2 from the width and height of the canvas object, then move the canvas down and right 1 pixel. 

![](https://i.imgur.com/XiNkWLJ.png)

For the inlets/outlets: Subtract another 2 from the height, and move the canvas down 1 pixel. 

![](https://i.imgur.com/OcevMEE.png)

For the name: There are a couple of ways you can do this....

* Normal: Subtract another 20 from the height, then move the canvas down 20 pixels. 

![](https://i.imgur.com/chXt2k5.png)
* Better: Instead of resizing the canvas, simply change the canvas's label to the name of the object. You can also change the label's color as well. 

![](https://i.imgur.com/lN2Kd4W.png)

Actually, canvas labels give you a lot of freedom, since you can change their color, you can use them as colored comments! This might not be very efficient, and it probably isn't very performance friendly, but it's easy so lets give it a shot.


## Medium Vanilla

Wouldn't it be nice to have labeled inlets and outlets? 

You can extend the graph-on-parent height by 40 pixels and add comments to the top and bottom, but it doesn't look great. 

![](https://i.imgur.com/qpv1G9t.png)

It'd be nice if we could make it look... *nice*. Well we can do this using canvas objects.

Extend the graph-on-parent window height by 40 pixels, and move the whole GUI down by 20 pixels. Then duplicate the background canvas and move this new canvas up to the top left corner of the red graph-on-parent rectangle. Resize the height of this new canvas to fill the gap, but without covering up anything else. Change the label to the name of the first inlet.

![](http://i.imgur.com/aqt0n8L.gif)

From here you can duplicate this canvas object, resize it to something smaller like 15x15, and move it over to the right to the next inlet you want to label.

![](https://i.imgur.com/WDkOIjo.png) 

If you have a different labeled canvas for each inlet, you end up with something like this.

![](https://i.imgur.com/X2WA8rS.png)

Hey, that's starting to look pretty nice!

Now for the bottom, we can just duplicate the canvas we used for the top left corner, and move it to the bottom left. Let's try inverting the colors. So the canvas is the dark gray, and the label is the light gray.

![](https://i.imgur.com/mpQNt7Q.png)

Not bad! If we want to go for the metro look, we just have to expand the canvases to cover the borders.

Final result.

![](https://i.imgur.com/1uEEmzr.png)

Of course from here you can go crazy with the colors and change things however you like, but I think it's a nice result for the bit of effort that it took to make.

Before we jump into structs in the advanced section, I want to mention the crazy extent that you can, *but shouldn't*, go with canvas objects. We've seen before that you can overlap canvases with the edges of the GUI to hide the border. You can do this with all of the other GUI objects as well.

Behold.

![](https://i.imgur.com/3WKJdJM.png) 

All of the borders of the GUI objects are being covered up by canvas objects. You can see in the patch on the right that at each of the 4 corners of the GUI objects there is a 1 pixel thick canvas object. This is very inconvenient, slow to modify, and all around frustrating to work with. On top of that, you can only make rectangles with canvas objects. What about arbitrary polygons? Thankfully there is a better way.

 
## Advanced Vanilla

[struct] allows you to draw shapes onto a subpatch using [drawpolygon], [filledpolygon], [drawcurve], [filledcurve], [plot], [drawnumber], and [drawsymbol]. 

Lets say that we want to make an abstraction of a flat, metro style slider, like this.

![](http://i.imgur.com/HjY2snA.gif)

This is an abstraction containing a slider with a rectangle drawn on top of it to cover the black outline and inlet / outlet. The rectangle's color is set to match the slider's background color. Here's an exploded view.

![](http://i.imgur.com/5ikbjjD.png)

If you've already used structs before, you can probably build this yourself. If not, then keep reading as I'll try and go into it. 

In order to build this abstraction, create a new file named colored_slider.pd \(or whatever you want the abstraction to be called\) and make a new subpatch like mine, [pd $0-slider-template].

![](http://i.imgur.com/bjT1oKP.png)

That subpatch contains the template for the struct, which includes all of the different variables used for the drawing objects. The template also contains all of the draw objects that we are going to be using, and you'll notice I used two [drawpolygon] objects. I don't like the way that the line thickness option looks in PD, so instead of drawing one 2 pixel thick rectangle, I draw two separate rectangles that are each 1 pixel thick. Also note that x and y are not within the arguments of either of the [drawpolygon] objects. The variables x and y are reserved names that we don't need to specify within the arguments of [drawpolygon]. It always starts drawing at these x y coordinates. In our case we are drawing at 0, 0 so x and y are irrelevant.

Close that subpatch and make a new one next to it. This subpatch will hold the drawing that we are going to make. I called mine [pd $0-slider-data]. Set this subpatch window aside, because to actually draw into this subpatch we need to use the [append] object from the parent patch. We don't have to place anything in here by hand, but we do need the window open so we can see what we are drawing!

Go ahead and take a look at the help file for [append]. It works like this: [append template-name var1 var2 var3...]. 

In our case, it's [append $0-slider-template w h w2 h2 rgb]. This creates 6 inlets, one corresponding to each variable, and the last one on the right is for a pointer to the subpatch we want to draw in. [append] needs this pointer before it can append anything to the subpatch, otherwise it wouldn't know where to append it! To get this pointer, we create a [pointer] object and send it a message to traverse the data subpatch, then bang the first result. After hooking up all of the inlets, your patch should look something like this.

![](http://i.imgur.com/fzesqm1.png)

Note that the pointer object needs to know that we are looking in a subpatch, so make sure to include the "pd" in "pd-$0-slider-data". I had to put this into a symbol object in order to get $0 to resolve correctly in the message box, otherwise it would spit out "pd-0-slider-data" each time. http://write.flossmanuals.net/pure-data/dollar-signs/

If you have done everything correctly up to this point, first bang the message into the pointer object, then bang the first value of append, and you should be seeing a rectangle within the $0-slider-data subpatch!

![](https://i.imgur.com/RO2jR50.png)

If you don't, I'll go through some troubleshooting below.

Troubleshooting:

* Error *"pointer: list 'pd-1003-slider-data' not found"* or similar : make sure you are spelling the subpatch name correctly.
* Error *"pd-#-slider-template.w: no such field"* or similar : This means that append cannot find the variable "w" within the template. Make sure that you set up the struct correctly.
* Error *"append: no current pointer"* : make sure that you bang the message to the pointer object before banging the append object. This might lead to the next error...
* Error *"append: stale pointer"* : This happens when you try to append without having set the pointer, then you set the pointer afterwards. The only way I've gotten it to go away is by closing the patch and reopening it. I don't have a good understanding of pointers in PD, so there might be a better way to fix this.
* No error, but nothing appears in the data patch : Make sure the color rgb isn't set to white (999), and make sure that the w and h values are not negative. Also check your template subpatch, and make sure the draw objects are set up correctly. For example, [drawpolygon] and [fillpolygon] have a different number of arguments, and if you mix them up it might not throw an error, but it will display incorrectly.

If you've gotten to the point of drawing the rectangle in the subpatch, you may have noticed by now that if you modify any of the values and bang [append] again, it creates a new object with those values, and leaves the old object there! We can avoid this by clearing the subpatch using the message \[clear\( to \[send pd-$0-slider-data\].

![](http://i.imgur.com/bCfBlOp.png)

It is inefficient to clear and recreate the strcut object over and over. It would be nice to just modify just one object using [set], but since we are building this into an abstraction we run into a couple of problems:
* If you move the abstraction, it doesn't move the struct that you draw!
* If you are using graph-on-parent and and not clearing the subpatch, that original object never goes away. Not even after you delete the subpatch! You'd have to reload the file for it to disappear.

Whenever you clear the subpatch, you have to  have to give [append] the pointer to it again. The easiest way to do all this is to just use a \[metro\] object to do it automatically every half second or so.

![](http://i.imgur.com/Y7ziv0c.png)

Now with that set up, we want to display the rectangle in the parent patch, not just the subpatch. Naturally, you do this by right clicking on [pd $0-slider-data] -> properties -> graph-on-parent. You should immediately notice that the rectangle is huge!

![](http://i.imgur.com/YYa3RPC.png)

This is due to the range and size options within the canvas properties for the data subpatch. To fix this we need to set the x and y range to match the x and y size.

![](http://i.imgur.com/3lEDXvV.png) 

I picked 10x10 just to show what was going on, but you could pick 1x1 and the graph-on-parent window would be hidden as 1 pixel in the top left corner. It's pretty hard to click on that 1 pixel, so instead we can be tricky and actually set the window size to match the rectangle size. The rectangle will just draw over it!

![](http://i.imgur.com/hUpvEEH.png)

Thankfully you can click through subpatches like this, so all we need to do is move our $0-slider-data subpatch on top of a normal hslider object. Since we want to make this an abstraction, we set graph-on-parent for the parent patch, set it to the size of our slider, and then fit everything inside of the rectangle.

![](http://i.imgur.com/DFG3cII.gif)

From here if you'd like to change the color or label you can create an inlet for the slider's properties.

![](http://i.imgur.com/tVrpagB.png) 

With normal GUI objects like the hslider, you can change their colors using the message "color" like this [color bg fg label( The annoying part is that normal GUI objects and structs use slightly different RGB schemes. Normal GUI objects can be set to R G B values between 0 and 255, but [drawpolygon] [filledpolygon] [drawnumber] ect. only support R G B values *between 0 and 9*. 

To make things easier I made two helper objects for setting the colors.

![](http://i.imgur.com/J4pNSJh.png)

Parameters changed using the messages are not saved when the patch is loaded, so you have to loadbang messages, or use some other state saving method.

![](http://i.imgur.com/gnT8dAI.png) 

___________
All of the pd files I've included here are within the github repo. If you have any questions, you can find me on Reddit with the username:
```
_megalon_
```

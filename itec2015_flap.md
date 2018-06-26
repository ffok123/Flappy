#Mobile Apps workshop

## Flappy Birds

### Download the Raw Materials

Let’s start up Firefox and download the file `flap.zip`. This file contains all of the raw materials (images and configuration file) required for this workshop.

The downloaded file can be found at the bottom right corner of your screen inside the `Downloads` folder. Let’s click `flap.zip` to extract the raw files (Some machines may have already done it for you).

Next, move the folder to the desktop.

Go to **Launchpad** to find and start up the **Corona Simulator**. Login by using the **loginID** and **password** provided. Click **New Project** to start a new empty project.

In the next page, type in an **application name** (something similar to Flappy Bird). We will use the default Blank project template. For screen size, let’s pick **Phone Preset**, which would give the **iPhone4 dimension**. As we target **iPhone5 or some other longer screen sizes**, let’s modify the **height to `568`**. Some files will then be generated in the project folder.


Next, copy all the files in the **flap folder** to this **project folder**. 

### Text Editor

<img src="https://bungeshea.com/wp-content/uploads/sublime-text.png" style="width:128px;float:left;margin:10px" />

Go to **Launchpad** and start up the **Sublime text** editor (also available in the Windows environment).

Use `File -> Open Folder` to open the project folder. All the files will then be displayed on the left side. Locate and double click the `main.lua` there. For the coming two hours, we will use this text editor to write codes, and
the result will be shown in the **Corona Simulator**.

Let’s Hit **Corona Editor** on the toolbar and select **Run Project**, you see an iOS device which shows nothing. Don’t worry; we are going to add a background real soon. In the Corona Simulator, if the default device is not iPhone 6, please go to **Window**, and then **View As**, and pick **iPhone 6**.

## Setting up the Stage
To add a background, simply use the function `display.newImageRect( )`. 

**Syntax**:
`variable = display.newImageRect( filename [baseDirectory] ,[width] , [height])`

Press `command + S` to save the text file and the Corona simulator will reload. 

~~~lua
background = display.newImageRect("bg568.png", 378, 567) 
background.x = display.contentCenterX
background.y = display.contentCenterY
~~~

`display.contentCenterX` and `display.contentCenterY` will give the values of the center point of the screen.

##Here comes the Bird (Again)

<img src="http://cslinux0.comp.hkbu.edu.hk/~mtchoy/Screen Shot 2016-07-29 at 11.18.58 AM.png" style="float:right;width:40%;margin:10px" />

Using similar technique, let’s add our first character, the bird.

~~~lua
bird = display.newImageRect("char.png", 128, 26) 
bird.x = display.contentCenterX * 0.5
bird.y = display.contentCenterY
~~~



Yes, three birds are displayed. Don’t worry, we will fix it later.

We use a variable `bird` to refer to the bird image. Then, we can use this variable to do more settings when necessary.

In Corona, we use **(x, y) to represent the coordinates**. Notice that the coordinate system used here, as shown below, is different to the Cartesian coordinate system you learn in Math class.

Now, add some animation and let our bird experience the gravity. Let’s add the following code to start **Physics** first:

<!--display.vdispl-->

~~~lua
local physics = require("physics") 
physics.start()
physics.addBody(bird)
~~~
 
We bring **physics** into our game and our bird will show some **physical** behavior (free falling). Now, let’s add a ground to catch the falling bird.

**Syntax**: 
`display.newRect(center x, center y, width, height )`

~~~lua
local ground = display.newRect(display.contentCenterX, 
		display.contentHeight - 30, 2 * display.contentWidth, 60);
physics.addBody(ground);
~~~

Since the ground falls as well, we need to **make the ground static**, which means its position is always fixed. **Change** the last line to

~~~lua
physics.addBody(ground, "static");
~~~

Finally, let’s hide this ground.

~~~lua
ground.isVisible = false;
~~~

##I Believe I Can Fly

<img src="http://cslinux0.comp.hkbu.edu.hk/~mtchoy/Screen Shot 2016-07-29 at 11.21.03 AM" style="float:right;width:40%;margin:10px" />

Now we want to control our bird and let it fly. As previously mentioned, we need
to use an advanced programming technique called **event-driven programming** to do this. In event-driven programming, a piece of code is first developed in a user-defined function. When an event happens, such as the screen being touched, the user-defined function will then be processed. Add the code below:

~~~lua
function birdFlapped(event)
	bird:applyLinearImpulse(0, -0.3, bird.x, bird.y);
end

Runtime:addEventListener( "tap", birdFlapped );
~~~

By using `addEventListener ( eventName, listener )`, we relate the **tap** event with the `birdFlapped()` function. Here, the **EventListener** is applied on the `Runtime` to capture all the taps on the screen.

In physics, we know that force is needed to bring an object from its **non-moving** state to a **moving** state. In Corona, we **apply a force** to our bird by using `applyLinearImpulse()`.

**Syntax**:
`body:applyLinearImpulse( xForce, yForce, bodyX, bodyY )`

The first two parameters control the force applied to the x and y axis respectively. The latter two specify the position where the force is applied.

## The Moving Obstacles

<img src="http://cslinux0.comp.hkbu.edu.hk/~mtchoy/Screen Shot 2016-07-29 at 11.22.17 AM.png" style="width:30%;float:right;margin:10px" />

Again, using similar techniques, let’s add **a pair of vertical pipes** to our game as shown on the right.

~~~lua
function newPipes()
  	pipeTop = display.newImageRect("pipeTop.png", 48, 644);
  	pipeTop.x = display.contentWidth - 30;
  	pipeTop.y = math.random(-250, -40);
  	
	pipeBtm = display.newImageRect("pipeBtm.png", 48, 644); 
	pipeBtm.x = display.contentWidth - 30;
	pipeBtm.y =  pipeTop.y + 644 + 100;
	
	physics.addBody( pipeTop, "static" ); 
	physics.addBody( pipeBtm, "static" );
	
	pipeTop.name = "pipe";
	pipeBtm.name = "pipe";
end

newPipes();
~~~

Note that here we use a **random function** to produce a **random number** within the range of `-250` and `-40`. This number represents the `y` coordinate of the **center of the upper pipe**. In the second line, `644` is the **length of each pipe** while `100` is the **size of the gap between the two pipes**. You may adjust these values yourself. We also give a `name` to these pipes, which helps us to **distinguish objects** during collision detection.

To move these pipes to the left, we can use a new function, `transition.to()`. Add the following two lines at the end of the **newPipes()** function.

~~~lua
transition.to( pipeTop, {time = 10000, x = 30 } ); 
transition.to( pipeBtm, {time = 10000, x = 30 } );
~~~

`time`, clearly is the **expected time** taken (`10000 ms`) to complete this animation while `x` represents the **final destination** of this movement. We can also add a `y` coordinate here too. Now, the pipes stay on the screen even when the animation completes. This is not a good idea as it still consumes some resource. We would like to **remove it completely from our program**. To do so, let’s develop another function named `removePipe()`

~~~lua
 function removePipe(obj)
   if (obj.removeSelf ~= nil) then
        obj:removeSelf();
   end
 end

~~~

This function is really simple, what it does is to remove an object that passed to it. We have to modify the previous two sentences as follows

~~~lua
transition.to( pipeTop, {time = 10000, x = 30, onComplete = removePipe});
transition.to( pipeBtm, {time = 10000, x = 30, onComplete = removePipe});

~~~

Now, when the animation completes, the `removePipe()` function invokes and remove the `pipeTop` and `pipeBtm` images from our program.

Good. Now we need to bring in **more pipes** and they should be generated at **regular intervals**. Corona provides us a very useful **timer function**. Let’s replace the `newPipes()` function call with the following.

~~~lua
bgTimer = timer.performWithDelay( 4500, newPipes, 0 );
~~~

**Syntax**: 
`timer.performWithDelay(delayInMSec, callbackFunction, repeatTimes)`

Let’s see what the parameters mean:

- `delayInMSec` specifies the delay in milliseconds.
- `callbackFunction` is the name of function – `newPipes( )`, which will be executed after the delay.
- `repeatTimes` specifies the number of times that the callback function will be invoked; 0 means we want it to loop forever.

The `timer.performWithDelay( )` function returns an **ID number**, which allows us to manage the timer (e.g., pause, resume, stop, etc.).

The `newPipes()` function will then be called every **4500 ms**.
                              
##Scoring and Collision Detection
Let’s use a variable named `score` to store the score. Put the following lines after the ground was created.

~~~lua
local score = 0;

scoreText = display.newText(score, 
	display.contentCenterX, display.contentHeight - 25, null , 36);

~~~

We setup a text area at the bottom of the screen.

The `display.newText( )` function accepts **five input parameters** – text being displayed, x position, y position, the font type, and finally the font size. Corona will use the system default font if the font type is specified with <i>nil</i>.

**Collision Detection** is very useful in game programming; it allows us to detect whether two objects touch each other. Luckily, in Corona, collision can be detected automatically, and we simply need to provide a function to handle what happens **after** the collision.

~~~lua
function collided ( event )
	if (event.phase == "began" and event.other == ground) then 
		score = score - 5;
	end

	scoreText.text = score;
end

bird:addEventListener("collision", collided)
~~~

Event-driven programming is again used here to handle collision detection. If the bird hits the ground, 5 marks will be deducted. We also have to update the score text by setting the text property of `scoreText`.

We would like to add **one mark** when the bird passes the gap once. We do this also with collision detection. This time, we **build a small rectangle at the gap to detect collision** with the bird. In the bottom of `newPipes()`, let’s add

~~~lua
gap = display.newRect(display.contentWidth - 30, pipeTop.y + pipeTop.height * 0.5 + 50, 5, 100);
physics.addBody(gap, "static"); 
gap.isSensor = true;

gap.name = "gap";
gap.isVisible = false;

transition.to( gap, {time = 10000, x = -30, onComplete = removePipe} );
~~~

`gap` will be our invisible block. **A sensor object** can detect collisions but does not response to them. Let’s modify the `collided` function to award a point when the bird **completely passed through** the **gap** object, thus we use the `ended` phase here.

~~~lua
function collided ( event )
    if (event.phase == "began" and event.other == ground) then 
    	score = score - 5;
	elseif (event.phase == "ended" and event.other.name == "gap") then 
		score = score + 1;
	end
	
	scoreText.text = score;
end
~~~

The `scoreText` and `bird` may appear behind the new pipes. To bring them to the front, we can add the following to the end of the `newPipes()` function.

~~~lua
scoreText:toFront()
bird:toFront()
~~~

## Game Over

How to represent this part is up to you. Here's my design:

~~~lua
function gameOver()
	bird.yScale = -1;
	bird.bodyType = "static";
	
	scoreText.text = "Game Over: " .. score;
	scoreText.y = display.contentCenterY;
	scoreText:setFillColor(128, 0, 0);
end
~~~

the `yScale() = -1` would flip the bird vertically. You can also use this function to resize your image. 

So, let's capture the **Game Over** event. Modify the `collided` function as follows.

~~~lua
function collided ( event )
    if (event.phase == "began" and event.other == ground) then 
    	score = score - 5;
	elseif (event.phase == "ended" and event.other.name == "gap") then 
		score = score + 1;
	elseif (event.phase == "began" and event.other.name == "pipe") then
		Runtime:removeEventListener("tap", birdFlapped);
		timer.performWithDelay(1, gameOver, 1);
	end
	
	scoreText.text = score;
end
~~~


## Flap, Flap and Flap
The game is unplayable if we still keep the three birds there. These three pictures would be combined to produce a flapping animation. Firstly, we need an `ImageSheet`

~~~lua
local birdSheet = graphics.newImageSheet("char.png", 
	{width = 41, height = 24, numFrames = 3, sheetContentWidth = 123, sheetContentHeight = 24});
~~~

Also, a sequence data, telling that we start at the **first frame**, the animation would use **three frames**. The time for each frame is **400 ms**.

~~~lua
local birdSequenceData = { name = "flying", start = 1, count = 3, time = 400};
~~~

Finally, please **comment out** the line that declared the bird 

~~~lua
-- bird = display.newImageRect("char.png", 128, 26)
~~~

And replace it with the following lines.

~~~lua
bird = display.newSprite(birdSheet, birdSequenceData); 
bird:play();

~~~

##Background Music
Put the following codes at the beginning of our code:

~~~lua
bgMusic = audio.loadSound("fightscene.mp3");
 
channel = audio.play(bgMusic, {loops = -1});

~~~

We use `audio.play( )` to play the background music. `loops` allows us to specify the number of times the audio clip will play and `-1` means forever. The returned value of this function call is the **channel number** being assigned.

You may add the following line before the `audio.play(...)` line to adjust the volume of the provided channel:

~~~lua
audio.setVolume(0.3, channel)
~~~

##Bird Sound
Next, we add a bird sound. The bird sound will play when the bird passes through the gap. Put the following line at the upper region of your program

~~~lua
sound = audio.loadSound("crow.wav");
~~~

Then modify the `collided()` function as follows
               
~~~lua               
function collided ( event )
	if (event.phase == "began" and event.other == ground) then
       score = score - 5;
	elseif (event.phase == "ended" and event.other.name == "gap") then
		score = score + 1; 
		audio.play(sound);
	elseif (event.phase == "began" and event.other.name == "pipe") then
		Runtime:removeEventListener("tap", birdFlapped);
		timer.performWithDelay(1, gameOver, 1);
	end

	scoreText.text = score;
end
~~~



~ End ~

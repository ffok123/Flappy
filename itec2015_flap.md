#ITEC2015 iMakeApps




<img src="https://coronalabs.com/wordpress/wp-content/uploads/2016/08/corona_logo.png" style="width:128px;float:left;margin:10px" />



![](http://cslinux0.comp.hkbu.edu.hk/~mtchoy/Screen Shot 2017-09-27 at 5.22.33 PM.png)


### Text Editor





**Syntax**:


~~~lua
background = display.newImageRect("bg568.png", 378, 567) 
background.x = display.contentCenterX
~~~

`display.contentCenterX` and `display.contentCenterY` will give the values of the center point of the screen.

<img src="http://cslinux0.comp.hkbu.edu.hk/~mtchoy/Screen Shot 2016-07-29 at 11.18.58 AM.png" style="float:right;width:40%;margin:10px" />

~~~lua
bird.x = display.contentCenterX * 0.5
~~~






physics.start()
 
We bring **physics** into our game and our bird will show some **physical** behavior (free falling). Now, let’s add a ground to catch the falling bird.

`display.newRect(center x, center y, width, height )`

~~~lua
local ground = display.newRect(display.contentCenterX, 
		display.contentHeight - 30, 2 * display.contentWidth, 60);


~~~lua
~~~


~~~lua
~~~


<img src="http://cslinux0.comp.hkbu.edu.hk/~mtchoy/Screen Shot 2016-07-29 at 11.21.03 AM" style="float:right;width:40%;margin:10px" />


~~~lua
~~~





<img src="http://cslinux0.comp.hkbu.edu.hk/~mtchoy/Screen Shot 2016-07-29 at 11.22.17 AM.png" style="width:30%;float:right;margin:10px" />

Again, using similar techniques, let’s add **a pair of vertical pipes** to our game as shown on the right.

~~~lua
function newPipes()
  	pipeTop.x = display.contentWidth - 30;
  	pipeTop.y = math.random(-250, -40);
  	
	pipeBtm.x = display.contentWidth - 30;
	pipeBtm.y =  pipeTop.y + 644 + 100;
	
	physics.addBody( pipeTop, "static" ); 
	physics.addBody( pipeBtm, "static" );
	
	pipeTop.name = "pipe";
	pipeBtm.name = "pipe";




~~~lua
transition.to( pipeBtm, {time = 10000, x = 30 } );


~~~lua
 function removePipe(obj)
   if (obj.removeSelf ~= nil) then
        obj:removeSelf();
   end
 end
~~~


~~~lua
transition.to( pipeTop, {time = 10000, x = 30, onComplete = removePipe});
transition.to( pipeBtm, {time = 10000, x = 30, onComplete = removePipe});
~~~



~~~lua
~~~

`timer.performWithDelay(delayInMSec, callbackFunction, repeatTimes)`



                              

~~~lua
local score = 0;
	display.contentCenterX, display.contentHeight - 25, null , 36);
~~~




~~~lua
		score = score - 5;
~~~


gap.isSensor = true;


~~~


~~~lua
    	score = score - 5;
		score = score + 1;
	scoreText.text = score;
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
    	score = score - 5;
		score = score + 1;
		Runtime:removeEventListener("tap", birdFlapped);
		timer.performWithDelay(1, gameOver, 1);
	end
	scoreText.text = score;
~~~



~~~lua
local birdSheet = graphics.newImageSheet("char.png", 
	{width = 41, height = 24, numFrames = 3, sheetContentWidth = 123, sheetContentHeight = 24});
~~~


~~~lua
~~~


~~~lua
-- bird = display.newImageRect("char.png", 128, 26)
~~~

bird = display.newSprite(birdSheet, birdSequenceData); 
bird:play();
~~~

~~~lua
bgMusic = audio.loadSound("fightscene.mp3");
 
channel = audio.play(bgMusic, {loops = -1});
~~~


~~~lua
~~~


~~~lua
~~~

~~~lua               
function collided ( event )
		audio.play(sound);
		Runtime:removeEventListener("tap", birdFlapped);
		timer.performWithDelay(1, gameOver, 1);
	end
~~~


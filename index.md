## Team CupCake

The Wanderbot script's main goal was to be coded to handle both fast linear velocity and fast turning. To optimize the speed while making sure we avoid the risk of colliding with other objects, we need to look into handling with camera and acceleration parameters. Knowing the weaknesses of the Laserscan, we know we need to make sharp fast turns to make our robot avoid pursuers

# Wanderbot

### Algorithm

The algorithm for our robot was the simplest part, most of the work came from fine-tuning the topics to go fast and not hit things. Our Algorithm would simply check if an object is within a certain distance (usually a number around less than a meter), if there was it would turn and if there wasn't it would go forward. On Top of that we added a small function that would make sure it would turn “randomly” left or right instead of always turning the same direction

### Camera
The Camera took the most amount of work because it was the main factor in whether or not our fast robot would hit something or not. Besides adding the using the module that converts a depth map to a laserscan, there was two main things we did to make sure our camera would pick up potential obstacles. First we tinkered with the scan<u>_</u>height from the depthimage<u>_</u>to_laserscan to make sure it can properly detect where we wanted it to detect to avoid potential obstacles (the default would often miss objects in certain areas). This took a lot of trial and error to find the value we wanted for scan_height. Lastly, we did a quick math algorithm to calculate the range of the scan we would want to use, narrowing the field of view allowing us to travel closer to obstacles, in order to trip up the follower, but still being wide enough to avoid obstacles. With this range, we trimmed the range topic we got from depthimage_to_laserscan go get the angle we wanted.

### Twists
We needed to smooth the movement topic from our wanderer code to our robot’s velocity to avoid very abrupt and hard movement. To do this, we made a node in-between wanderer and the velocity that would take our values being sent by wander and “smooth” them with a steady acceleration instead of a hard “on/off”. This gave us two main benefits, it looks a lot better and also the camera is less likely to “miss” obstacles with smooth movement compared to hard movement.

### Blind Spot 
Due to the limitations of the camera and the camera’s position on the robot, there was one area that would often appear “invisible” to our robots when traveling around. If the opponent robot is close to the front and a little to the right or the left of our robot, it will not show up on the laserscan. This is a limitation of the hardware’s blindspot that would be very hard to solve without replacing the camera with an actual laserscanner.

### Conclusion
In the end, our competitors didn’t go with laserscan so our solution was no longer fine tuned to our estimation of their strategies. Both opponents went with tracking with visual tracking code instead of laserscan distance finding like we used. This still worked out in our favour because the sharp/fast turns we used to lose the laserscan also ended up losing the visual tracking algorithms very we put against. Against one robot, after the first turn our robot consistently lost the tracking of the opponent and won. Against the other bot, both “runs” our bot did a sharp turn near the opponent bot and then we got in the “blind spot” for the opponents camera, causing the opponent to not see us and go forward “ramming” us, giving us the point. This was an unintentional behaviour, but with how we tuned our bot to avoid obstacles, it makes sense. In the end I think our wanderbot was a great success in regards to the competition. If we had more time, the next thing we would probably look at would be making the bot move more “random” instead of its common behaviour of moving around the perimeter of the room.

# Followbot


Having trouble with Pages? Check out our [documentation](https://help.github.com/categories/github-pages-basics/) or [contact support](https://github.com/contact) and we’ll help you sort it out.

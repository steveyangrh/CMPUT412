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

## Movement

### What we did
The movement was fairly simple, and we did not do much to modify it. We started off with the laserscan followbot uses a PD controller to follow a point in 3D space. This itself is pretty straight forward, and required only some tinkering with variables to set the maximum and minimum distance for a point, and more importantly, to set the distance that we would follow at. We chose to go very close to the bot, about 0.8m, which is close to the minimum usable range of the depth camera. In practice, this worked pretty well, and we could usually retreat from a turtlebot if they were coming too close to see. However, if the turtlebot was quick enough, it could rush inside of our visible range and we would lose it.

Extensions
An obvious extension would be obstacle avoidance. If the opposing turtlebot moved to close to an obstacle, we ran the risk of running into it in a blind chasing of them. In practice, however, we found most bots did not travel close enough to obstacles to pose a risk.
Another possible extension would be to modify the PD controller to try to put us in the same position and heading of the opposing bot once they left, to essentially follow in their footsteps. This could be done by measuring both the position and velocity of the turtlebot, and keeping a buffer of states to strive towards. This, however, seemed like quite an undertaking, so we focussed more on the targeting aspect for improvements (see next section).

### Targeting
What we did 
We started off with the laserscan followbot’s point publisher, which took a laserscan and found a nearby point to follow. Initially this picked the nearest point, which was a good start, but we wanted to have a bit of memory, so we would pick a point near the last point we were following. In this way, we hoped to keep track of a single object consistently.
The way we ended up implementing this was with a list of previous points collected, each of which contributed to the picking of a new point. As each point got older, it’s contribution to picking the new point faded. When a new point was picked, it would have its distance compared to each previous point in our cache, to determine if it was a suitable continuation of our previously followed points.

Performance
It seemed to perform quite well on slow moving objects, and seemingly better than the original, but this is all just based on observation. When moving faster, it was okay until the object it was tracking approached a wall.

Mistakes and Fixes
When we wrote the code to weight each point according to the previous points, we kept all the points in the reference frame of the robot. This means as the robot moved, the points would move with it through the world space, making a fast moving robot move these points substantially before they were too old. The fix would be to use the odometry of the robot to transform each old point based on the new position, making them more accurate. This could possibly fix the problem where we would latch onto a wall after moving quickly towards it, because the turtlebot would be projecting its previous points onto the wall, rather than its target.

Specifics
When picking a point, rather than just taking the minimum distance, we would take the minimum of the distance plus a heuristic, defined by the previous points. The heuristic was defined as a sum of weighted distances from the new candidate point. The weight decreased as the point got older, leaving newer points as more influential, as older points contributed less but not nothing, to counter some noise in measurements. The exact equation would be 
h(x,{x0,x1,...,xn}) = i=0ni(x-xi)2
where x is the candidate point, xi is a previous point, with x0 being the newest point, and xn being the oldest point. Gamma is a weighting factor, which is between 0 and 1. A maximum number of points was set, so this operation is done in constant time based on runtime of the algorithm.

The options for this algorithm were how many previous points to store, and the weighting factor. A larger weighting factor meant older points were more influential, while a smaller one put more weight on the newer points. The number of points stored changed how long a point was used to influence point selection, but mostly existed to make the calculation constant time, since the weighting factor would also decrease the influence of the point exponentially.

### Conclusions
We had some good ideas for a simple yet effective tracking improvement which based its decisions on not only what it was seeing but what it had seen. In retrospect, however, there was an error in the calculation which may have greatly degraded its actual performance with fast moving targets. If we were to do it again, we would probably use a similar approach, but account for our mistake.


Having trouble with Pages? Check out our [documentation](https://help.github.com/categories/github-pages-basics/) or [contact support](https://github.com/contact) and we’ll help you sort it out.

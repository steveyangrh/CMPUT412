## Team CupCake

The Wanderbot script's main goal was to be coded to handle both fast linear velocity and fast turning. To optimize the speed while making sure we avoid the risk of colliding with other objects, we need to look into handling with camera and acceleration parameters. Knowing the weaknesses of the Laserscan, we know we need to make sharp fast turns to make our robot avoid pursuers


### Algorithm

The algorithm for our robot was the simplest part, most of the work came from fine-tuning the topics to go fast and not hit things. Our Algorithm would simply check if an object is within a certain distance (usually a number around less than a meter), if there was it would turn and if there wasn't it would go forward. On Top of that we added a small function that would make sure it would turn “randomly” left or right instead of always turning the same direction

### Camera

The Camera took the most amount of work because it was the main factor in whether or not our fast robot would hit something or not. Besides adding the using the module that converts a depth map to a laserscan, there was two main things we did to make sure our camera would pick up potential obstacles. First we tinkered with the scan<u>_</u>height from the depthimage<u>_</u>to_laserscan to make sure it can properly detect where we wanted it to detect to avoid potential obstacles (the default would often miss objects in certain areas). This took a lot of trial and error to find the value we wanted for scan_height. Lastly, we did a quick math algorithm to calculate the range of the scan we would want to use, narrowing the field of view allowing us to travel closer to obstacles, in order to trip up the follower, but still being wide enough to avoid obstacles. With this range, we trimmed the range topic we got from depthimage_to_laserscan go get the angle we wanted.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://help.github.com/categories/github-pages-basics/) or [contact support](https://github.com/contact) and we’ll help you sort it out.

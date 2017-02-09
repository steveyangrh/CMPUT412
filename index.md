## Team CupCake

The Wanderbot script's main goal was to be coded to handle both fast linear velocity and fast turning. To optimize the speed while making sure we avoid the risk of colliding with other objects, we need to look into handling with camera and acceleration parameters. Knowing the weaknesses of the Laserscan, we know we need to make sharp fast turns to make our robot avoid pursuers


### Algorithm

The algorithm for our robot was the simplest part, most of the work came from fine-tuning the topics to go fast and not hit things. Our Algorithm would simply check if an object is within a certain distance (usually a number around less than a meter), if there was it would turn and if there wasn't it would go forward. On Top of that we added a small function that would make sure it would turn “randomly” left or right instead of always turning the same direction

### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/steveyangrh/CMPUT412/settings). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://help.github.com/categories/github-pages-basics/) or [contact support](https://github.com/contact) and we’ll help you sort it out.

Some key learnings through this process which I was assited by both Kong & Reiyo_oki (thank you both)

When you're trying to create an item that adds a stat such as: Movement Speed

But you don't want the player to be able to just continually add chunks of 20% movement speed until they get to 100%+

We need to be able to limit how the user is able to do this. We can use three fields in the CubeMain.txt to limit this interaction. Below are the fields are some examples of what you could do to limit a player.

# OP 
This field is very important. This allows you to choose the requirements for a Cube Recipe

The full table can be found here: https://d2mods.info/forum/kb/viewarticle?a=284 (I will also copy out the table into another document here for prosperities sake, just in case the site ever goes down)

For the OP field there are two major differences that I would like to highlight:

## player stat 

This refers to the values on a characters skill sheet. If you wanted a crafting recipe to only be available to someone that had enough dexterity to perform this intricate craft, or enough strength to be able to forge the item together (whatever story premise that you may wish) you would choose from one of the OP function which reference player stat.

There are two sets of functions inside of player stat:

### Stat.Accr

This checks the players total values including bonuses from items. Below we see the OP's that check for the players total amount.

So for a player that has 100 strength from investing hard points (earned when leveled up) and 30 strength from their worn gear we would get a total **accr** of 130

This means that these functions would be checking for the value of **130** (in this example)

~~~~~~ 10519 ~~~~~~~~~~ Stat.Accr

OP

 3      player stat(param) < value
 
 4      player stat(param) > value
 
 5      player stat(param) = value
 
 6      player stat(param) ! value
 
 ### Stat.Base
 
 Still checking the players values, this excludes anything that is not a hard point in the players values. So no bonuses that would appear as blue in the player character sheet would added here. Following our previous example, these would be looking for our base of **100**

~~~~~~ 10521 ~~~~~~~~~ Stat.Base

 7      player stat(param) < value
 
 8      player stat(param) > value
 
 9      player stat(param) = value
 
10      player stat(param) ! value

### Stat.Bonus  (accr - base)

This check function is slightly different to the above. It actually takes the total amount from both the characters hard points, as well as the bonus they have and adds them together. Once added, it will subtract the base value, in this case, hard points.

As a calulation we can worth with the following example:

If a player has a base strength of 100 and a bonus of 30 from charms & gear, the total of these two give us our **accr** 

**accr** 130

for these OP's, we need to minus our base. So we'll do our accr (130) - our base (100) leaving us with **30**

This is the value that these parameters will be checking for.

11      player stat(param) < value
12      player stat(param) > value
13      player stat(param) = value
14      player stat(param) ! value


With these, we're able to start definiing when we want a player to be able to utilise a Cube Recipe. Don't worry, we'll have some tutorials on this towards the bottom of this document.







op defines what you are trying to do
param is id from itemstatcost.txt
value is the value of the param (the previous itemstatcost id you typed in)
so if you use:
op 18, param 0, value 100
then you are telling the game that
"Skip this recipe if strength (itemstatcost id 0 from param) is not equal to 100 (value entered)"

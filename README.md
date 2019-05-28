Some key learnings through this process which I was assited by Kong, Reiyo_oki & HarvestWombs (thank you all)

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

## Stat.Accr

This checks the players total values including bonuses from items. Below we see the OP's that check for the players total amount.

So for a player that has 100 strength from investing hard points (earned when leveled up) and 30 strength from their worn gear we would get a total **accr** of 130

This means that these functions would be checking for the value of **130** (in this example)

10519  Stat.Accr

OP

 3      player stat(param) < value
 
 4      player stat(param) > value
 
 5      player stat(param) = value
 
 6      player stat(param) ! value
 
 
 
 ## Stat.Base
 
 Still checking the players values, this excludes anything that is not a hard point in the players values. So no bonuses that would appear as blue in the player character sheet would added here. Following our previous example, these would be looking for our base of **100**

 10521  Stat.Base

 7      player stat(param) < value
 
 8      player stat(param) > value
 
 9      player stat(param) = value
 
10      player stat(param) ! value



## Stat.Bonus  (accr - base)

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



##         Fail Item if ... (item stats)

These are great for recipies that apply to items and used to stop recipes from functioning if they met particular requirements.

If we had a recipe that would give +10 strength to an item, but didn't want to allow it to be used on items that had more than 50 strength, we could use the below to stop that happening. 

I have an example of this later on with making a pair of movement speed boots.

OP's 15 - 18 are Stat.Accr, again meaning Base + Bonus.

 10519  Stat.Accr

15      item stat(param) < value

16      item stat(param) > value

17      item stat(param) = value

18      item stat(param) ! value

OP's 19 - 22 are only looking at the Base value and could perhaps be used for defence, damage and durability (I'm sure more uses are possible)

 10521  Stat.Base

19      item stat(param) < value

20      item stat(param) > value

21      item stat(param) = value

22      item stat(param) ! value

These again calculating accr (which is base + bonus) then - base 

 10522  Stat.Bonus  (accr - base)

23      item stat(param) < value

24      item stat(param) > value

25      item stat(param) = value

26      item stat(param) ! value


With these, we're able to start definiing when we want a player to be able to utilise a Cube Recipe. Don't worry, we'll have some tutorials on this towards the bottom of this document.


-----------------------
### player stat & item stat

Both of these are referencing the **ItemStatCost.txt**  

Don't panic, we'll have more on how to apply these soon.

#param

Once of the most important fields in this txt file. 

The thing you need to know: the **ID** of the stat from the ItemStatCost.txt is placed into this field.

Some examples from the Diablo II Lord of Destruction txt files are found below:

strength	0
energy	1
dexterity	2
vitality	3
statpts	4
newskills	5
hitpoints	6
maxhp	7
mana	8
maxmana	9
stamina	10
maxstamina	11

Just ensure that when you're adding the modified that you want to apply to your item, write down the ID of it to save yourself some time later.

# Value

This is going to be how we definte what we want the OP function to base off of. Lets break this out into a table.


Description | OP | Param | Value
|---|---|---|---|
Boots + Stamina Potion | 16 | 96 | 19 |
Boots + Stamina Potion + Standard Gem | 16 | 96 | 39 |
Boots + Stamina Potion + Perfect Gem | 16 | 96 | 59 |

So, I've chosen **OP 16** lets have a look at why:

**OP 16 item stat(param) > value** - This means that If the item has more movement speed (param 96) than 19 (our value) the recipe won't work.

The next two recipes are the same, but both have an additional item required to make them harder for the player to obtain.

With only these recipes, I'd run into some issues, but it's good to note that the file is able to accept thousands of inputs.

# Key notes:

## OP is taken from the table above, choose what you need.

## Param is taken from **ItemStatCost.txt**

## Value is the number that you want your OP to check against

description | enabled |	version	| op	| param |	value	|	numinputs |	input 1 |	input 2 |	input 3 |	output	| mod1 |	mod 1 chance |	mod 1 min	|mod 1 max|
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
Boots + Stamina Potion -> Movement Speed Boots 20%	|1 | 100	| 16 |	96 |	19	|	2 |	boot |	spot,qty=1 | | useitem |	move1 |	100	|	20 |	20
Boots + Stamina Potion + Standard Gem -> Movement Speed Boots 40% |	1 | 100	| 16 |	96 |	39 | 3 |	boot |	spot,qty=1 |	gem2,qty=1	| useitem | move1 |	100	|	20 |	20
Boots + Stamina Potion -> Movement Speed Boots 60% |	1 | 100 |	16 |	96 |	59	|	3	| boot |	spot,qty=1	| gem4,qty=1	|useitem	|	move1	| 100	|	20	| 20


# D2Data
This is for checking Txt file edits

Added the following lines to CubeMain.txt for better quality of life:
9 Chipped Amethyst -> Standard Amethyst

6 Chipped Amethyst + 1 Flawed Amethyst -> Standard Amethyst	

3 Chipped Amethyst + 2 Flawed  Amethyst-> Standard Amethyst

9 Flawed Amethyst -> Flawless Amethyst

6 Flawed Amethyst + 1 Standard Amethyst -> Flawless Amethyst

3 Flawed Amethyst + 2 Standard Amethyst -> Flawless Amethyst	

9 Standard Amethyst -> Perfect Amethyst	

6 Standard Amethyst + 1 Flawless Amethst -> Pefect  Amethyst

3 Standard Amethyst + 2 Flawless Amethst -> Pefect  Amethyst	

[Note: This is the same for ALL GEMS, including skulls]

Added Tier 3 Movement Speed Boots Recipes

Any boots + Stamina potion = 20% movement speed.

This recipe works all the way up to 19%, adding the 1% required to make it to 20%

Boots + Stamina Potion + Standard Gem -> 40% movement speed boots

Boots + Stamina Potion + Perfect Gem -> 60% movement speed boots

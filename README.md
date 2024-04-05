﻿# Item-Json-Generator
This tool is designed to make creating your own custom items for my personal project/game as straightforward and easy as possible.
All json Objects produced are readable by the json parser in personal project.

## Glossary:

## Item Types: 

### Dropper:
 Droppers produce/mine Ore. They are responsible for setting the base properties(Value, temperature, Multi-Ore) and effects of the ore.
 - **Ore Name:** Name of the ore. 

 - **Ore Value:** This of the value of the ore and is fundamental to the game. When sold this value is added to the players wallet.

 - **Ore Temperature:** The temperature of the ore. 0 represents a neural/default value. Temperature vales that are less
than zero indicates that the ore is cold and values greater than 0 indicate that the ore is warm. Temperature is used to
encourage people to build their base/tycoon in an intentional and strategic way by making them consider the impact that
ore temperature can have on how ore is upgraded/processed. 
   - EX: An upgrader that multiplies ore value by 10x if is its temperature is greater than 100 otherwise it adds 20 to the ore temperature. 
   - EX: An upgrader that multiplies ore value based on the temperature of the ore. 
   - EX: an upgrader that resets an ore if the ore is cold enough.
 - **Multi-Ore:** Allows the player to increase the number of ore without needing to drop more ore.
Functions as a "multiplier" for the number of ore.
   - EX: an ore with a multi-ore of one can be thought of as 1 ore but an ore with a Multi-Ore stat of 3 can be thought 
   of as 3 version/copies of an ore. This property should be treated with care as it can easily break the game. 
 
### Furnace:
Furnaces are responsible for selling ore. When a furnace goes to sell an ore it applies a bonus/upgrade to the ore. It
then calculates the ore's sell price by multiplying the ore value by the Multi-Ore, then adds that value to the players wallet.
After processing a certain number of ore(special point reward threshold)
the furnace will reward a currency called Special Points which can be used to buy special/unique items in the shop or to
purchase upgrades. Multi-Ore influences/impacts this process as well. An ore with a multi-ore stat of 2 represents 2 ores
being processed.
- EX:  If a furnace sells an ore with a Multi-Ore property of 10 the furnace will have effectively processed 10 ore.

### Conveyors:

Conveyors are fairly simple and only do one thing, move ore around the tycoon. The speed at which a conveyor moves ore around 
is determined by its speed stat. A higher speed stat means that ore will travel faster, the base speed is 1.

### Upgraders:

Upgraders are the heart of the game and are rather complex. Upgraders function like conveyors except they tend to take up more space
and they also upgrade the ore. When an upgrader goes to upgrade an ore it first checks the ore's upgrade tag to see if
the ore can be upgraded by it(the upgrader). The number of times an upgrader of that type/name can upgrade an ore is determined by
the maxUpgrades Property of that Upgraders upgrade tag. Each upgrader has a Primary/Main upgrade strategy. 
Upgrader Strategies determine how the upgrader upgrades the ore and are triggered when an upgrader upgrades an ore.

#### Current Upgrade Strategies include:
- **Add, Multiply, Subtract:** These 3 Strategies have a value to modify(VTM) and a modifier. Valid VTMS are Ore value, Ore Temperature, and Multi-Ore.
  - EX: A Multiply upgrade with a modifier of 1.5x and whose value to modify is Value will multiply ore value by 1.5x
  - EX: An add upgrade with a modifier of 20 whose value to modify is temperature will add 20 to ore temperature.
  - EX: A subtract upgrade with a modifier of 1 whose value to modify is will at subtract 1 from Mult-Ore.
- **Bundled:** A bundled upgrade strategy wraps an unlimited number of upgrade strategies into one.
    - EX: If you want to create an upgrade that multiplies ore value and adds to ore Temp you would choose a bundled upgrade.
    You would then select Multiply as your first upgrade and add as your second upgrade. Then you could exit out. 
- **Conditional:** A conditional Upgrade has a condition to evaluate, a comparison type(Greater Than, less than, equal to), 
a threshold(the value which condition to evaluate is compared to) and then takes two upgrade strategies, ifModifier and elseModifier.
The ifModifier is applied if the comparison evaluates to true and the elseModifier is applied if the comparison is false.
You can set the ifModifier and elseModifier to be Bundled upgrade if you want it do multiple things. EX: you want to multiply ore value and add to ore Temp if true.
- **Resetter:** Resets the upgrade tags on an Ore allowing it to be upgraded again by previous upgraders, this is a very
powerful effect. Tagging an upgradeTag as a resetter tells the game not to reset the upgrade tag
when the ore is reset by an upgrader.

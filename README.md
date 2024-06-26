# \[Expression2\] ACF Turret Framework

---

## Overview

An all-in-one turret controller script written in expression2, designed to work with the ACF3 addon.

##### Features:
- Configurable Keybinding.
- Configurable sounds triggered on keybind usage.
- Possibility of various turret setups for all sorts of vehicles, multiple turrets, turrets on turrets, go wild.
- Configurable turret groups, turrets can be assigned to groups which you can swap between.
- Configurable weapon groups, for those vehicles that have a silly amount of guns.
- Configurable ammo groups for each weapon group.
- Fancy EGP HUD included.

---

 This guide will provide you with all the knowledge you need to use the full set of features provided by this script. 

This guide will be presented in multiple steps, as shown below.

1. [Installation](#installation)
2. [The Basics](#the-basics)
3. [Inputs](#inputs)
4. [Keybinds and Sounds](#keybinds-and-sounds)
5. [Functions and Arguments](#Functions-and-Arguments)
6. [Using the Debug functionality](#using-the-debug-functionality)
7. [Calling user functions](#calling-user-functions)
8. ["Building" and "UnBuilding"](#building-and-unbuilding)
9. [Troubleshooting](#troubleshooting)

Instructions for editing libraries bundled with this E2 will not be included, however I can offer advice if you wish to contact me.

There is an FAQ and Troubleshooting guide at the end of this. Refer to that for most commonly encountered issues and how to solve them.

---

## Installation
To 'install' the script, download the latest release from this github page, open the zip file and simply drop the *Expression2_TurretFramework* folder inside of your *garrysmod/data/expression2/* folder. 

Your file structure should look something like this:

```.
garrysmod/data/expression2/Expression2_TurretFramework/examples/...
garrysmod/data/expression2/Expression2_TurretFramework/init.txt
garrysmod/data/expression2/Expression2_TurretFramework/main.txt
garrysmod/data/expression2/Expression2_TurretFramework/config.txt
...
```

*\*Installing the example configs is not required*

If you've done everything right, when you open the **config.txt** inside the in-game Expression2 editor, the script should validate and you should be able to spawn the e2 with the tool. 

If you cannot validate the E2 inside the in-game editor or spawn it without erroring, make sure you have the correct file-structure, otherwise the \#include statements inside of config.txt will reference non existent locations/files.

---

## Getting Started

To get started with actual setup, open the config.txt script in the Expression2 tool, this is the only file which you should have to modify. 

You can set the @name directive of this chip to whatever you like, it has no effect on functionality.

##### Important things to keep in mind

> - Refreshing the chip at any point after ***Build*** has been set to 1 will bug out your entities and likely remove them from existence, *poof*.
> - To edit anything within the chip **after** you have 'built', you must first set ***Build*** to 0, then dupe and paste your entire contraption. You can now edit any settings and refresh the chip as much as you like. When you are done remember to set ***Build*** to 1 to apply all parents.

---

## The Basics

After opening config.txt in your expression 2 editor, navigate to line ~44. The code you are looking for looks like this:

```Lua
    #CONFIG
    #---------------------------------------------------------------------------
    GlobalVariables["DEBUG", number]           = 1

    GlobalVariables["TickInterval", number]    = 4
    #---------------------------------------------------------------------------
```

The first statement determines if the E2 is in debugging mode, when in debugging mode, things like turret/gun axis are shown, more information on debugging mode is provided under the [Debug functionality header](#using-the-debug-functionality).

>When you are done setting up your vehicle, set DEBUG to 0 to not draw the debug holos.
>
>Make sure you do this **before** setting ***Build*** to 1.

The second statement determines how quickly the E2 runs, lower is faster, higher is slower. This value has no impact on turret rotation speeds or responsiveness to keybinds, it will only negatively impact the smoothness of turret movement.

>When using this E2 with high intervals, it is advised to increase the turret "smoothing". This can help counteract some of the consequences of using a high interval.

---

## Inputs

Like any normal E2 chip, we define our inputs in the directives section (@name, @inputs etc.) at the top of the file.

This E2 requires the following inputs to be provided. This is listed as inputs required per vehicle/chip and per turret and gun. Some of these are provided for you.

For more information, click the input names and types listed below to deploy the spoiler.

#### Per Vehicle/Chip:

The only non-standard input here is ***Build***, it is provided for you and usage is extremely simple. Click the spoiler for some more info.

- <details><summary>Vehicle Base - entity</summary>
	
	>	The base prop of your vehicle, this is what will be used to determine the forward orientation of your vehicle.
	>	
	>	Keep that in mind when your turret starts rotating the wrong way or something, ideally you want the base forward vector and turret forward vector to be aligned.
	>	
	>	There is a turret angle offset you can adjust per turret, this is explained later.

</details>

- <details><summary>Seat/Pod - entity</summary>
	
	>	The seat which will control the turrets.
	>	
	>	The chip will use the inputs from the driver of this seat.

</details>

- <details><summary>Build - number</summary>
	
	>	This is used to finalize the vehicle setup, you should set this to 1 when you are happy with the setup and all the debug holos look correct. 
	>	
	>	The easiest method is to wire this to a toggle button and just flick it on when you are satisfied. If you want to change something after you have built, you *MUST* first toggle build off, dupe the whole build and spawn it fresh.

</details>

> The inputs which are required per vehicle/chip are provided for you. 

#### Per Turret:

- <details><summary>Turret Base - entity</summary>
	
	>	This acts as the turret ring, it will rotate on the axis and plane visible in debug mode.
	>	
	>	Depending on your setup, your turret ring props might have 'funky' angles and will require setting an angle offset, this is further explained in the functions and arguments section.

</details>

- <details><summary>Turret Parent - entity</summary>
	
	>	This is what your turret will be attached to, in most cases this will be the same as the entity used for your Vehicle Base
	>	
	>	You can use any valid entity you like here, even world().
	>	
	>	You can attach turrets to other turrets.

</details>

- <details><summary>Turret Gun/Pointer - entity</summary>
	
	>	This is the prop that will act as your gun, it can be your acf gun entity or if you want to use multiple guns on the same axis, you can make this a prop and parent your guns to it.
	>	
	>	When doing this, it is important to orient the pointer prop so that its forward vector is aligned with the guns forward vector. Otherwise your guns aim angle will be offset by the error in orientation (10 degree difference in pitch will make the guns always aim 10 degrees above your aim position, same for yaw)

</details>

> The inputs needed for setting up a single turret with 1 primary gun are also provided for you.

#### Per Gun/Weapon Group:

A seperate input is required for each ACF gun you wish to control with this chip. It is totally fine to completely bypass the weapon group and ammo group functionality of this script and, for example, simply wire your ACF gun 'Fire' input to your pod controller or even use your own script for controlling this functionality.

- <details><summary>Primary ACF Gun - entity</summary>
	
	>	This *must* be an acf gun, otherwise the chip will be calling acf function on a non acf entity, and doing more or less nothing.
	>	
	>	A seperate input is required for every primary gun you wish to have in a given weapon group. 
	>	
	>	Gun inputs can be reused in multiple weapon groups if you want to have the same gun in multiple weapon groups.

</details>

- <details><summary>Secondary ACF Gun - entity</summary>
	
	>	This *must* be an acf gun, otherwise the chip will be calling acf function on a non acf entity, and doing more or less nothing.
	>	
	>	A seperate input is required for every secondary gun you wish to have in a given weapon group. 
	>	
	>	Gun inputs can be reused in multiple weapon groups if you want to have the same gun in multiple weapon groups.
	>	
	>	IMPORTANT: Secondary guns are not affected by ammo group settings and switching, they will load from all attached crates.

</details>

---

## Keybinds and Sounds

We can now start looking at configuring keybinds and sounds, these should be located below the block of code we edited in [The Basics](#the-basics) section.

#### Keybinds

To edit your keybinds, go to line ~60, the code should look something like this:

```Lua
	#KEYBINDS
	#---------------------------------------------------------------------------
	Keybinds["AmmoSwitch", string]          = "1"
	Keybinds["WeaponGroupSwitch", string]   = "2"
	Keybinds["BallisticSwitch", string]     = "3"
	...
	#---------------------------------------------------------------------------

```

Just change the strings initialized in this section to whatever you may want.

Not all keybinds are always required, for example when using a single turret there is no need to bind the turret switch, so you can leave it as an empty string "". More information is present as comments within the code.

>  It's important to note that due to the way expression 2 handles inputs, using the scroll wheel as a keybind can cause the chip's cpu usage to temporarily spike.

#### Sounds

To edit the sounds, go to line ~80, the code should look like this:

```Lua
    #SOUNDS
	#---------------------------------------------------------------------------
    Sounds["AmmoSwitch", string]          = "acf_missiles/fx/bomb_reload.mp3"
    Sounds["WeaponGroupSwitch", string]   = "buttons/button22.wav"
    ...
    #---------------------------------------------------------------------------

```



---

## Functions and Arguments
This section shows a brief description of each function and what its arguments are used for internally, these functions are used for configuring all turret, weapon, ammo and camera behaviour. More information on how to use these functions to achieve the intended setup is available in the [Calling user functions](#calling-user-functions) section. 

###### 

---

## Using the Debug functionality

"Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum."

"Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum."

---

## Calling user functions

"Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum."

"Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum."

---

## "Building" and "UnBuilding"

"Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum."

"Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum."

---

## Troubleshooting

"Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum."

"Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum."

---

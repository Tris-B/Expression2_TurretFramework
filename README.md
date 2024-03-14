# \[Expression2\] ACF Turret Framework

---

 This guide will provide you with all the knowledge you need to use the full set of features provided by this script. 

This guide will be presented in multiple steps, as shown below.

1. Installation
2. Overview
3. Inputs
4. Debug
5. Keybinds
6. Functions and their Arguments
7. Calling user functions
8. "Building" and "UnBuilding"

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

### Inputs

To start, open the config.txt script in the Expression2 tool, this is the only file which you should have to modify. Like any normal E2, we define the name and our inputs in the directives section (@name, @inputs etc.).

This E2 requires the following inputs to be provided. Some of these are provided for you.

Per Vehicle:

- Vehicle Base - entity
<details>
<summary>More Info</summary>
  
```
The base prop of your vehicle, this is what will be used to determine orientations, so keep that in mind when going forward.
```
</details>



- Seat - entity
<details>
<summary>More Info</summary>
  
	<pre>
The seat which will control the turrets.
</details>

- Build - number


> The inputs which are required per vehicle are provided for you. 

Per Turret:

- Turret Base - entity
This acts as your turret ring

- Turret Parent - entity
For most vehicles, this is your Vehicle Base. You can set this to be the base of another turret for example, to have a turret on a turret.

- Turret Gun/Pointer - entity
This will usually be your gun. However, for turrets with multiple barrels, you can use any entity and just parent your guns to that entity to make them rotate with it.

> The inputs needed for setting up a single turret with 1 primary gun are also provided for you.

---

### The Basics

Now that you know all about inputs, navigate to line ~44, depending on how you have setup your inputs, the code may be on a slightly different lines. The code you are looking for looks like this:

```Lua
    #CONFIG
    #---------------------------------------------------------------------------
    GlobalVariables["DEBUG", number]           = 1

    GlobalVariables["TickInterval", number]    = 4
    #---------------------------------------------------------------------------
```

The first statement determines if the E2 is in debugging mode, when in debugging mode, things like turret/gun axis are shown, more information on debugging mode is provided under the Debug Mode header.

>When you are done setting up your vehicle, set DEBUG to 0 to not draw the debug holos.

The second statement determines how quickly the E2 runs, lower is faster, higher is slower. This value has no impact on turret rotation speeds or responsiveness to keybinds, it will only negatively impact the smoothness of turret movement.

>When using this E2 with high intervals, it is advised to increase the turret "smoothing". This can help counteract some of the consequences of using a high interval.

---

### Keybinding

We can now start looking at keybinds, these should be located below the block of code we edited in the previous section, around line ~60, with the code looking as follows:

```Lua
	#KEYBINDS
	#---------------------------------------------------------------------------
	Keybinds["AmmoSwitch", string]          = "1"
    Keybinds["WeaponGroupSwitch", string]   = "2"
    Keybinds["BallisticSwitch", string]     = "3"
    ...
	#---------------------------------------------------------------------------

```

Not all keybinds are always required, more information is present as comments within the code.

To edit your keybinds, just edit the strings initialized in this section to whatever you may want.

---

### Troubleshooting

---



---
 
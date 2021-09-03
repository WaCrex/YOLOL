# NavList
### Requirements
|Component|Instructions|
|---|---|
| 2x Buttons | One named **:BD** for decreasing the ID by 1 & one named **:BU** for increasing the ID by 1. Button style must be set to **1**.|
| 1x progress bar | Named **:DI** for storing the destination ID as an INTEGER instead of a STRING, set the minimum to **0** and the maximum to **9999**. |
| 1x Basic YOLOL Chip | For the Core Script, and one additional one for each Extension added. Each chip can hold a maximum of 18 destinations for a maximum of 10.000 destinations. |
| 1x progress bar or 1x text panel | (Optional) For showing the name of the Destination, must be named **:DEST**. |

**Note:** *You only need the two buttons and Core script if you want to manually interact with the NavList. If you are planning on using the NavList as a database for a different YOLOL Script only, then you will only need a Basic YOLOL chip with the **Extension** on it.*

### Global Variables
| Global | Description | Used By |
|:------:| ----------- | ----------- |
| :DX | The X coordinate of the destination. | Core & Extension |
| :DY | The Y coordinate of the destination. | Core & Extension |
| :DZ | The Z coordinate of the destination. | Core & Extension |
| :DI | The ID of the destination. | Core & Extension |
| :DEST | The NAME of the destination. | Core & Extension |
| :BD | Input Button, decrease ID by one. | Core Only |
| :BU | Input Button, increase ID by one. | Core Only |

## NavList - Core
Handles Button input and stores the destinations 0-17.

### How it works
The script will stay on line 2 and handle button input and wait until the ID is within the range 0-E, it will then go to the line where the destination is stored and read the data after which it will jump to line 1 and store it globally before returning to line 2.
```c
//Note I is the internal name for the Destination ID :DI, as the core is "broadcasting" the ID we only need to read the ID once at the initialization.
I+=:BU*(I<M)-:BD*(I>0) :BD=0 :BU=0 :DI=I //Increase I by 1 if :BU==1, otherwize decrease I by 1 if :BD==1, finally reset :BD & :BU and store I as :DI.
goto2+((I+1)*(0<=I)*(I<=E)) //Stay on line 2 until I is between 0 and E, if so go to that line instead.
```

###  Options
| Option | Min | Max | Description |
| ------ |:---:|:---:| ----------- |
| M | 0 | 9999 | The maximum allowed Destination ID, prevents the user from going out of range. Needs to be changed each time you add/remove a destination. |
| E | 0 | 17 | The last Destination ID listed on this chip. Needs to be changed if you have less then 18 destinations, should be set to destination_count - 1.

### Example Setup
**Setup:** *Core + Extension, 30 Destinations in total.*

**Note:** *Note this only covers the **Core** part of the setup.*
```c
:DX=X :DY=Y :DZ=Z :DEST=N I=:DI M=0029 E=0017//github.com/WaCrex/YOLOL
I+=:BU*(I<M)-:BD*(I>0) :BD=0 :BU=0 :DI=I goto2+((I+1)*(0<=I)*(I<=E))
X=-1514 Y=-58557 Z=14605 N="Origin 1" goto1     //ID#000 - Origin 1
X=-12356 Y=-53133 Z=9482 N="Origin 2" goto1     //ID#001 - Origin 2
X=-21163 Y=-48710 Z=9735 N="Origin 3" goto1     //ID#002 - Origin 3
X=-30412 Y=-39176 Z=4620 N="Origin 4" goto1     //ID#003 - Origin 4
X=-35894 Y=-28937 Z=-569 N="Origin 5" goto1     //ID#004 - Origin 5
X=-40449 Y=-19201 Z=-5273 N="Origin 6" goto1    //ID#005 - Origin 6
X=-45639 Y=-9224 Z=-5593 N="Origin 7" goto1     //ID#006 - Origin 7
X=-45676 Y=511 Z=-5543 N="Origin 8" goto1       //ID#007 - Origin 8
X=-45810 Y=10676 Z=-5501 N="Origin 9" goto1     //ID#008 - Origin 9
X=-41736 Y=22001 Z=-5152 N="Origin 10" goto1    //ID#009 - Origin 10
X=-37458 Y=30822 Z=-743 N="Origin 11" goto1     //ID#010 - Origin 11
X=-32408 Y=40743 Z=4281 N="Origin 12" goto1     //ID#011 - Origin 12
X=-22508 Y=50948 Z=9390 N="Origin 13" goto1     //ID#012 - Origin 13
X=-12627 Y=56072 Z=9548 N="Origin 14" goto1     //ID#013 - Origin 14
X=-2299 Y=61232 Z=14458 N="Origin 15" goto1     //ID#014 - Origin 15
X=7554 Y=61302 Z=14522 N="Origin 16" goto1      //ID#015 - Origin 16
X=17836 Y=56855 Z=10092 N="Origin 17" goto1     //ID#016 - Origin 17
X=28872 Y=51332 Z=9665 N="Origin 18" goto1      //ID#017 - Origin 18
```

## NavList - Extension
Extends the distination list by an another 18 slots, used for the destinations 18-9999.

### How it works
The script will stay on line 2 until the ID is within the range set by S & E, it will then go to the line where the destination is stored and read the data after which it will jump to line 1 and store it globally before returning to line 2.
```c
I=:DI //Read the globally stored destination ID :DI and store it internally as I.
goto2+((I+1-S)*(S<=I)*(I<=E)) //Stay on line 2 until I is between 0 and E, if so go to that line instead.
```

### Options
| Option | Min | Max | Description |
| ------ |:---:|:---:| ----------- |
| S | The first destination ID listed on this chip. |
| E | The last destination ID listed on this chip. Needs to be changed if you have less than 18 destinations on this chip. |

### Example Setup
**Setup:** *Core + Extension, 30 Destinations in total.*

**Note:** *Note this only covers the **Extension** part of the setup.*
```c
:DX=X :DY=Y :DZ=Z :DEST=N S=0018 E=0029//github.com/WaCrex/YOLOL E=END
I=:DI goto2+((I+1-S)*(S<=I)*(I<=E))//Destination ID Extension  S=START
X=38334 Y=42004 Z=4963 N="Origin 19" goto1      //ID#018 - Origin 19
X=43363 Y=32331 Z=81 N="Origin 20" goto1        //ID#019 - Origin 20
X=48612 Y=22216 Z=-5050 N="Origin 21" goto1     //ID#020 - Origin 21
X=53755 Y=11921 Z=-5458 N="Origin 22" goto1     //ID#021 - Origin 22
X=53982 Y=1680 Z=-5390 N="Origin 23" goto1      //ID#022 - Origin 23
X=53948 Y=-8049 Z=-5410 N="Origin 24" goto1     //ID#023 - Origin 24
X=48380 Y=-18037 Z=-5330 N="Origin 25" goto1    //ID#024 - Origin 25
X=44005 Y=-28331 Z=-466 N="Origin 26" goto1     //ID#025 - Origin 26
X=38773 Y=-38244 Z=4672 N="Origin 27" goto1     //ID#026 - Origin 27
X=28909 Y=-48311 Z=9594 N="Origin 28" goto1     //ID#027 - Origin 28
X=18452 Y=-53450 Z=9642 N="Origin 29" goto1     //ID#028 - Origin 29
X=8356 Y=-58735 Z=15266 N="Origin 30" goto1     //ID#029 - Origin 30
X=0 Y=0 Z=0 N="" goto1                          //ID#030 - Not In Use
X=0 Y=0 Z=0 N="" goto1                          //ID#031 - Not In Use
X=0 Y=0 Z=0 N="" goto1                          //ID#032 - Not In Use
X=0 Y=0 Z=0 N="" goto1                          //ID#033 - Not In Use
X=0 Y=0 Z=0 N="" goto1                          //ID#034 - Not In Use
X=0 Y=0 Z=0 N="" goto1                          //ID#035 - Not In Use
```

## TODO
* *(Improvement) Find a way to "squeeze in" I=:DI at the start of line 2 in the Core script, this should allow manually entering the destination ID into :DI and thus allow the user to jump directy to an destination ID.*

## Credits
| Name | GitHub | Description |
| ------ | ------ | ----------- |
| Patrick Lineruth | WaCrex | The Author of these two scripts. |
| | Syb80 | For writing navi_notebook.yolol and giving me the inspiration to create these two scripts. It was a nice challenge and I like challenges :) |

**Syb80's version (navi_notebook.yolol):**
```c
 x=-21756 y=-48358 z=9615 name="Origin 3"
// x=-21000 y=-48000 z=9000 name="None" // your comment here
// x=-21000 y=-48000 z=9000 name="None"
// x=-21000 y=-48000 z=9000 name="None"
// x=-21000 y=-48000 z=9000 name="None"
// x=-21000 y=-48000 z=9000 name="None"
// x=-21000 y=-48000 z=9000 name="None"
// x=-21000 y=-48000 z=9000 name="None"
// x=-21000 y=-48000 z=9000 name="None"
// x=-21000 y=-48000 z=9000 name="None"
                   :DX=x :DY=y :DZ=z :TN=name goto 1 //  comments -> |
// ==== Captain's notebook ====
// Store waypoints here. Uncomment one line at once
// NAME = 12 chars max
```

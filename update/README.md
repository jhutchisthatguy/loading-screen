> [!WARNING]
> This is no longer needed! I included all the loading screen files in the earlier versions of OGL, but they were somewhat unnecessary, so I’ve switched to using webhost configurations. I’m keeping these gmad files here for reference, as they provide a tutorial on how to modify and compile them.

To upload and update a loading screen addon, we actually need a custom GMad build with a wider files whitelist and SteamCMD to workaround gmpublish restrictions.

As it's a complicated process, I decided to write these instructions down. I hope to help other people to achieve the same result as mine.

# Compile a custom GMad

If you don't want to create your own binary, I included mine in the "update/bin" folder, so ignore this section entirely.

## To compile GMad:

### Common steps:

1. Clone gmad into the update folder (https://github.com/Facepunch/gmad.git)
1. Clone bootil inside gmad (https://github.com/garrynewman/bootil)
1. Add the following lines to ``gmad/include/AddonWhiteList.h``:
```lua
			"html/*.html",
			"html/*.wav",
			"html/*.png",
			"html/icons/*.png",
```

### On Windows (VS 2017)

1. Copy premake5.lua from this repository https://github.com/AnthonyFuller/gmad and place it next to premake4.lua
1. Change all #include "lib.h" to #include "../include/lib.h" in GMad's src folder files
1. Open a terminal and change the directory to "gmad"
1. Create the solution files running .\premake5 vs2017
1. In VS2017, retarget the solution to the current windows sdk (Right click on the solution and redirect solution)
1. Compile selecting Release
1. The binary will be placed inside the bin folder
		
### On Linux (Ubuntu)

1. sudo apt install premake4
1. open the terminal and cd to gmad/bootil/projects
1. premake4 gmake
1. cd linux/gmake
1. make bootil_static # (The file gmad/bootil/lib/linux/gmake/libbootil_static.a will be compiled)
1. change the directory back to gmad
1. premake4 --outdir="bin/" --bootil_lib="bootil/lib/linux/gmake" --bootil_inc="bootil/include" gmake
1. make GMad
1. The binary will be placed inside the bin folder

# Creating the gma

1. In the terminal, change the directory to our update/bin folder (If you're going to use my binaries)
1. Create the gma with normal gmad commands. E.g.:
```
.\gmad.exe create -folder "E:\Games\SteamLibrary\steamapps\common\GarrysMod\garrysmod\addons\loadingscreen" -out "D:\somefolder\loadingscreen.gma"
```

# Uploading the addon with SteamCMD

1. Download SteamCMD from https://developer.valvesoftware.com/wiki/SteamCMD
1. Open SteamCMD
1. Login with ``login username password``
1. Edit our ``update/files/update_ogl.vdf`` file with all the changes and correct directories
1. Upload or update the addon with ``workshop_build_item update_ogl.vdf`` (change the path to update_ogl.vdf)
1. Run ``quit``

# Hosting the screen

I'd recommend to host the screen on GitHub pages, but I actually don't know how it works or if they impose some random limits... So I simply keep my files from the 'host' folder in my Google Drive and serve them with DriveToWeb: https://www.drv.tw/. The website has been online for years now and I think it's one of the easiest ways to host a website.

# Making your own changes

You may want to modify the loading screen to do better tricks and have a nicer style, but beware!! The HTML/CSS/JS in it is weird and old for a reason!! As GMod uses a very old version of Awesomium, we also need to keep going with the OG web style. If you break the page with new JS features and fancy CSS, it's going to be a pain in the ass to debug it!! Do small changes, apply them and immediately test in-game. You've been warned.

That's it! Enjoy your loading screen!
- Xalalau
## Welcome to 9810 AOSP Building guide!
## All of the Instructions lines are from my experience while building AOSP for 9810 devices.
   If I messed something up below, please send me a message with what is to fix.

1. Requirements
To build AOSP on 9810 devices, you must prepare:
+ A Linux environment (Ubuntu 18.04 or newer is recommended!).
+ A 4-cores CPU and 16GB RAM or higher.
+ A good network! (Because you will need to sync 100GB+ of sources)
+ 300GB or more storage. (256GB can also work, but more complicated)S
+ Basic knowledge about git.

2.Steps
+ Setting up building environment:
  - First, you will need the building environment before building. Luckily, we have Akhilnarang's scripts, which is more easier for us to setting up.
  - We need to clone his scripts on Github by doing this:
     ```bash
     git clone https://github.com/akhilnarang/scripts
     ```
 
  - This should copy all the scripts into /home/username/scripts
  
  - Then we need to navigate into that folder by doing this:
     ```bash
     cd scripts/setup
     ```
  - Then we need to lists out the directory:
     ```bash
     ls
     ```

  - Find the file that corresponds to our Linux build. Since we are using Ubuntu it will be android_build_env.sh.
    For other Distros refer to the readme that has also been cloned.
 
  - Run the scripts: 
    ```bash
     . android_build_env.sh
    ```

+ After all of the scripts are synced, we will need to setup Github by doing this:
    ```bash
     git config --global user.name "your name"
     git config --global user.email "youremail@example"
    ```

+ Syncing the sources
  - This is one of the worse step, you have to wait for a long time, depends on your network xD
  - Check the ROM you want to build, then we will make a directory for the sources
    (at here, I'll take PixelExperience for an example, you can change the name of the directory as you like):
    ```bash
     mkdir "ROMNAME"
      (example: mkdir pexp)
    ```

  - Then we will navigate to that directory:
    ```bash
     cd "ROMNAME" 
     (example: cd pexp)
    ```

  - You will have to check the command line to sync all the sources by looking on their Github.
    Usually The repository initiation command can be found on the GitHub page in manifest repo,
    then scroll down to repo initialisation and copy command.
  
  - It should look like this: 
    ```bash
     repo init -u https://github.com/crdroidandroid/android.git -b 13.0 --depth 1
    ```

+ Cloning devices trees:
  - For 9810: you may have a look into our organization for all trees at here: https://github.com/SamsungExynos9810
  - Tip: https://github.com/SamsungExynos9810/local_manifests (All trees that we used to build is at here)
 ```bash
    while in rom folder do
    git clone https://github.com/9810s/local_manifests .repo/local_manifests
 ```

  - Then download all the sources:
    ```bash
     repo sync -c -j$(nproc --all) --force-sync --no-clone-bundle --no-tags
    ```

  - Now wait!

+ Modifying the trees for ROMs: (romfolder/device/samsung/starlte, star2lte and crownlte)
  - There are 3 files that you need to edit here: romname_device.mk, romname.dependencies, AndroidProducts.mk.
  	- In the trees youre basically syncing with this tutorial, you can build lineageOS, crDroid and roms that use lineage.dependencies. 
	
	
  [WE TAKE STAR2LTE AS AN EXAMPLE HERE!!!]
  - If you are using a rom like sparkOS, you have to edit a few things: 
  	- lineage_star2lte > spark_star2lte... i think you get me. anything with lineage in name will be spark for sparkOS
	- then we also have to edit romname_devicee.mk:
	EXAMPLE: ```bash
	 nano spark_star2lte.mk
	 ```
	 - that will open a basic text editor in the terminal.
	 there you edit stuff, that contain "lineage" to "spark"
	 - then we nano into AndroidProducts.mk
		-there we change "lineage" to "spark" as well.
		
	- you do that for star, star2 and crownlte.

3. Build
    ```bash
   # Set up environment
     . build/envsetup.sh

   # Choose a target
     lunch romname_$device-userdebug

   # Build the code
     mka bacon  
   (check ROM manifest on github for exact commands)      
    ```

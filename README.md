# COSP #

## Introduction ##
These instructions will hopefully assist you to start with a stock Android device, and then download the required tools as well as the very latest source code for COSP (based on Google’s Android operating system) for your device. Using these, you can build COSP and a Recovery image from source code, and then install them both to your device.

It is difficult to say how much experience is necessary to follow these instructions. While this guide is certainly not for the very very very uninitiated, these steps shouldn’t require a PhD in software development either. Some readers will have no difficulty and breeze through the steps easily. Others may struggle over the most basic operation. Because people’s experiences, backgrounds, and intuitions differ, it may be a good idea to read through just to ascertain whether you feel comfortable or are getting over your head.

Remember, you assume all risk of trying this, but you will reap the rewards! It’s pretty satisfying to boot into a fresh operating system you baked at home :). And once you’re an Android-building ninja, there will be no more need to wait for updates from anyone. You will have at your fingertips the skills to build a full operating system from code to a running device, whenever you want. Where you go from there– maybe you’ll add a feature, fix a bug, add a translation, or use what you’ve learned to build a new app or port to a new device– or maybe you’ll never build again– it’s all really up to you.


## What you'll need ##
* An Android device with an unlocked bootloader
* A relatively recent 64-bit computer (Linux(preferably Ubuntu) or macOS) with a reasonable amount of RAM and about 100 GB of free storage (more if you enable `ccache` or build for multiple devices). The less RAM you have, the longer the build will take (aim for 8 GB or more). Using SSDs results in considerably faster build times than traditional hard drives.
* A decent internet connection and reliable electricity :)
* Some familiarity with basic Android operation and terminology. It would help if you’ve installed custom roms on other devices and are familiar with recovery. It may also be useful to know some basic command line concepts such as cd, which stands for “change directory”, the concept of directory hierarchies, and that in Linux they are separated by /, etc.

Let's begin!

## Install the build packages ##
Several packages are needed to build COSP. You can install these using your distribution’s package manager.

To build COSP, you'll need:
* ```bc bison build-essential ccache curl flex g++-multilib gcc-multilib git gnupg gperf imagemagick lib32ncurses5-dev lib32readline-dev lib32z1-dev liblz4-tool libncurses5-dev libsdl1.2-dev libssl-dev libwxgtk3.0-dev libxml2 libxml2-utils lzop pngcrush rsync schedtool squashfs-tools xsltproc zip zlib1g-dev```

For Ubuntu versions older than 16.04 (xenial), substitute:
* ```libwxgtk3.0-dev → libwxgtk2.8-dev```

Java
* OpenJDK 1.8 (install `openjdk-8-jdk`)

## Create the directories ##
You’ll need to set up some directories in your build environment.

To create them:
```
mkdir -p ~/bin
mkdir -p ~/android/cosp
```

The ~/bin directory will contain the git-repo tool (commonly named “repo”) and the ~/android/cosp directory will contain the source code of COSP.

## Install the `repo` command ##
Enter the following to download the repo binary and make it executable (runnable):
```
curl https://storage.googleapis.com/git-repo-downloads/repo > ~/bin/repo
chmod a+x ~/bin/repo
```

## Put the `~/bin` directory in your path of execution ##
n recent versions of Ubuntu, `~/bin` should already be in your PATH. You can check this by opening `~/.profile` with a text editor and verifying the following code exists (add it if it is missing):
```
# set PATH so it includes user's private bin if it exists
if [ -d "$HOME/bin" ] ; then
    PATH="$HOME/bin:$PATH"
fi
```

Then, run `source ~/.profile` to update your environment.

## Initialize the COSP source repository ##

Enter the following to initialize the repository:
```
cd ~/android/cosp
repo init -u https://github.com/cosp-project/manifest -b pie
```


### Download the source code ###
To start the download of the source code to your computer, type the following:
```
repo sync -c -j4 --force-sync --no-clone-bundle --no-tags
```
**NOTE**: This may take a while, depending on your internet speed. Go and have a coffee/tea/nap in the meantime!


## Prepare the device-specific code ##
After the source downloads, ensure you’re in the root of the source code (`cd ~/android/cosp`), then type:
```
source build/envsetup.sh
breakfast <devicename>
```

This will download your device’s device-specific configuration and kernel.


## Turn on caching to speed up build ##
Make use of [ccache](https://ccache.samba.org/) if you want to speed up subsequent builds by running:
```
export USE_CCACHE=1
```

and adding that line to your `~/.bashrc file`. Then, specify the maximum amount of disk space you want ccache to use by typing this:
```
ccache -M 50G
```

where 50G corresponds to 50GB of cache. This needs to be run once. Anywhere from 25GB-100GB will result in very noticeably increased build speeds (for instance, a typical 1hr build time can be reduced to 20min). If you’re only building for one device, 25GB-50GB is fine. If you plan to build for several devices that do not share the same kernel source, aim for 75GB-100GB. This space will be permanently occupied on your drive, so take this into consideration. See more information about ccache on Google’s [Android build environment initialization page](https://source.android.com/source/initializing.html#setting-up-ccache).

You can also enable the optional ccache compression. While this may involve a slight performance slowdown, it increases the number of files that fit in the cache. To enable it, run:
```
export CCACHE_COMPRESS=1
```
or add that line to your `~/.bashrc` file.

## Start the build ##

Time to start building! Now, type:
```
croot
brunch cheeseburger
```

The build should begin.

## Installing the build ##

Assuming the build completed without errors (it will be obvious when it finishes), type the following in the terminal window the build ran in:
```
cd $OUT
```

There you’ll find all the files that were created. The two files of interest are:
1. `recovery.img`
2. `COSP-<yymmdd>-UNOFFICIAL-<devicename>.zip`

## Success! So… what’s next? ##
You’ve done it! Welcome to the elite club of self-builders. You’ve built your operating system from scratch, from the ground up. You are the master/mistress of your domain… and hopefully you’ve learned a bit on the way and had some fun too.

## To get assistance ##
Contact [@SphericalKat](https://t.me/sphericalkat) on Telegram. (That's me :D)

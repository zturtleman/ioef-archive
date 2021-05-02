README for StarTrek Voyager - Elite Force engine release
   - last updated 18.05.2006

This project is a modification to the icculus.org maintained quake3 engine
to make it possible to run EliteForce holomatch. This means: you can *not*
play single player missions with this project.
This engine has full compatibility for the newer _and_ the original
EliteForce protocol, clients using the original engine can use
newer servers, and clients using my engine can use old servers, too.

Now, the next few paragraphs are just some random blabla about the
advantages of this release, but if you want to you can just skip it and go
right to the installation instructions.

What was the motivation for me doing this project?
It actually started pretty small.. The Quake3 source has been released more
than half a year ago and I got the crazy idea to try the quake3 engine with
EliteForce on my trusted Gentoo Linux.
When I started the program first the screen went black and nothing
happened... nearly nothing. I heard some sounds and when I moved the cursor
I heard random click-clicks from the mouse going over some menu buttons.
This meant this stuff is not completely incompatible after all, it didn't
crash right away though that was exactly what I expected it to do.
I digged in Raven's multiplayer source that they released (only the virtual
machine stuff, to allow for building modifications) and compared it to
quake3's virtual machines and realized that there was a small
incompatibility in one of the data structures used to pass information
between engine and virtual machine. It was nothing big and after fixing this
the menu worked!
There were some quirks here or there, but most of the menu really worked.
So it got me thinking: If I fix all these incompatibilities.. maybe I can
finally play my beloved multiplayer game I've been clinging to for over 5
years on my favorite OS natively.. and IPv6 support for EliteForce would be
pretty cool too...
So a crazy idea was turned into a serious project. I had to add support for
Raven's models, playing mp3s, EliteForce's own network protocol. My goal was
to make this release as close to the original as possible. Most of the time,
I only could guess what names and functions would mean to the engine or I
had to compare the original game and then try to make it work the same way
in my engine.

The quake3 game relies heavily on floating point operations. Unfortunately,
the original EliteForce engine does non-ISO compliant rounding of floating
point numbers to integers in the VMs.
The game VM uses that flaw which will result in higher jumping for certain
com_maxfps settings. As with the new engine there are many platforms that
can be supported, there are probably many different ways one has to take to
revert the rounding to the old behaviour. This is nearly impossible to do,
so I had to use a different approach: build new VMs.

############################
### GENERAL INSTRUCTIONS ###
############################

All player and server admins, no matter whether old or new EliteForce engine
should download the pak9.pk3 from
http://thilo.kickchat.com/efport-progress/bin/vm/
and copy the pak file to their baseEF directories.
Dedicated servers using my engine will yield a greatly impaired movement if
this pak file is not installed.
This pak only fixes a few obvious bugs but otherwise doesn't change gameplay
at all.
Modders please read information about this further below.

#########################################
### WINDOWS INSTALLATION INSTRUCTIONS ###
#########################################

Installing this on windows is pretty straightforward.
If you don't have it already on your harddrive, you can install EliteForce
using the normal installer. You must also install either the expansion pack
or the official patch to upgrade your installation to version 1.2.

Once you have done all this, you can just copy the engine from this project
to your main eliteforce installation directory where Raven's original
stvoyHM.exe file resides.
You can get the new engine here:
http://thilo.kickchat.com/efport-progress/bin/win32/

You may have also noticed an OpenAL installer in the same directory. OpenAL
is a portable library that provides 3d sound functions. This one is even EAX
enabled (though I'm not sure whether the engine actually makes of the EAX
extensions).
If you want to, you can install OpenAL on your computer too, but it's not
required.
Keep in mind, that OpenAL support is still somewhat experimental.
Sound quality can be different, unusual or even be worse with OpenAL. If you
don't like it, you can always turn off OpenAL support by setting the
s_useOpenAL cvar to 0.

#######################################
### LINUX INSTALLATION INSTRUCTIONS ###
#######################################

Prerequisites:
 * A working hardware accelerated OpenGL setup (DRI or fireglx or nvidia-glx...)
 * libSDL - SDL libraries
 * libmad - (MPEG Audio Decoder, also known as MAD sometimes.)
Optional:
 * OpenAL - OpenAL sound libraries

You can fullfill these dependencies by installing the correct packages from
your favorite distribution's packet manager.

Create a new eliteforce directory, you will probably want to use a dir like:
/usr/local/games/eliteforce

Copy a build of this engine that you can get from
http://thilo.kickchat.com/efport-progress/bin/
into the new directory.
Also copy any directories from MODs you installed previously on windows to
this directory.

Create a subdirectory named "baseEF" (case sensitive!!) and copy the
pak0.pk3, pak1.pk3 and pak2.pk3 from the original eliteforce
release in there.
pak1.pk3 and pak2.pk3 are files that have been released after the game went
gold. You must either get them by installing the latest official EF patch on
windows or you can get them from the expansion pack cd.
pak3.pk3 can only be found on the expansion pack CD. If you want to have the
maps and additions from the expension pack, you must copy this file as well.

If you have an old hmconfig.cfg from windows, don't add it to baseef but to
your home directory in ~/.stvef/baseEF/
Speaking of that dir, the hidden directory where user specific config
files/maps/mods will be stored is - as you may have guessed by now -
.stvef in your home directory.

########################
### For MOD creators ###
########################

I spoke about modifications necessary to get the old movement.
Unfortunately, you will have to do these modifications to your mods too if
you want your mod to run on my dedicated server version and don't want the
users to get too frustrated over slowness.

The problem is in the SnapVector macro located in q_shared.h:
It uses casting a float to int using the (int) cast. The ISO standard
requires the decimal places to be cut off and the number truncated. Thus,
3,4 is converted to 3 or 5,9 is converted to 5.
This is what happens in my engine.
In EliteForce though, a 5,9 will be converted to 6 which is not standard
compliant and will ultimately result in faster movement. This - among other
factors - is the cause for framerate dependent jumping out of tranches, etc.
What one could see as flaw many players now have learnt to rely upon.

I have started a codebase where all modders can start from that fixes some
known bugs and the movement so that everything works like in the original
EF. Even if you have already changed much code, there is a way you can
quickly insert the changes into your MOD, too. I created sourcecode patches
that take care of the bugs.

At this time, there are already a couple of bugfixes that have been added to
the codebase. For instance, bots can now join password protected servers,
the demo selection menu now has scrollbars (thanks to TiM). A more complete
list can be found here:
http://svn.kickchat.com/index.cgi/stvoy/trunk/ChangeLog?view=log

The patch can be found here:
http://thilo.kickchat.com/efport-progress/patches/stvoyVM-bugfixes-beta3.diff
You can also check out my source code on
http://svn.kickchat.com/index.cgi/stvoy/trunk/

The patch can be applied using the "patch" tool under Linux. For Windows,
I recommend that you install TortoiseSVN which is an excellent tool for
accessing subversion projects. It has also got the ability to create and
merge patches:
http://tortoisesvn.tigris.org/

Quick instructions for Windows users to integrate my improvements into your
mod:

- Check out the repository at revision 1:
  svn co svn://kickchat.com/stvoy/trunk stvoy
- Copy and overwrite all files in that newly checked out directory.
  Make sure the .svn directories are left intact.
- Create a patch using the TortoiseSVN context menu in Windows Explorer. The
  patch should then contain all modifications you did to Raven's original
  VMs.
- Check out the same repository into a new directory, but this time the
  latest revision (HEAD).
- Apply the patch you created to the new dir using TortoiseSVN.
  If you're lucky and didn't change too much, everything will work fine. If
  you're unlucky, there will be conflicts that you have to resolve manually.
  For that you need to go into the conflicted files with an editor and
  search for the string "mine". It will first show you your version, then
  the one from the repository. Try to integrate these changes into your MOD.
- Build!

That's pretty much all you have to do. If you see that there are more
bugfixes included in my repository at a later time, you can use TortoiseSVN
to upgrade to this version. All changes are automatically merged into your
mod.

########################################################################
## For Engine fiddlers and Gentoo-I-compile-everything-myself-freaks ###
########################################################################

Since icculus.org's quake3 engine is GPL'ed, all my modifications are freely
available in the form of patches.

a) You must get a recent revision of ioquake3
   - see http://www.icculus.org/quake3

b) Download the most recent patch that modifies the code
   - see http://thilo.kickchat.com/efport-progress/patches/

c) Apply the patch
   - Linux users do: patch -p0 < thepatchname.diff in the main sourcedir
   - Windows users must install seperate tools like TortoiseSVN that have
     the ability to apply unix-style patches.

Linux users:
d) Make sure you got all packages needed, especially the -dev packages.
   You need the header files for libmad, libSDL, libopenal and libGL
   
e) You may need to edit the Makefile to make use of a gcc version < 4.
   gcc 4.0.x is not advisable if you need stability.

f) Type in "make eliteforce-debug" for a version with debugging symbols or
   "make eliteforce" for a release version. If all goes well, the new
   binaries will be found in the build/ subdir.

Windows users (will take a bit longer, experienced coders only):
d) Open up the project (project files can be found in code\win32\msvc\)
e) You need to compile the libmad library from:
   http://sourceforge.net/projects/mad
   so that you have a .lib file you can link against
f) If you compile and it complains about a missing dinput.h you need the
   DirectX SDK
g) If you want to have OpenAL you need the OpenAL SDK, too.
h) Back to your MSVC environment:
   look at the renderer and botlib subproject and modify the preprocessor
   definitions in a way so that ELITEFORCE is defined (#define ELITEFORCE),
   remove _DEBUG in the release version and add NDEBUG (only in release)
i) Same goes to the main quake3 subproject, also add the define:
   USE_CODEC_MP3=1 and USE_OPENAL=1;USE_OPENAL_DLOPEN=1 (the last two ones
   only when you want OpenAL support).
j) Add all include and library directories to the compiler/linker like
   OpenAL include/library dir, MAD include/library dir, etc...
k) You may need to specify additional libraries in the linker dependencies
   like user32.lib Advapi32.lib ole32.lib etc...

Good luck!

###############
### Credits ###
###############

My thanks go out to alot of people who were helpful in creating my mod:

 - A special thanks goes to TiM from the RPG-X project who helped with alot of
   information

 - The guys on the RPG-X board (Sharky, Scooter...)

 - CoreFusion for a bit of testing in the beginning :)

 - Skinner&Neural Link for infos about the MDR model format.


 -- Thilo Schulz (arny@ats.s.bawue.de)

README for StarTrek Voyager - Elite Force engine release
Project page: http://thilo.kickchat.com/efport-progress/
   - last updated 13.11.2006

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

############
### NEW! ###
############

Version 1.37:
- Sound attenuation has been changed to nearly the original behaviour.

- Ogg vorbis sound format is now supported natively by the engine.

- The Ioquake3 project has included a download redirect option! Server
  admins can now point with an URL where to download files from and the
  client can download them. See the README-IO.txt file for more info on how
  to use this.

- Antiwarp feature: Makes movement of people with wrong packet settings
  smooth. Enable it by setting cvar sv_antiWarp to 1.
  It comes in two flavours:
  sv_antiWarpBias 0 will hardly affect response to
  user commands (shoot, move, etc.), but will lead to many prediction
  errors.
  sv_antiWarpBias 1 won't have the problem with prediction errors but will
  add *a lot* of latency on misconfigured clients and slightly noticeable
  latency even on correctly configured client.
  A constant framerate and good connection is a must. This ensures that all
  players appear smoothly and those that do not - if their command rate is
  too unstable to compensate for - gain no advantage.
  A correctly configured client is one that uses the same value or a divider
  of com_maxfps for cl_maxpackets.

Version 1.36:

- There now is the possibility to revert the fastsky to EF's original color.
  set cvar r_origfastsky and the color of fastsky will be the normal beige
  you know.

- Ignore system. This is actually not specific to my engine but included in
  the VM updates in pak91.pk3. You can use it to ignore chat messages from
  abusive players. The ingame menu has been extended so you can conveniently
  add/remove players to/from your ignore list.
  Three new commands have been introduced: ignore, unignore and unignoreall,
  their names pretty much speak out for themselves.
  If the ignore or unignore command is called without playername, the person
  the crosshair is pointing at is affected. If you run it like:
  "/ignore evil 1", all players with the string "evil" in their name are
  affected. Colors in names will be stripped and don't affect the result at
  all.
  Of course, the ignore system is only available if the server has been
  updated with the pak91.pk3.

- On Windows, the settings and user specific data are saved in the "Document
  and Settings" folder. The fs_homepath cvar indicates the exact position.
  On most english systems, the folder is called:
  C:\Documents and Settings\<username>\Application Data\STVEF
  See README-IO.txt for more information.

############################
### GENERAL INSTRUCTIONS ###
############################

All players and server admins, no matter whether users of the old or new
EliteForce engine should add the pak92.pk3 to their baseEF directories.
If you have old pak files from older versions of this engine, delete them.
The files to be deleted - if present - are:
pak9.pk3, pak90.pk3 and pak91.pk3
If this README is from an archive (zip or bzip2) or an automated installer
the pak92.pk3 will already be included.. You can also find the file here:
http://thilo.kickchat.com/efport-progress/bin/vm/
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
using Raven's original installer. You must also install either the expansion
pack or the official patch to upgrade your installation to version 1.2.

Once you have done all this, run the windows installer.  The installer
will ask you for an installation path.  This path should be where your
stvoyHM.exe file resides.  The default is to the Elite Force default
installation folder.  The installer will also double check to be sure
you've chosen the correct location.  Once the installer completes you
will find a new group in your start menu called "Elite Force Holomatch 1.37"
With shortcuts to run the game using the new patch and shortcuts to this
read me file.

After installing, a new file named iostvoyHM-1.37.exe has been created. If
you want to use my improved engine you must start this one instead of the
original stvoyHM.exe.

This release comes with OpenAL support for sound which improves sound for
most users. An OpenAL installation tool comes with this installer and can
optionally be run from it. If you have got the latest version installed
already, of course you do not need to reinstall OpenAL.
Keep in mind, that OpenAL support is still somewhat experimental. For alot
of people OpenAL gives a dramatical improvement in sound.
Still, depending on the hardware and drivers in use, sound may be unusual or
even worse with OpenAL. If you don't like it, you can always turn off OpenAL
support by disabling it in the sound settings.
Since version 1.35 some changes have happened in the way OpenAL gets used.
Default settings will now make OpenAL work well on most hardware setups. If
you had sound problems with previous versions, this release will likely fix
them for you. Device enumeration support has been implemented, so you now
have the choice between different devices.
For most people, two devices are available: "Generic Hardware" and
"Generic Software". Previous versions had "Generic Hardware" as default
which uses DirectSound 3D and was often causing troubles with OpenAL. If you
don't belong to these users, you can switch back by selecting the device in
the sound configuration menu.

########################################
### MACOSX INSTALLATION INSTRUCTIONS ###
########################################

Install the normal game from CD if you have not already done so. Make also
sure to have the expansion pack, or at least the latest patch version 1.2
that is available for EliteForce, installed.
Now extract the .dmg file to the installation directory of your EliteForce
game. Run iostvoyHM-1.37 to start the game.

There have been problems with the MacOSX version of OpenAL and ioquake3.
This is why OpenAL is disabled per default in this release.
You can still enable OpenAL sound in the sound configuration menu, though.
You will have to click the sound quality setting in the sound configuration
menu to change the backend used. Maybe it works fine for you so you won't
have to play with the standard sound.

#######################################
### LINUX INSTALLATION INSTRUCTIONS ###
#######################################

Prerequisites:
 * A working hardware accelerated OpenGL setup 
   (DRI, fireglx, nvidia-glx...)
 * libSDL - Simple Direct Layer libraries
 * libmad - MPEG Audio Decoder (also known as MAD sometimes).
 * libvorbis, libogg, libvorbisfile - OGG Vorbis audio decoder
Optional:
 * OpenAL - OpenAL sound libraries
 * libcurl - FTP/HTTP download support.

You can fullfill these dependencies by installing the correct packages from
your favorite distribution's packet manager.

There's a loki installer available that installs all the relevant files for
you in an automated manner. The installer covers both, binaries for 64 bit
and 32 bit systems so there's no split-up into two archives anymore.
The installer offers you to copy the pak*.pk3 files from the original
EliteForce CDs. If you don't want this to happen for some reason, see the
.pk3 file copying instructions in the manual installation instructions a few
lines down.

This is the location of the installer:
http://thilo.kickchat.com/efport-progress/bin/linux/io_eliteforce-1.37.run
Run it in a shell like:
sh iostvoyHM-1.37.run

If you have an old hmconfig.cfg from windows, don't add it to baseef but to
your home directory in ~/.stvef/baseEF/
Speaking of that dir, the hidden directory where user specific config
files/maps/mods will be stored is - as you may have guessed by now -
.stvef in your home directory.

Should the installer fail for some reason you can still install manually:

Create a new eliteforce directory, you will probably want to use a dir like:
/usr/local/games/stvef

Copy a build of this engine that you can get from
http://thilo.kickchat.com/efport-progress/bin/
into the new directory.
Also copy any directories from MODs you installed previously on windows to
this directory.

Create a subdirectory named "baseEF" (case sensitive!!) and copy the
pak0.pk3, pak1.pk3 and pak2.pk3 from the original eliteforce
release in there and don't forget the new pak91.pk3!
pak1.pk3 and pak2.pk3 are files that have been released after the game went
gold. You must either get them by installing the latest official EF patch on
windows or you can get them from the expansion pack cd.
pak3.pk3 can only be found on the expansion pack CD. If you want to have the
maps and additions from the expension pack, you must copy this file as well.

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
the demo selection menu now has scrollbars (thanks to TiM), the EF LCARS
patch from Lt. Cmdr. Salinga has been applied. A more complete
list can be found here:
http://svn.kickchat.com/index.cgi/stvoy/trunk/ChangeLog?view=log

The patch can be found here:
http://thilo.kickchat.com/efport-progress/patches/stvoyVM-bugfixes-1.37.diff
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
  You must make sure that you really check out the very first revision!
- Overwrite all source files in that newly checked out directory with the
  source files from your mod. Make sure the ".svn" directories are left
  intact.
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
- Build
- Be sure to add the language .dat files (you find them in baseef/ext_data
  in the subversion repository) in your pk3.

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

f) Type in "make debug" for a version with debugging symbols or "make" for a
   release version. If all goes well, the new binaries will be found in the
   build/ subdir.

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
l) Build.

Good luck!

###############
### Credits ###
###############

My thanks go out to alot of people who were helpful in creating my mod:

 - A special thanks goes to TiM from the RPG-X project who helped with alot
   of information

 - The guys on the RPG-X board (Sharky, Scooter...)

 - CoreFusion for a bit of testing in the beginning :)

 - Skinner&Neural Link for infos about the MDR model format

 - Thomas Sahling aka Lt. Cmdr. Salinga for the source code to his LCARS
   patch

 - Brian Shaffer aka Shafe for the windows installer

 - Derek O. for a PPC Linux and MacOSX shell

 -- Thilo Schulz (arny@ats.s.bawue.de)

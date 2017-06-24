# Jetson-TX2-Install-ROS-Indigo
Guide with how to install ros indigo on Jetson TX2

Solve Deps:
1. Build Ogre 1.8.1 from https://sourceforge.net/projects/ogre/files/ogre/1.8/1.8.1/ogre_src_v1-8-1.tar.bz2/download
   need to patch "This can be fixed by adding " || defined(__aarch64__)" to the end of line 136 in OgreMain/include/OgrePlatform.h"
2. player: https://sourceforge.net/projects/playerstage/files/Player/3.0.2/player-3.0.2.tar.gz/download
   (1) need to patch "client_libs/libplayerc++/playerclient.cc" from "boost::xtime_get(&xt, boost::TIME_UTC)" to "boost::xtime_get(&xt,   boost::TIME_UTC_)"
   (2) To be specific, in client_libs/libplayerc++/CMakeLists.txt, there are two mechanisms to find boost.  One is for CMake 2.6+ and one is a fallback for older versions of CMake.  The logic is faulty though: the scripts check for "CMAKE_MINOR_VERSION EQUAL 6" which worked for CMake 2.6, but is broken for CMake 2.8.  To fix the issue, replace "CMAKE_MINOR_VERSION EQUAL 6" with "CMAKE_MINOR_VERSION GREATER 5". This is the fix that went into svn. (Recomended)
       or
       run cmake first, then change the CMAKE_CXX_FLAGS variable in /player-3.0.2/build/CMakeCache.txtChange the line 
       CMAKE_CXX_FLAGS:STRING=
       CMAKE_EXE_LINKER_FLAGS=
       To
       CMAKE_CXX_FLAGS:STRING=-lboost_system
       CMAKE_EXE_LINKER_FLAGS=-lboost_system
   (3) readlog.cc got some "File*" force convert to "gzFile_s*" error, fix them mansualy by simplely adding "(gzFile_s*)" before "this->file".
3. stage: https://sourceforge.net/p/playerstage/svn/HEAD/tree/code/stage/trunk/
   (1) yorkroboticist.blogspot.jp/2011/12/upgrading-to-ubuntu-1110-with.html
   (2) https://sourceforge.net/p/playerstage/patches/586/
4. gazebo2: http://gazebosim.org/tutorials?tut=install_from_source#BuildAndInstallGazebo

Ros Build:
1. rosdep install --from-paths src --ignore-src --rosdistro indigo -y --skip-keys="python-opencv python-qt-bindings-webkit opencv libopencv-dev libopencv"
2. 

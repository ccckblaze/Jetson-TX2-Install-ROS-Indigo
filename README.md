# Jetson-TX2-Install-ROS-Indigo
Guide with how to install ros indigo on Jetson TX2

Solve Deps:
1. Build Ogre 1.8.1 from https://sourceforge.net/projects/ogre/files/ogre/1.8/1.8.1/ogre_src_v1-8-1.tar.bz2/download
   need to patch "This can be fixed by adding " || defined(__aarch64__)" to the end of line 136 in OgreMain/include/OgrePlatform.h"
   
2. player: https://sourceforge.net/projects/playerstage/files/Player/3.0.2/player-3.0.2.tar.gz/download
   (1) need to patch "client_libs/libplayerc++/playerclient.cc" from "boost::xtime_get(&xt, boost::TIME_UTC)" to "boost::xtime_get(&xt, boost::TIME_UTC_)"
   (2) run cmake first, then change the CMAKE_CXX_FLAGS variable in /player-3.0.2/build/CMakeCache.txt
       Change the line 
       CMAKE_CXX_FLAGS:STRING=
       To
       CMAKE_CXX_FLAGS:STRING=   -lboost_system
      
3. stage:  https://sourceforge.net/projects/playerstage/files/Stage/3.2.2/Stage-3.2.2-Source.tar.gz/download
4. gazebo2: http://gazebosim.org/tutorials?tut=install_from_source#BuildAndInstallGazebo

Ros Build:
1. rosdep install --from-paths src --ignore-src --rosdistro indigo -y --skip-keys="python-opencv python-qt-bindings-webkit opencv libopencv-dev libopencv"
2. 

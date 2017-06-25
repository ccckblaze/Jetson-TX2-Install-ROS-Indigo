# Jetson-TX2-Install-ROS-Indigo
Guide with how to install ros indigo on Jetson TX2

Solve Deps:
1. Build Ogre 1.8.1 from https://sourceforge.net/projects/ogre/files/ogre/1.8/1.8.1/ogre_src_v1-8-1.tar.bz2/download
   need to patch "This can be fixed by adding " || defined(__aarch64__)" to the end of line 136 in OgreMain/include/OgrePlatform.h"
2. player: https://sourceforge.net/projects/playerstage/files/Player/3.0.2/player-3.0.2.tar.gz/download
   (1) need to patch "client_libs/libplayerc++/playerclient.cc" from "boost::xtime_get(&xt, boost::TIME_UTC)" to "boost::xtime_get(&xt,   boost::TIME_UTC_)"
   (2) use patch/CMakeLists.txt replace client_libs/libplayerc++/CMakeLists.txt to fix the boost_system link error
3. stage: https://sourceforge.net/p/playerstage/svn/HEAD/tree/code/stage/trunk/
   (1) yorkroboticist.blogspot.jp/2011/12/upgrading-to-ubuntu-1110-with.html
   (2) https://sourceforge.net/p/playerstage/patches/586/]
4. libformat2.0: http://osrf-distributions.s3.amazonaws.com/sdformat/releases/sdformat-2.0.1.tar.bz2
5. gazebo2: http://gazebosim.org/tutorials?tut=install_from_source#BuildAndInstallGazebo
   (1) fix "has_binary_operator.hp:50: Parse error at 'BOOST_JOIN'" by add "#ifndef Q_MOC_RUN" and "#endif" around "gazebo/transport/TransportTypes.hh", "/gazebo/msgs/msgs.hh" and all the gui .hh header files in "gazebo/gui/" and sub-folders
   (2) "cmake ../ -DENABLE_TESTS_COMPILATION=False" use ENABLE_TESTS_COMPILATION=False to disable the gtest, in order to prevent the gazebo_sensors and gazebo_physics circle reference error.
   (3) "terrain/libgazebo_gui_terrain.a(TerrainEditorPalette.cc.o): relocation R_AARCH64_ADR_PREL_PG_HI21 against external symbol `__stack_chk_guard@@GLIBC_2.17' can not be used when making a shared object; recompile with -fPIC" just append "-fpic" to "CMAKE_CXX_FLAGS_RELWITHDEBINFO:STRING=-O2 -g -DNDEBUG" in CMakeCache.txt
   
Ros Build:
1. rosdep install --from-paths src --ignore-src --rosdistro indigo -y --skip-keys="python-opencv python-qt-bindings-webkit opencv libopencv-dev libopencv"
2. 

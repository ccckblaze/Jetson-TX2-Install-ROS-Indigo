# Jetson-TX2-Install-ROS-Indigo
Guide with how to install ros indigo on Jetson TX2

## Solve Deps:
1. Build Ogre 1.8.1 from https://sourceforge.net/projects/ogre/files/ogre/1.8/1.8.1/ogre_src_v1-8-1.tar.bz2/download 
   need to patch "This can be fixed by adding " || defined(__aarch64__)" to the end of line 136 in OgreMain/include/OgrePlatform.h" 
2. player: https://sourceforge.net/projects/playerstage/files/Player/3.0.2/player-3.0.2.tar.gz/download 
   (1) need to patch "client_libs/libplayerc++/playerclient.cc" from "boost::xtime_get(&xt, boost::TIME_UTC)" to "boost::xtime_get(&xt,   boost::TIME_UTC_)" 
   (2) use patch/CMakeLists.txt replace client_libs/libplayerc++/CMakeLists.txt to fix the boost_system link error 
3. stage: https://sourceforge.net/projects/playerstage/files/Stage/3.2.2/Stage-3.2.2-Source.tar.gz/download
   (1)   https://sourceforge.net/p/playerstage/mailman/playerstage-users/thread/32826178.post%40talk.nabble.com/
    -Wl,--no-as-needed
   (2) yorkroboticist.blogspot.jp/2011/12/upgrading-to-ubuntu-1110-with.html 
   (3) https://sourceforge.net/p/playerstage/patches/586/
4. libformat2.0: http://osrf-distributions.s3.amazonaws.com/sdformat/releases/sdformat-2.0.1.tar.bz2 
5. gazebo2: http://gazebosim.org/tutorials?tut=install_from_source#BuildAndInstallGazebo 
   (1) change "/usr/include/boost/type_traits/detail/has_binary_operator.hpp"
      namespace BOOST_JOIN(BOOST_TT_TRAIT_NAME,_impl) {
      ..
      } //namespace impl
      to
      #ifndef Q_MOC_RUN
      namespace BOOST_JOIN(BOOST_TT_TRAIT_NAME,_impl) {
      ..
      } //namespace impl
      #endif
      or
      fix "has_binary_operator.hp:50: Parse error at 'BOOST_JOIN'" by add "#ifndef Q_MOC_RUN" and "#endif" around "gazebo/transport/TransportTypes.hh", "/gazebo/msgs/msgs.hh" and all the gui .hh header files in "gazebo/gui/" and sub-folders 
   (2) "cmake ../ -DENABLE_TESTS_COMPILATION=False" use ENABLE_TESTS_COMPILATION=False to disable the gtest, in order to prevent the gazebo_sensors and gazebo_physics circle reference error. 
   (3) change CMakeList.txt "set (CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS}${WARNING_CXX_FLAGS}")" to set "(CMAKE_CXX_FLAGS "-fPIC ${CMAKE_CXX_FLAGS}${WARNING_CXX_FLAGS}")" to aoivd "terrain/libgazebo_gui_terrain.a(TerrainEditorPalette.cc.o): relocation R_AARCH64_ADR_PREL_PG_HI21 against external symbol `__stack_chk_guard@@GLIBC_2.17'
   
## Ros Build:
1. rosdep install --from-paths src --ignore-src --rosdistro indigo -y --skip-keys="python-opencv python-qt-bindings-webkit opencv libopencv-dev libopencv" 
2. sudo apt install libpcl-dev
3. do the same as 5(1) told or you will got BOOST error in rviz
2. sudo ./src/catkin/bin/catkin_make_isolated --install -DCMAKE_BUILD_TYPE=Release --install-space /opt/ros/indigo -DCUDA_USE_STATIC_CUDA_RUNTIME=OFF

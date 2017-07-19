# moveit/moveit:kinetic-experimental
# Based on a moveit source install, adds the mongo driver and the warehouse packages

FROM moveit/moveit:kinetic-source

ENV CATKIN_WS=/root/ws_moveit
RUN mkdir -p $CATKIN_WS/src
WORKDIR $CATKIN_WS/src

# installing mongocxx driver for the warehouse
# &" is used to chain commands together, 
#such that the next command is run if and only if the preceding command exited without errors 
# (or, more accurately, exits with a return code of 0).
# "\" by itself at the end of a line is a means of concatenating lines together.
RUN git clone -b 26compat https://github.com/mongodb/mongo-cxx-driver.git && \
    # -qq: No output except for errors
    apt-get update -qq && \
    apt-get -qq install -y scons mongodb && \
    cd mongo-cxx-driver && \
    scons --use-system-boost --prefix=/usr/local/ --full --disable-warnings-as-errors && \
    ls /usr/local/lib && \
    # scons install && \
    rm -rf /var/lib/apt/lists/*

# Download warehouse source
RUN wstool set -yu warehouse_ros_mongo --git https://github.com/ros-planning/warehouse_ros_mongo.git -v jade-devel && \
    wstool set -yu warehouse_ros --git https://github.com/ros-planning/warehouse_ros.git -v jade-devel

# build the workspace
RUN cd $CATKIN_WS && \
    source /root/ws_moveit/devel/setup.bash && \
    catkin init  && \
    catkin build

FROM ubuntu:bionic

MAINTAINER "Rishav Singh <rishav.zero@pm.me>"

RUN echo 'Etc/UTC' > /etc/timezone && \
    ln -s /usr/share/zoneinfo/Etc/UTC /etc/localtime && \
    apt-get update && \
    apt-get install -q -y --no-install-recommends tzdata

# install packages
RUN apt-get update && apt-get install -q -y --no-install-recommends \
    dirmngr \
    gnupg2

# setup keys
RUN apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys C1CF6E31E6BADE8868B172B4F42ED6FBAB17C654

# setup sources.list
RUN echo "deb http://packages.ros.org/ros/ubuntu bionic main" > /etc/apt/sources.list.d/ros1-latest.list

# setup environment
ENV LANG C.UTF-8
ENV LC_ALL C.UTF-8

ENV ROS_DISTRO melodic

# install ros packages
RUN set -x \
    && apt-get update && apt-get install -y \
    ros-melodic-desktop-full \
    && echo "source /opt/ros/melodic/setup.bash" >> ~/.bashrc

# rosdep setup and catkin installation
RUN set -ex \
    && apt-get install -y \
    python-rosdep ros-melodic-catkin python-catkin-tools ros-melodic-std-msgs \
    && rosdep init \
    && rosdep update

# Solving gazebo error
COPY ./.ignition /root/.ignition

# catkin_ws setup
RUN set -ex \
    && mkdir --parents ~/catkin_ws/src && cd ~/catkin_ws \
    && /bin/bash -c "cd ~/catkin_ws && . /opt/ros/melodic/setup.bash && catkin init && catkin build" \
    && echo "source ~/catkin_ws/devel/setup.bash" >> ~/.bashrc


# setup entrypoint
# COPY ./ros_entrypoint.sh /

# ENTRYPOINT ["/ros_entrypoint.sh"]
# CMD ["bash"]

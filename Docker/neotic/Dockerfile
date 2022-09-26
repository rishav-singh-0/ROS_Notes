FROM ubuntu:focal

MAINTAINER "Rishav Singh <rishav.zero@pm.me>"

# setup timezone
RUN echo 'Etc/UTC' > /etc/timezone && \
    ln -s /usr/share/zoneinfo/Etc/UTC /etc/localtime && \
    apt-get update && \
    apt-get install -q -y --no-install-recommends tzdata && \
    rm -rf /var/lib/apt/lists/*

# install packages
RUN apt-get update && apt-get install -q -y --no-install-recommends \
    dirmngr \
    gnupg2 \
    && rm -rf /var/lib/apt/lists/*

# setup keys
RUN apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys C1CF6E31E6BADE8868B172B4F42ED6FBAB17C654

# setup sources.list
RUN echo "deb http://packages.ros.org/ros/ubuntu focal main" > /etc/apt/sources.list.d/ros1-latest.list

# setup environment
ENV LANG C.UTF-8
ENV LC_ALL C.UTF-8

ENV ROS_DISTRO noetic

# install ros packages
RUN apt-get update && apt-get install -y --no-install-recommends \
    && rm -rf /var/lib/apt/lists/*
RUN set -x \
    && apt-get update && apt-get install -y --no-install-recommends \
    ros-noetic-desktop-full \
    && echo "source /opt/ros/$ROS_DISTRO/setup.bash" >> /root/.bashrc

# install bootstrap tools
RUN set -ex \
    && apt-get update && apt-get install --no-install-recommends -y \
    build-essential \
    python3-rosdep \
    python3-rosinstall \
    python3-rosinstall-generator \
    python3-wstool \
    python3-vcstools \
    && rm -rf /var/lib/apt/lists/*

# bootstrap rosdep
RUN rosdep init && \
  rosdep update

# catkin installation and setup
RUN set -ex \
    && apt-get install -y \
    ros-noetic-catkin python3-catkin-tools \
    ros-noetic-ros-tutorials \
    && mkdir --parents /root/catkin_ws/src && cd /root/catkin_ws \
    && /bin/bash -c "cd /root/catkin_ws && . /opt/ros/$ROS_DISTRO/setup.bash && catkin_init_workspace && catkin_make" \
    && echo "source /root/catkin_ws/devel/setup.bash" >> /root/.bashrc

ENTRYPOINT ["/bin/bash"]

# Base Image: https://hub.docker.com/r/osrf/ros/tags?page=1&name=humble
FROM osrf/ros:humble-desktop-full AS amd64
# Base Image: https://hub.docker.com/r/arm64v8/ros/tags?page=1&name=humble
FROM arm64v8/ros:humble AS arm64

# Use docker automatic platform args to select the base image.
# It may be `arm64` or `amd64` depending on the platform.
# Ref: https://docs.docker.com/reference/dockerfile/#automatic-platform-args-in-the-global-scope
FROM $TARGETARCH
ARG TARGETARCH

# Arguments for the default user
ARG USERNAME=user
ARG USER_UID=1000

# Upgrade packages
RUN apt-get update && apt-get upgrade -y \
    && rm -rf /var/lib/apt/lists/*

# Install sudo and create a user with sudo privileges
# Ref: https://stackoverflow.com/a/65434659
RUN apt-get update && apt-get install -y sudo \
    && useradd -m -s /bin/bash -u $USER_UID -G sudo $USERNAME \
    && echo '%sudo ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers \
    && rm -rf /var/lib/apt/lists/*

# Install common tools
RUN apt-get update && apt-get install -y \
    curl \
    gnupg2 \
    lsb-release \
    git \
    htop \
    iputils-ping \
    nano \
    net-tools \
    tmux \
    tree \
    unzip \
    vim \
    wget \
    zip \
    python3-pip \
    software-properties-common \
    mesa-utils \
    x11-apps \
    x11-utils \
    libgl1 \
    libgl1-mesa-glx \
    libgl1-mesa-dri \
    portaudio19-dev \
    && rm -rf /var/lib/apt/lists/*

# Install ros2 dependencies
RUN apt-get update && apt-get install -y \
    ros-humble-rviz2 \
    ros-humble-rmw-cyclonedds-cpp 

# Install Python requirements (with numpy constraint)
COPY --chown=$USERNAME:$USERNAME requirements.txt /home/$USERNAME/requirements.txt
RUN pip3 install -r /home/$USERNAME/requirements.txt

# Other
COPY --chown=$USERNAME:$USERNAME .bashrc /home/$USERNAME/.bashrc
USER $USERNAME
ENTRYPOINT []
CMD ["/bin/bash"]

# Shell Virtual Machine Setup

## Prerequisites

Download and install VirtualBox from: [VirtualBox](https://www.virtualbox.org/)
Download Ubuntu 22.04 ISO from: [Ubuntu 22.04](https://releases.ubuntu.com/jammy/)

## Create a Virtual Machine

- Open VirtualBox and create a new VM
- Name: **ShellAPC_VM**
- Type: **Linux**
- Version: **Ubuntu (64-bit)**
- Allocate at least **4 CPU cores, 8GB RAM, and 90GB of disk space**
- Enable **3D Acceleration** under Display settings

## Install Ubuntu 22.04

- Boot from Ubuntu ISO and follow the installation wizard
- After installation, open the terminal in ubuntu and update the system:
  ```bash
  sudo apt update && sudo apt upgrade -y
  ```

## Install Dependencies

Install essential packages:
  ```bash
  sudo apt install -y curl git apt-transport-https ca-certificates \
      gnupg software-properties-common build-essential locales \
      python3-colcon-common-extensions
  ```

## Install Docker

- Update and install Docker:
``` bash
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

  ## Install NVIDIA Container Toolkit (If Using NVIDIA GPU)
- Add repository and install:
  ```bash
  curl -s -L https://nvidia.github.io/nvidia-docker/gpgkey | sudo apt-key add -
  curl -s -L https://nvidia.github.io/nvidia-docker/ubuntu22.04/nvidia-docker.list | sudo tee /etc/apt/sources.list.d/nvidia-docker.list
  sudo apt update
  sudo apt install -y nvidia-container-toolkit
  sudo nvidia-ctk runtime configure --runtime=docker
  sudo systemctl restart docker
  ```

## Install ROS 2 Humble
Follow the guide here https://docs.ros.org/en/humble/Installation/Ubuntu-Install-Debs.html

## Clone and Set Up Shell Eco-marathon APC Docker Environment

- Clone the repository and run the setup script:

```bash
git clone https://github.com/swri-robotics/sem-apc-student-docker-environment.git
cd sem-apc-student-docker-environment
./run.sh

```
## Verify Docker Containers

* Check running containers:
  `docker ps -a`
* LATER if u want to Start containers:
  `docker start carla_server ros_environment`
- LATER if u want to Stop containers:
  `docker stop carla_server ros_environment`

## Access ROS Workspace

- Enter ROS environment:
  ```bash
  docker exec -it ros_environment /bin/bash
  ```

- Navigate to the workspace and install dependencies:
  ```bash
  cd shell_ws
  sudo apt update
  rosdep update
  rosdep install --from-paths src -y --ignore-src
  ```


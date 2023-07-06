# Autoware in CARLA

The carla autoware bridge is now hosted and maintained [here](https://github.com/Autoware-AI/simulation/tree/master/carla_simulator_bridge).

This repository contains a demonstrator of an autoware agent ready to be executed with CARLA.

**The carla autoware integration requires CARLA 0.9.11. You can download it from [here](https://github.com/carla-simulator/carla/releases/tag/0.9.11)**



## CARLA autoware agent
The autoware agent is provided as a ROS package. All the configuration can be found inside the `carla-autoware-agent` folder.

![carla-autoware](docs/images/carla_autoware.png)

The easiest way to run the agent is by building and running the provided docker image.

### Requirements

- Docker (19.03+)
- Nvidia docker (https://github.com/NVIDIA/nvidia-docker)
- Carla (0.9.11)
- Cuda (10.2)
  
### Setup

Firstly download Carla simulator (version 0.9.11 release)[https://github.com/carla-simulator/carla/releases/tag/0.9.11].

Then Setup environment variables for Carla.

```
export CARLA_ROOT=/path/to/your/carla/installation
export PYTHONPATH=$PYTHONPATH:${CARLA_ROOT}/PythonAPI/carla
export PYTHONPATH=$PYTHONPATH:${CARLA_ROOT}/PythonAPI/carla/agents
export PYTHONPATH=$PYTHONPATH:${CARLA_ROOT}/PythonAPI/carla/dist/carla-0.9.11-py3.7-linux-x86_64.egg
```

install git-lfs
```
curl -s https://packagecloud.io/install/repositories/github/git-lfs/script.deb.sh | sudo bash
sudo apt-get install git-lfs
git lfs install
```


clone the carla autoware repository, where additional [autoware contents](https://bitbucket.org/carla-simulator/autoware-contents.git) are included as a submodule:

```sh
git lfs clone --recurse-submodules https://github.com/casper-auto/carla-autoware.git
```

Afterwards, build the image with the following command:

```sh
cd carla-autoware && ./build.sh
```

This will generate a `carla-autoware:latest` docker image.

### Run the agent

1. Run a CARLA server.

```
./CarlaUE4.sh
```

2. Run the `carla-autoware` image:

```sh
./run.sh
```

This will start an interactive shell inside the container. To start the agent run the following command:

```sh
roslaunch carla_autoware_agent carla_autoware_agent.launch town:=Town01
```

### notes：

1、The not subscribed topics may not be an issue. Try to give the ego car a goal in Rviz (just click goal in navi bar then click anywhere on the map) and see if it moves.
2、Step 12 may encounter a keyring GMG error, while step 21 encounters a 443 error when cloning carla Ros Bridge. This can be resolved by connecting to a mobile hotspot.


## CARLA Autoware contents
The [autoware-contents](https://bitbucket.org/carla-simulator/autoware-contents.git) repository contains additional data required to run Autoware with CARLA, including the point cloud maps, vector maps and configuration files.

# Murmuration - Starling system runners and deployments

A repository for deployment and running of a preset collection of docker-compose and kubernetes deployment files. 

## Docker-Compose

The [`docker-compose`](docker-compose) contains a number of preset examples which spin up at minimum a simulator, a software autopilot, mavros and a ros webridge, but also example ui's and basic controllers. 

Each example file has a *linux* and *windows* variant. 
- The *linux* variant runs `network_mode=host` which allows for ROS2 traffic to be shared between the container and your local network. Therefore the traffic is visible to your machine's local (non-container) ROS2 foxy instance, allowing you to run bare-metal application such as rviz2 or your own controllers (i.e. you do not need to wrap your own controllers in a docker container). Any exposed ports are automatically exposed to `localhost`. 
- The *windows* variant runs inside a docker-compose network named `<folder-which the-docker-compose-file-is-in>_default`, (e.g. running a px4 example will create a local network `px4_default`). This network is segregated from your local network traffic *except* for the exposed ports in the docker-compose file which are now accessible from `localhost` (Windows has no support of `net=host`). Any other ROS2 nodes will need to be wrapped in a docker container for running and run with `--network px4_default` or `--network ardupilot_default`. See the example controller repository for an example ROS2-in-docker setup. 

### PX4 Examples

The [docker-compose/px4](docker-compose/px4) folder contains a number of px4 based examples. See the [README](docker-compose/px4/README.md) for more details. 

#### Core System
To start the simulator, a px4 SITL, mavros and the web-bridge, use the following command:
```
# First Pull the relevant docker containers
docker-compose -f docker-compose/px4/docker-compose.core.linux.yml pull
# or for windows
# docker-compose -f docker-compose/px4/docker-compose.core.windows.yml pull

# Run the docker-containers
docker-compose -f docker-compose/px4/docker-compose.core.linux.yml up
# or for windows
# docker-compose -f docker-compose/px4/docker-compose.core.windows.yml up
```

#### Simple Offboard with Trajectory Follower UI System
To start the core with a [simple-offboard controller](https://github.com/StarlingUAS/starling_simple_offboard), [simple-allocator](https://github.com/StarlingUAS/starling_allocator) and [Trajectory follower Web GUI](https://github.com/StarlingUAS/starling_ui_dashly), use the following command:

```
# First Pull the relevant docker containers
docker-compose -f docker-compose/px4/docker-compose.simple-offboard.linux.yml pull
# or for windows
# docker-compose -f docker-compose/px4/docker-compose.simple-offboard.windows.yml pull

# Run the docker-containers
docker-compose -f docker-compose/px4/docker-compose.simple-offboard.linux.yml up
# or for windows
# docker-compose -f docker-compose/px4/docker-compose.simple-offboard.windows.yml up
```

### Ardupilot Examples

The [docker-compose/ardupilot](docker-compose/ardupilot) folder contains a number of px4 based examples. See the [README](docker-compose/ardupilot/README.md) for more details. 

#### Core System
To start the gazebo simulator, an arducopter SITL, mavros and the web-bridge, use the following command:
```
# First Pull the relevant docker containers
docker-compose -f docker-compose/ardupilot/docker-compose.ap-gazebo.linux.yml pull
# or for windows
# docker-compose -f docker-compose/ardupilot/docker-compose.ap-gazebo.windows.yml pull

# Run the docker-containers
docker-compose -f docker-compose/ardupilot/docker-compose.ap-gazebo.linux.yml up
# or for windows
# docker-compose -f docker-compose/ardupilot/docker-compose.ap-gazebo.windows.yml up
```

### Running External Examples
An example offboard ROS2 controller can then be conncted to SITL by running the following in a separate terminal:

```
# Download the latest container
docker pull uobflightlabstarling/example_controller_python 

docker run -it --rm --network px4_default uobflightlabstarling/example_controller_python
# or for ardupilot
docker run -it --rm --network ardupilot_default uobflightlabstarling/example_controller_python
```

See [the docs](https://docs.starlinguas.dev/guide/single-drone-local-machine/#2-running-example-ros2-offboard-controller-node) for further details
## Kubernetes

TODO
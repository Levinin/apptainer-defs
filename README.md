# Apptainer definition files

A small repo of apptainer definition files I have put together for my own use and in an effort to keep my base install clean.

| # | Name | Purpose | Active | Issues |
|---|------|---------|---|--------|
| 1  |  ros2_humble.def | A ROS2 Humble container with Ubuntu 22.04, python 3.10, and tmux. | Y | Only for use in sandbox mode. |
| 2  |  simple_ml.def | An Ubuntu 22.04 container for data analysis without use of DNNs with sklearn, pandas, matplotlib, jupyter etc. | Y  | Only for use in sandbox mode. |
|   |      |         |   |        |

## Installing Apptainer Ubuntu - quick guide

Full instructions are on the [Apptainer website](https://apptainer.org/docs/admin/main/installation.html).
To quickly install on managed Ubuntu devices, i.e. without adding a new repo:

```
wget https://github.com/apptainer/apptainer/releases/download/v1.4.0/apptainer_1.4.0_amd64.deb
$ sudo apt install -y ./apptainer_1.4.0_amd64.deb
```

## Basic use for these containers

### ROS2 humble

```
apptainer build --sandbox ros2_humble/ ros_humble.def
apptainer shell --no-home --bind /your/colcon_ws/dir:/colcon_ws --writable ros2_humble/
cd /colcon_ws
tmux
source /opt/ros/humble/setup.bash
colcon build
source install/local_setup.bash
ros2 run ...
```

Note the `--no-home` above. This is to avoid some funky jupyter and tmux behaviour where new terminals read your local .bashrc meaning all sorts of non-container related stuff is enabled.

Now create a new package, copy files from outside the directory as required.
It is possible to use your favourite editor directly, treating the container as a directory.
Alternatively, use Jupyter by running the following in the container:

```
jupyter lab --ip=127.0.0.1 --port=8888 --no-browser
```

And then copy the url from the message, remembering to include the key, and paste into your local browser.

The ROS# bridge for ROS2 and Unity should be installed which can be started with:
```
source /opt/ros/humble/setup.bash
ros2 launch rosbridge_server rosbridge_websocket_launch.xml
```

### Simple ML

Very similar to ROS2. This version is made for use with Jupyter Lab as above.
Instead of a `/colcon_ws` directory there is a `/python_scripts` directory for development.

```
apptainer build --sandbox simple_ml/ simple_ml.def
apptainer shell --no-home --bind /your/python/script/dir:/python_scripts --writable simple_ml/
tmux
jupyter lab --ip=127.0.0.1 --port=8888 --no-browser
```

And then copy the url from the message, remembering to include the key, and paste into your local browser.

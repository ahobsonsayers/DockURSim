# DockURSim

## A URSim (Universal Robots Simulator) Docker Container with a Browser Accessible Interface

Runs URSim in a docker container with the Polyscope Interface accessible via a browser.

This is a suitable alternative to using the URSim virtual machine image on windows.

**Current URSim Version: 5.4.2.76197**

## Example Usage

First create volume to store robot configuration.

```bash
docker volume create dockursim
```

Start URSim with an UR5 robot and all interface ports exposed.

```bash
docker run -d \
    --name="dockursim" \
    -e ROBOT_MODEL=UR5 \
    -p 8080:8080 \
    -p 29999:29999 \
    -p 30001-30004:30001-30004 \
    -v /path/to/mount/program/folder:/ursim/programs \
    -v dockursim:/ursim \
    --privileged \
    --cpus=1 \
    arranhs/dockursim:latest
```

The Universal Robot Interface can now be accessed at http://localhost:8080.

**Ctrl-Alt-Shft** will open a menu that allows changing input options, as well as controlling the clipboard.

**NOTE!** This container requires running with ```--privileged``` set due to pthread spawn issues that couldnt be solved in any other way. This has security implications so DO NOT expose this container to the internet without doing your due diligence first.
**Use this container at your own risk**.

**NOTE Number 2!** I highly recommend running this container with the ```--cpus=1``` option as the container seems to use all available machine resources otherwise and this will likely cause your simulator container (and possibly your computer) to become unresponsive or slow.

## Parameters

Container is configured using parameters passed at runtime.

|       Parameter       | Function                                                                                   |
| :-------------------: | ------------------------------------------------------------------------------------------ |
|  `-e ROBOT_TYPE=UR5`  | Specify robot model to use in URSim. Valid options are UR3, UR5 and UR10. Defaults to UR5. |
| `-e TZ=Europe/London` | Specify a timezone to use e.g. Europe/London                                               |
|    `-e PUID=1000`     | Set UserID                                                                                 |
|    `-e PGID=1000`     | Set GroupID                                                                                |
|      `-v /ursim`      | The URSim application directory                                                            |
|    `-v /programs`     | The UR programs directory. This is accessible within the robot interface                   |
|       `-p 8080`       | Allows HTTP access to the robot interface. (Required for Browser Viewing)                  |
|       `-p 3389`       | Allows RDP access to the robot interface. (Optional. For VNC Viewers)                      |
|       `-p 502`        | Universal Robots Modbus Port. (Optional)                                                   |
|      `-p 29999`       | Universal Robots Dashboard Server Interface Port. (Optional)                               |
|      `-p 30001`       | Universal Robots Primary Interface Port. (Optional)                                        |
|      `-p 30002`       | Universal Robots Secondary Interface Port. (Optional)                                      |
|      `-p 30003`       | Universal Robots Real-time Interface Port. (Optional)                                      |
|      `-p 30004`       | Universal Robots RTDE Interface Port. (Optional)                                           |

The volume and port parameters are separated by a colon and indicate `<external>:<internal>` respectively. For example, `-p 8080:80` would expose port `80` from inside the container to be accessible from the host's IP on port `8080` outside the container.

------

This container was built in my free time to aid with research work. Please feel free to open any issues.

**All rights of the offline simulator application belong to [Universal Robots A/S](https://www.universal-robots.com).**

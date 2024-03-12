---
layout: post
title: Running OpenGL Applications Remotely Made Easy. The VirtualGL and TurboVNC Docker Image
categories: [tutorial, docker, opengl]
---

In the world of software development, running applications that require graphical interfaces or hardware acceleration remotely has always been a challenge, especially when these applications need to interact with specialized hardware like GPUs. This is where VirtualGL and TurboVNC comes into play, making it significantly easier for developers and users to run such applications remotely, including complex software like ROS Gazebo.

Docker containers offer a portable and consistent environment for running graphically intensive applications, ensuring they operate the same across different systems and reducing compatibility issues. This technology, combined with the isolation, scalability, and ease of deployment Docker provides, makes it an indispensable tool for developers needing to deploy applications with graphical interfaces or hardware acceleration remotely.

In this post, I’m going to introduce the docker image allowing us to run OpenGL applications inside the docker container. 

## Introduction to the Docker Image

My Docker image, available on Docker Hub as [damanikjosh/virtualgl-turbovnc](https://hub.docker.com/r/damanikjosh/virtualgl-turbovnc), is built upon the solid foundation of the [nvidia/opengl](https://hub.docker.com/r/nvidia/opengl) image. It integrates [VirtualGL](https://www.virtualgl.org/), [TurboVNC](https://www.turbovnc.org/), and [noVNC](https://novnc.com/info.html), creating a solution for running graphically demanding applications in a Docker container. I made two tags of the image, one using Ubuntu 22.04: `virtualgl3.1-ubuntu22.04`. The other using Ubuntu 20.04: `virtualgl3.1-ubuntu20.04` tag.

## Getting Started

Before you can take advantage of this Docker image, there are a few prerequisites:

1. NVIDIA drivers must be installed on your host system.
2. The Docker and NVIDIA Container Toolkit should be installed. You can follow the installation instructions [here](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html).
3. VirtualGL must be installed on your host system. Let’s dive deeper on how to install VirtualGL on the host machine.

## VirtualGL Installation Steps

Installing VirtualGL on your host machine involves a few steps, including generating an `/etc/X11/xorg.conf` file tailored to your setup, setting up the VirtualGL server.

1. Check the PCI bus ID of the GPU.

```bash
nvidia-xconfig --query-gpu-info
```

```
Number of GPUs: 1

GPU #0:
  Name      : NVIDIA GeForce RTX 3060 Ti
  UUID      : GPU-bee95cfa-fd98-c87d-894d-c2e16dc913af
  PCI BusID : PCI:1:0:0     <-- This is the PCI bus ID

  Number of Display Devices: 0
```

Copy the text after `PCI BusID :`

2. Generate the/etc/X11/xorg.conf file using `nvidia-xconfig`

```bash
nvidia-xconfig -a --allow-empty-initial-configuration --virtual=1920x1200 --busid PCI:1:0:0
```

Make sure to change the `busid` using your configuration.

3. Shutdown the display manager.

If you are using Ubuntu Desktop, first switch to Terminal display by using key combination `ctrl` + `alt` + `F1` and login using your user credentials.

Then, run one of the command below depending on display manager installed. If you’re unsure, there is no harm running both.

``` bash
# For GDM
sudo systemctl stop gdm
# For LightDM
sudo systemctl stop lightdm
```

Once VirtualGL is installed, you'll configure the VirtualGL server and reboot the system to apply changes.

4. Install VirtualGL on host machine

```bash
export VIRTUALGL_VERSION=3.1
wget -qO /tmp/virtualgl.deb https://sourceforge.net/projects/virtualgl/files/${VIRTUALGL_VERSION}/virtualgl_${VIRTUALGL_VERSION}_amd64.deb/download | sudo dpkg -i /tmp/virtualgl.deb
```

5. Configure VirtualGL server

```bash
sudo /opt/VirtualGL/bin/vglserver_config
```

Then follow the prompts
```
1) Configure server for use with VirtualGL (GLX + EGL back ends)
2) Unconfigure server for use with VirtualGL (GLX + EGL back ends)
3) Configure server for use with VirtualGL (EGL back end only)
4) Unconfigure server for use with VirtualGL (EGL back end only)
X) Exit

Choose:
1

WARNING: Configuring this server for use with VirtualGL will disable the
ability to log in locally with a Wayland session.

Continue?
[Y/n]


Restrict 3D X server access to vglusers group (recommended)?
[Y/n]
n

Restrict framebuffer device access to vglusers group (recommended)?
[Y/n]
n

Disable XTEST extension (recommended)?
[Y/n]

... Creating /etc/modprobe.d/virtualgl.conf to set requested permissions for
    /dev/nvidia* ...
... Attempting to remove nvidia module from memory so device permissions
    will be reloaded ...
modprobe: FATAL: Module nvidia_drm is in use.
... Granting write permission to /dev/nvidia-caps /dev/nvidia-modeset /dev/nvidia-uvm /dev/nvidia-uvm-tools /dev/nvidia0 /dev/nvidiactl for all users ...
... Granting write permission to /dev/dri/card0 for all users ...
... Granting write permission to /dev/dri/renderD128 for all users ...
... Modifying /etc/X11/xorg.conf.d/99-virtualgl-dri.conf to enable DRI
    permissions for all users ...
... Modifying /etc/X11/xorg.conf to enable DRI
    permissions for all users ...
... Adding xhost +LOCAL: to /etc/gdm3/Init/Default script ...
... Creating /usr/share/gdm/greeter/autostart/virtualgl.desktop ...
... Disabling Wayland in /etc/gdm3/custom.conf ...

Done. You must restart the display manager for the changes to take effect.

IMPORTANT NOTE: Your system uses modprobe.d to set device permissions.  You
must execute 'modprobe -r nvidia_uvm nvidia_drm nvidia_modeset nvidia' with the
display manager stopped in order for the new device permission settings to
become effective.
```

Note: I disable the group restriction to simplify the installation. If you need to enable this feature, don’t forget to add the user inside the container to `vgluser` group.

6. Reboot the system

```bash
sudo reboot
```

## Running Your Applications

With everything set up, running graphical applications in a Docker container is straightforward. 
Use the docker image [damanikjosh/virtualgl-turbovnc](https://hub.docker.com/r/damanikjosh/virtualgl-turbovnc) to directly run the container, or if you prefer, build the Docker image from my [repository](https://github.com/damanikjosh/virtualgl-turbovnc-docker.git).

Customize your container's behavior with environment variables like `DISPLAY`, `VGL_DISPLAY`, `VNC_PASSWORD`, `VNC_RESOLUTION`, and `NOVNC_PORT` to fit your specific needs.

* `DISPLAY`: The display number to use for VNC. Default is :10.
* `VGL_DISPLAY`: The display number to use for VirtualGL. Default is :0.
* `VNC_PASSWORD`: The password for the VNC server. If not set, you will be prompted to enter a password when you run the container.
* `VNC_RESOLUTION`: The resolution of the VNC server. Default is 1280x800.
* `NOVNC_PORT`: The port on which the noVNC server will listen. Default is 8080.

Use the `vglrun <command>` inside the Docker container to launch your applications using VirtualGL.

In my configuration, the VirtualGL server is using display `:1` and VNC server is using display `:2`. To run the docker container I can run command

```bash
docker run --gpus all --rm -it --network=host -e DISPLAY=:2 -e VGL_DISPLAY=:1 damanikjosh/virtualgl-turbovnc:latest vglrun glxspheres64
```

```
Password: 
Verify:   
Would you like to enter a view-only password (y/n)? n
xauth:  file /root/.Xauthority does not exist

Desktop 'TurboVNC: joshua-ki:1 ()' started on display joshua-ki:1

Starting applications specified in /opt/TurboVNC/bin/xstartup.turbovnc
Log file is /root/.vnc/joshua-ki:1.log

Polygons in scene: 62464 (61 spheres * 1024 polys/spheres)
GLX FB config ID of window: 0xad (8/8/8/0)
Visual ID of window: 0x21
Context is Direct
OpenGL Renderer: NVIDIA GeForce RTX 3060 Ti/PCIe/SSE2
728.220730 frames/sec - 812.694335 Mpixels/sec
772.531431 frames/sec - 862.145077 Mpixels/sec
733.322725 frames/sec - 818.388161 Mpixels/sec
697.386022 frames/sec - 778.282801 Mpixels/sec
725.497405 frames/sec - 809.655104 Mpixels/sec
```

Flag descriptions:
- `--gpus all` : Attaching host GPUs into the docker container
- `--rm` : Remove the docker container on exit
- `-it` : Allowing interactive terminal and run in background 
- `-e DISPLAY=:2` : Setting the VNC server to use display `:2`
- `-e VGL_DISPLAY=:1` : Setting the display where VirtualGL run at
- `vglrun glxspheres64` : Running OpenGL benchmarking application to check the configuration and test the performance.

Using the default configuration, docker container can be accessed using browser on port `8080`. After logging in using the VNC password, you can see the `vglspheres64` running.

![Screenshot](/assets/image/posts/screenshot.png)

## Conclusion

The VirtualGL and TurboVNC Docker image opens up new possibilities for running OpenGL applications remotely with ease. Whether you're a developer needing to access graphical applications on remote servers or someone looking to run high-performance computing tasks, this Docker image provides a reliable and efficient solution. By bridging the gap between remote applications and GPU acceleration, it ensures that your graphical applications run smoothly, no matter where you are.

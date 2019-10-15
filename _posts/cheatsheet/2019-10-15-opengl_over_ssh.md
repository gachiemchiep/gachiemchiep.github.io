---
title: "run opengl application over ssh"
excerpt: "How to run opengl application over ssh using virtualgl."
categories: 
  - cheatsheet
tags: 
  - opengl
  - ssh
published: true
comments: true
toc: true
support: true
order: 9
author: vugia.truong
---


# Run OpenGL application over ssh

## Problems

OpenGL application can not run through X11 forwarding feature of ssh. 
For example, when run an opengl application *realsense-viewer* through ssh using -X option, we'll face the following error:

```bash
# Log in
$ ssh -X jil@192.168.11.22

# Run realsense-viewer
$ realsense-viewer 
libGL error: No matching fbConfigs or visuals found
libGL error: failed to load driver: swrast
OpenGL 3.0 or ARB_vertex_array_object extension required!
```

## How to run opengl application through ssh

### Quick answer

Use VirtualGL and TurboVNC

### Long answer

The're two approach to run opengl application through ssh : *Indirect rendering*  and *Server-Side 3D rendering*

[virtualGL](https://www.virtualgl.org/About/Background) explained these 2 approaches very well. Please go there if you need more information about these topics.

In short-word, *Indirect rendering* mean the 3d rendering will be occured on local machine while receiving command from server. 

![Indirect OpenGL Rendering Using GLX](https://virtualgl.org/pmwiki/uploads/About/indirectrendering.png)
FIGURE 1: Indirect OpenGL Rendering Using GLX

I try some below tutorials but still unable to make it work. 

1. [opengl-hardware-acceleration-through-remote-x11-ssh-connection](https://evpo.wordpress.com/2017/03/04/opengl-hardware-acceleration-through-remote-x11-ssh-connection/)
2. [X Window Systemを使ってリモートサーバでOpenGLなプログラムを走らせる方法](https://itakeshi.hatenablog.com/entry/2018/02/03/134644)
3. [how-to-install-opengl-in-ubuntu-linux.aspx](https://www.includehelp.com/linux/how-to-install-opengl-in-ubuntu-linux.aspx)

*Server-Side 3D rendering* mean the 3d will be rendered on server machine. Laterly only the result image is "screen captured" and foward to local machine. Up to now we have : *Out-of-Process (X Proxy)*, *In-process (GLX Interposing)*. *VirtualGL* is the software that allow us to do the *Server-Side 3D rendering*. The architecture is as follow: 

![aaa](https://virtualgl.org/pmwiki/uploads/About/vgltransport.png)
FIGURE 6: The VGL Transport: In-Process GLX Forking and Image Encoding

## Configuration detail

* We have 2 following machines

1. Server : ubuntu 16.04、Use to run opengl applications and 3d rendering
2. Client : ubuntu 18.04、Personal PC

* For each machine, the following software are added

| Machine |         Software          |                                                                   Link |
| ------- | :-----------------------: | ---------------------------------------------------------------------: |
| Server  | virtualgl_2.6.2_amd64.deb | [sourceforge](https://sourceforge.net/projects/virtualgl/files/2.6.2/) |
| Server  | turbovnc_2.2.3_amd64.deb  |  [sourceforge](https://sourceforge.net/projects/turbovnc/files/2.2.3/) |
| Client  | turbovnc_2.2.3_amd64.deb  |  [sourceforge](https://sourceforge.net/projects/turbovnc/files/2.2.3/) |
  

### Configure server machine

* Configure VirtualGL

```bash
# log as root
service lightdm stop
/opt/VirtualGL/bin/vglserver_config
# See link for how to answer question
# For maximum sevurity answer recommended answers
```

* Create file */etc/systemd/system/turbovncserver@.service* on server machine, which has content as follow:

```bash
[Unit]
Description=Remote desktop service (VNC)
After=vncserver@1.service

[Service]
Type=forking
User=gachiemchiep
PIDFile=/home/gachiemchiep/.vnc/%H:%i.pid
ExecStartPre=-/opt/TurboVNC/bin/vncserver -kill :%i > /dev/null 2>&1
ExecStart=/opt/TurboVNC/bin/vncserver -geometry 1280x800 :%i
ExecStop=/opt/TurboVNC/bin/vncserver -kill :%i

[Install]
WantedBy=multi-user.target
```

Note: Please be notify that, i already use the display *:1* (port 5901) for normal vnc server. So i use display *:2* for turbovnc

* Edit file *~/.vnc/xstartup.turbovnc* on server machine and add the following line to the end of file. Please check that xfce desktop was installed

```bash
xfce4-session &
```

* Enable turbovncserver service to be run on :2 display

```bash
systemctl enable turbovncserver@2.service
```

* Reboot

```bash
sudo reboot
```

### Configure server machine

No need further configuration

## How to use

From client Machinerun vncclient:

```bash
/opt/TurboVNC/bin/vncviewer 
```

Select address as {IP_ADDRESS}:{DISPLAY_NUM} (192.168.11.22:2)

![aaa](assets/images/opengl/01.png)

Note: for convinient, the password was set be as same as user password

After login, we have the following :

![bbb](assets/images/opengl/02.png)

From client Machine, open another terminal. Then use the following commands to run opengl applications.

```bash
# ssh -X {IP_ADDRESS}
ssh -X 192.168.11.22
# export DISPLAY=:{DISPLAY_NUM}
export DISPLAY=:2
# run opengl application
realsense-viewer
# or
glxgears
```

![ccc](assets/images/opengl/03.png)

## References

1. [virtualgl's background](https://www.virtualgl.org/About/Background)
2. [https://github.com/aancel/admin/wiki/VirtualGL-on-Ubuntu](https://github.com/aancel/admin/wiki/VirtualGL-on-Ubuntu)
3. [https://virtualgl.org/About/Introduction](https://virtualgl.org/About/Introduction)
4. [https://www.turbovnc.org](https://www.turbovnc.org)
5. [Setup VirtualGL and TurboVNC on Ubuntu for OpenGL forwarding
](https://gist.github.com/cyberang3l/422a77a47bdc15a0824d5cca47e64ba2)

End
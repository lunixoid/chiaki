Work In Progress
================
The switch applet is not ready yet.
There is a lot of stuff to fix to have a simple user friendly version.
Feel free to contribute, (do not hesitate).

Nintendo Switch build instructions
==================================
this project requires the devkitpro toolchain.
you can use your personal computer to install devkitpro
but the easiest way is to use the following container.

Build container image
---------------------
from the [script](../script/switch) folder.
Here I'm using podman (rootless), but you can use docker instead.
```
podman build --pull --no-cache switch_build .
```

Run container
-------------
from the project's [root folder](../)
```
podman run -it --rm \
	-v "$(pwd):/build" \
	-p 28771:28771 \
	localhost/switch_build
```

Build Project
-------------
```
./script/switch/build.sh
```

tools
-----
Push to homebrew Netloader
```
# where X.X.X.X is the IP of your switch
/opt/devkitpro/tools/bin/nxlink \
	-a 192.168.0.200 -s \
	./build_switch/switch/chiaki.nro
```

Troubleshoot
```
# replace 0xCCB5C with the backtrace adress (PC - Backtrace Start Address)
aarch64-none-elf-addr2line \
	-e ./build_switch/switch/chiaki \
	-f -p -C -a 0xCCB5C
```

Chiaki config file
------------------
(currently chiaki switch require a config file)
please create the **chiaki.conf** file.
this file must be copied with the chiaki.nro local folder.
```ini
# required: PS4-XXX (PS4 local name)
[PS4-XXX]
# required: lan PS4 IP address
host_ip = X.X.X.X
# required: sony oline id (login)
psn_online_id = ps4_online_id
# required: https://github.com/thestr4ng3r/chiaki#obtaining-your-psn-accountid
psn_account_id = ps4_base64_account_id
# optional(default 30): remote play fps (must be 30 or 60)
video_fps = 30
# optional(default 720p): remote play resolution (must be 1080p, 720p, 540p, 360p)
# 1080p is for ps4 pro only
video_resolution = 720p
```
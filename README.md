The installs themselves are pretty straight forward depending on distro

## After initial install

```sudo apt update && sudo apt upgrade -y```

> **_NOTE:_**  While there might be many distros a lot of the kernel uses are similar so I'll try to not be too specific. 
> What is generic is to check your device for what components they have: Especially Graphics devices which often can cause issues. 
> The above can take a few minutes giving you time to do some Google work.

```lspci -nnk ``` 
To see all devices being detected including network, other devices, etc

```lspci | grep -i vga```
For GPU specifically

The same can be done with ```lsusb``` if you're using weird connectors. 

```sudo reboot``` after the update and upgrade are done. 

## When first upgrade is done

Now we can take care of graphics: 
For intel it is most of time already integrated... As it's a often used in "dual" graphics 

```sudo apt install {nvidia-driver-XXX}```

For AMD it's a bit more fun: They are included in the Linux kernel due to their open-source. 
But you will need some tools for modern applications:

```sudo apt install mesa-vulkan-drivers mesa-utils``` 

> **_NOTE:_** It's best to go find on the official sites for compatibility:
> Device manufacturer (This gives you the full specs)
> Graphics provider (Specific version compatible with^^)

You will need to reboot again, then ```nvidia-smi``` to check or the mesa/vulkan equivalent.

## Keeping an install clean

Keep only essential packages, this both helps your system and makes you less vulnerable:

List all packages: It might seem like there is a lot of fluff depending on distro but if you're on a fresh install it's quite normal as it's all the system components.
This number is still 2-3x lower than a Windows counterpart.

```dpkg -l```

Check an individual problematic package: 

```dpkg -l | grep {package_name}```

Useful to check past pkg installs

```grep "install " /var/log/dpkg.log | tail```

## Update warnings ⚠️
These can happen quite frequently when one of the software providers you use didn't update to latest signatures. 
This will give you annoying errors

Here is how to fix it:

Removing
```sudo apt remove --purge {pkgs1} {pkgs2}```

To make sure dependencies are also removed
```sudo apt autoremove --purge```

----

Check the problematic package:

```grep -r "brave" /etc/apt/sources.list /etc/apt/sources.list.d/```

Remove it from sources list:

```sudo rm /etc/apt/sources.list.d/brave-browser-release.list```

Then run update to your packages lists again to remove the warning:
```sudo apt update```

If you 're still having issues with dependencies USE AT YOUR OWN RISK:

```sudo apt full-upgrade```

## Security 🔒

```sudo apt install ufw```
```sudo ufw enable```
```sudo ufw allow ssh``` Make sure to use keys :)

This will restrict incoming traffic, you can use a VPN for further anonimity. 

If you're running local servers: 
```sudo ufw allow 80/tcp``` 

To secure that further you can look into reverse proxies. 

## 🖥️ Remote desktop sessions

For some weird reason this can be quite annoying depending on the distro
One thing  I found to fix a lot of my issues is to create a second user with elevated privileges so that you can remote connect to it easily while keeping the device functional on it's own. 

```sudo apt install xrdp xserver-xorg-core```

```sudo systemctl enable xrdp```
```sudo systemctl start xrdp```

Check if it's running:

```sudo systemctl status xrdp```

```sudo adduser xrdp ssl-cert```

If not rebooting sometimes helps. 

If you have ufw enable make sure to ```sudo ufw allow 3389```
This is the standard port.

If you get a black screen you might need to ```echo "gnome-session" > ~/.xsession```
Depending on your desktop type. 

```ip addr show```

Then add the specific port 3389

Now this is where it usually fails because you're already in the session. To me the best is to make a second user for remote sessions:

```sudo adduser {rdpuser}```

Then give it privileges: ```sudo usermod -aG sudo {rdpuser}```

There you go :) You know how to make a good install. For cherry on the cake you can share a folder between all users (this could be your coding or media?)

```chmod 777 {/home/mainuser/shared-folder}```


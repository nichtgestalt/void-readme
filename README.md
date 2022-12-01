# void-readme

Void Linux is one of my favourite distribution; not because I figured it all out, but because I like to tinker with it. I like its speed when booting and installing applications, the way it manages services and that it is mostly independed.

Here you can find some 'How To's for Void Linux, in my case it is installed on an old Thinkpad T440p. Most of these should be applicable to other systems.

## Things to do after a (minimal) install

### Services

For my system I use the following services:
- dbus
- sshd
- elogind
- ufw
- polkitd
- NetworkManager
- Displaymanager (in my case: Sddm)
For Laptops:
- tlp

Enable those services by typing as root:
~~~
# ln -s /etc/sv/<service> /var/service/
~~~
Managing services is documented on the Void website so feel free to read along if there are questions.

## [T440p] Boot Error: Unknown key identifier 'zoom'

This is a quiete harmless error during boot, but in case you want to have it away, do the following:
- Copy the keyboard file to the acording direction:
~~~
# mkdir -p /etc/udev/hwdb.d/
# cp /usr/lib/udev/hwdb.d/60-keyboard.hwdb /etc/udev/hwdb.d/
~~~
- Then comment out all the lines that contain the word 'zoom' under the linovo/ibm section.
- After that run the following command:
~~~
# udevadm hwdb --update && udevadm control --reload-rules && udevadm trigger
~~~

## Automatic mounting of USB-Sticks

When installing the minimal iso of Void, chances are your USB-Sticks are not mounted automatically. To do so I configured my system like this:
1. If you haven't already enable the Dbus, Elogind and Polkitd services:
~~~
# ln -s /etc/sv/dbus /var/service/
# ln -s /etc/sv/elogind /var/service/
# ln -s /etc/sv/polkitd /var/service/
~~~
2. I'm using BSPWM (you can find my dotfiles on my Github as well), so I edited the file in ==/usr/share/xsessions/bspwm.session==:
~~~
Exec=dbus-run-session bspwm
~~~
You can put this in your .xinitrc file as well if you are using __startx__ for booting into your desktop.
3. Make sure your user is in the __storage group__.
4. Reboot.

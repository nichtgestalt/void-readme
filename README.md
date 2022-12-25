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

## Speed up Linux via GRUB

The thing I value the most about Linux is customize my system the way I want it to be. For that I set some Kernel parameters to make it even snapier. To do so you have to edit following file: /etc/default/grub
~~~
GRUB_CMDLINE_LINUX="rhgb quiet mitigations=off"
GRUB_CMDLINE_LINUX_DEFAULT="loglevel=0 console=tty2 udev.log_level=0 vt.global_cursor_default=0 nowatchdog"
# Following is for Intel only:
GRUB_CMDLINE_LINUX_DEFAULT="intel_idle.max_cstate=1 cryptomgr.notes initcall_debug intel_iommu=igfx_off no_timer_check noreplace-smp page_alloc.shuffle=1 rcupdate.rcu_expedited=1 tsc=reliable"
~~~

## Automatic mounting of USB-Sticks

When installing the minimal iso of Void, chances are your USB-Sticks are not mounted automatically. To do so I configured my system like this:
1. If you haven't already enable the Dbus, Elogind and Polkitd services:
~~~
# ln -s /etc/sv/dbus /var/service/
# ln -s /etc/sv/elogind /var/service/
# ln -s /etc/sv/polkitd /var/service/
~~~
2. I'm using BSPWM (you can find my dotfiles on my Github as well), so I edited the file in /usr/share/xsessions/bspwm.session:
~~~
Exec=dbus-run-session bspwm
~~~
You can put this in your .xinitrc file as well if you are using __startx__ for booting into your desktop.
3. Make sure your user is in the __storage group__.
4. Reboot.


## Automatic mounting of USB-Sticks

When installing the minimal iso of Void, chances are your USB-Sticks are not mounted automatically. To do so I configured my system like this:
1. If you haven't already enable the Dbus, Elogind and Polkitd services:
~~~
# ln -s /etc/sv/dbus /var/service/
# ln -s /etc/sv/elogind /var/service/
# ln -s /etc/sv/polkitd /var/service/
~~~
2. I'm using BSPWM (you can find my dotfiles on my Github as well), so I edited the file in /usr/share/xsessions/bspwm.session:
~~~
Exec=dbus-run-session bspwm
~~~
You can put this in your .xinitrc file as well if you are using __startx__ for booting into your desktop.
3. Make sure your user is in the __storage group__.
4. Reboot.



## [T440p] Suspend, Hibernate and closing the lid

By default the __apcid service__ and the __elogind service__ don't go well which each other. To prevent interfering you have to configure the /etc/elogind/elogind.conf file. Under [Login] uncoment every _Handle_ option and set it to _'ignore'_, for example:
~~~
[Login]
HandlePowerKey=ignore
...
~~~
Reboot your system and everything should work as desired. 

I personally like to suspend to RAM when pressing the power button. So I changed it behavior in /etc/acpi/handle.sh :
~~~
...
logger "PowerButton pressed: $2, suspending(changed by user)..."
#shutdown -P now
zzz
;;
...
~~~


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



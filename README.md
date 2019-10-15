![verified](https://badgen.net/badge/verified/ubuntu&nbsp;16.04/green)

# Create Virtual Screens and connect with NoMachine to it :)

In this Repo I would like to share straight forwarded information 
1. How to add multiple screen extensions
2. How to connect with the software [NoMachine](https://nomachine.com) after to those created screens.In case you will connect only to one screen the free version is good enough, but if you would like to connect from multiple devices to those Virtual screen you need [NoMachine Enterprise Desktop](https://www.nomachine.com/enterprise-desktop) (and it's not so expensive ;) )

I will try to explain it straight forward, but in any case if there are comments please let me know.
So I could try to help you out.

# 1. How to add multiple screen extensions
## 1.1 Check if you are ready to go
First we need to see if you can directly start with it. For this we need to see the output of the following command.
`xrandr`

    Screen 0: minimum 8 x 8, current 2520 x 1080, maximum 32767 x 32767
        eDP1 connected primary 1920x1080+0+0 (normal left inverted right x axis y axis) 340mm x 190mm
           1920x1080     60.01*+  59.93  
           1680x1050     59.88  
           1600x1024     60.17  
           1400x1050     59.98  
           1600x900      60.00    59.95    59.82  
           1280x1024     60.02  
           1440x900      59.89  
           1400x900      59.96    59.88  
           1280x960      60.00  
           1368x768      60.00    59.88    59.85  
           1360x768      59.80    59.96  
           1280x800      59.81    59.91  
           1152x864      60.00  
           1280x720      59.86    60.00    59.74  
           1024x768      60.00  
           1024x576      60.00    59.90    59.82  
           960x540       60.00    59.63    59.82  
           800x600       60.32    56.25  
           864x486       60.00    59.92    59.57  
           640x480       59.94  
           720x405       59.51    60.00    58.99  
           640x360       59.84    59.32    60.00  
        HDMI1 disconnected (normal left inverted right x axis y axis)

From this output please remember SCREEN 0 information because it will be useful. In my case it will be `eDP1`.
In case your above output is similar to what I got then please follow section 1.2 .
But in case you see some VIRTUAL as disconnected like below then you can jump to Section  1.3

    VIRTUAL1 disconnected (normal left inverted right x axis y axis)
    VIRTUAL2 disconnected (normal left inverted right x axis y axis)
    VIRTUAL3 disconnected (normal left inverted right x axis y axis)

 
## 1.2 Making Virtual screen available.
To enable virtual screens we need to do the following modification.
First check if following file `/etc/X11/xorg.conf.d/20-intel.conf` exist in case that not please create and edit it.
`mkdir /etc/X11/xorg.conf.d && vim /etc/X11/xorg.conf.d/20-intel.conf`
Please make sure you have the following content.

    Section "Device"
        Identifier "intelgpu0"
        Driver "intel"
        Option "VirtualHeads" "5" # This is the number of Virtual screens you want to make available. You can change this value
    EndSection

*Recommended* :  Reboot your Machine to make the change appear.
Make sure Virtuals exists after reboot with `xrandr`.

## 1.3 Setup your screen
To make this first step I use the command `gtf` to get the right values to be used in the next steps
> _Gtf_ is a utility for calculating VESA GTF modes. Given the desired horizontal and vertical resolutions and refresh rate (in Hz)  
  
Read more at: [https://www.commandlinux.com/man-page/man1/gtf.1.html](https://www.commandlinux.com/man-page/man1/gtf.1.html)

In my example I used the following resolutions `1280 800` and refresh rate`60` 
`gtf 1280 800 60`
and I got following values.
  

    # 1280x800 @ 60.00 Hz (GTF) hsync: 49.68 kHz; pclk: 83.46 MHz
      Modeline "1280x800_60.00"  83.46  1280 1344 1480 1680  800 801 804 828  -HSync +Vsync
We need this information for the next step`"1280x800_60.00"  83.46  1280 1344 1480 1680  800 801 804 828  -HSync +Vsync`

We're going to add new mode to xrandr
`xrandr --newmode "1280x800_60.00"  83.46  1280 1344 1480 1680  800 801 804 828  -HSync +Vsync`

Now we're going to attach the resolution to the virtual screen of our choose. In my example I am using VIRTUAL1.
`xrandr --addmode VIRTUAL1 1280x800_60.00`

So now we're ready to activate the VIRTUAL1 and add it on the right/left/above/below side of our current screen who is eDP1 (his information I have got from the xrandr command). With the flag --mode we are precising what resolution to use. Because you can repeat the steps before to add many other resolutions.

In my case I want to attach resolution 1280 x 800 to my Virtual1 screen and I want it to be on the right Side of my current screen.

`xrandr --output VIRTUAL1 --mode 1280x800_60.00 --right-of eDP1`

So with this part you have activated your Screen extension.
On the next step we're going to connect to them.

# 2 How to connect with NoMachine to those Screen.

As already stated in the beginning we can use the Free version to connect from 1 client to 1 server. If we would connect from multiple different devices to 1 server then it's recommended to use [NoMachine Enterprise Desktop](https://www.nomachine.com/enterprise-desktop).

In my case I would like to connect from multiple devices to 1 server so I use NoMachine Enterprise Desktop.

On the other devices from where I want to start the session I install, Free NoMachine version.
In my case I use 1 Android(left-of) device, 1 iPad(right-of). So in total 2 connection to my Laptop.
 
From my Clients I connect to my Server and in the NoMachine settings I choose option 'Display' and only show the screen I would like to see on this current Client, and I also go to option 'Input' and tick Show remote mouse Cursor. 


That's all I hope you will enjoy your Screen Virtual extension with NoMachine.

![Connection_with_nomachine](https://repository-images.githubusercontent.com/214096850/0554f780-ef99-11e9-8c9f-500185f85953)

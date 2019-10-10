
# Create Virtual Screens and connect with NoMachine to it :)

In this Repo I would like to share straight forwarded information 
1. How to add multiple screen extensions
2. How to connect with the software [NoMachine](https://nomachine.com) after to those created screens.

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

You will get something like this above. 
If yes you need to follow #1.2 
If you bellow this you see information like those 

    VIRTUAL1 disconnected (normal left inverted right x axis y axis)
    VIRTUAL2 disconnected (normal left inverted right x axis y axis)
    VIRTUAL3 disconnected (normal left inverted right x axis y axis)

 Jump to #1.3
 
## 1.2 Making Virtual screen available.
To enable virtual screens we need to do the following modification.
first check if following file `/etc/X11/xorg.conf.d/20-intel.conf` exist in case that not please create
`mkdir /etc/X11/xorg.conf.d && vim /etc/X11/xorg.conf.d/20-intel.conf`
and please add the following content inside

    Section "Device"
        Identifier "intelgpu0"
        Driver "intel"
        Option "VirtualHeads" "5" # This is the number of Virtual screens you want to make available. You can change this value
    EndSection

So please save the file and reboot to make the change appear.
Make sure Virtuals exists after reboot with `xrandr`.

Then you can follow step #1.3

## 1.3 Setup your screen
To make this first step I use the command `gtf` to get the right values to be used in the next steps
> _Gtf_ is a utility for calculating VESA GTF modes. Given the desired horizontal and vertical resolutions and refresh rate (in Hz)  
  
Read more at: [https://www.commandlinux.com/man-page/man1/gtf.1.html](https://www.commandlinux.com/man-page/man1/gtf.1.html)

In my example I used this resolutions `1280 800` and refresh rate`60` 
`gtf 1280 800 60`
and I got this value back
  

    # 1280x800 @ 60.00 Hz (GTF) hsync: 49.68 kHz; pclk: 83.46 MHz
      Modeline "1280x800_60.00"  83.46  1280 1344 1480 1680  800 801 804 828  -HSync +Vsync
So now we need to add this information for the next step`"1280x800_60.00"  83.46  1280 1344 1480 1680  800 801 804 828  -HSync +Vsync`

We're going to add new mode to xrandr
`xrandr --newmode "1280x800_60.00"  83.46  1280 1344 1480 1680  800 801 804 828  -HSync +Vsync`

After having done this we need to attach this to one of our Virtuals we create before to do so please proceed as following. In my example I am using VIRTUAL1.
`xrandr --addmode VIRTUAL1 1280x800_60.00`

So now we're ready to active the Virtual1 and add it in the right side of our current screen. This information can be used from xrandr.
`xrandr --output VIRTUAL1 --mode 1280x800_60.00 --right-of eDP1`

mac2xrandr
==========

Issues a xrandr command based on the network MAC address - good for LTSP environments with numerous monitor configs as the DE will override the LTSP monitor settings.



INSTALLATION:

If using LTSP fat clients, the scripts can be used as is. If using thin clients, mac2xrandr.desktop should be modified to execute "ltsp-localapps /usr/local/bin/mac2xrandr" instead (as the script must run locally). 

Files should be placed as follows: 
 - mac2xrandr -> /usr/local/bin/mac2xrandr - make sure it is executable: chmod +x /usr/local/bin/mac2xrandr
 - mac2xrandr.desktop -> /etc/xdg/autostart/mac2xrandr.desktop
 - sample_config -> /etc/mac2xrandr.conf - make any necessary changes as below. 


In an LTSP environment, all files should be placed within the client chroot: /opt/ltsp/i386/ or /opt/ltsp/amd64/ and the image needs to be updated afterwards (including after config file updates) with: ltsp-update-image -a i386




CONFIGURATION: 

In /etc/mac2xrandr.conf specify one operator per line. Allowable operators: 

 - mac
 - run
 - var
 - req
 - unreq


mac (required)

Specify the MAC address. You may specify multiple macs by separating with spaces or commas. Example: 
 - mac: 00:19:DB:65:F2:E2 80:1F:02:2F:4E:1F


run (required)

Command to run if MAC address is matched. Example: 
 - run: xrandr --output DVI-I-1 --auto --output VGA-1 --auto --left-of DVI-I-1


var (optional)

Set variables for repeated use in script. Example: 
 - var: xrandr_bin /usr/bin/xrandr


Now you can use it as follows: 
 - run: var[xrandr_bin] --output VGA --off;


req (optional)

Determine whether to run script or not based on current xrandr state. State is determined by xrandr -q, then transformed into format: output=state:widthxheight@rate; where: 

 - output - name of video output
 - state - connected or disconnected
 - width - width of current resolution in pixels
 - height - height of current resolution in pixels
 - rate - refresh rate


If we only want to run script if DVI-I-1 is connected: 
 - req: DVI-I-1=connected;


unreq (optional) 

The opposite of req - we only run if a certain xrandr is NOT true. Example: 
We DO NOT run the command if HDMI-0 is connected

 - unreq: HDMI-0=connected

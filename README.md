# tv_grab_file for Synology NAS
This is a modified version of <https://code.google.com/p/tv-grab-file/> for use on Synology NAS devices.

## What it is
tv_grab_file can be used to pull any XMLTV source and feed your XMLTV enabled PVR backend (e.g. Tvheadend) with EPG data.

Quoting the original Google Code repository:

> _Simple XMLTV grabber to "grab" a xmltv formatted file_
> 
> This XMLTV "grabber" is used to provide a way of loading a valid XMTV file into any application that supports the various XMLTV grabbers.
>
> It will respond to a small number of XMLTV options, just enough to pass as a valid XMLTV grabber, and it will just pass a valid XMLTV file to the requesting program.

## Why the modifications were necessary
The original source was written for Bash, but Synology's operating system (DSM) is an embedded Linux based on BusyBox and the built-in shell (ash) differs in terms of syntax and functions.

## Which versions are available

### 1. Local
This version behaves exactly like the original one: It reads a local XML file (e.g. output from [mc2xml](http://mc2xml.tk/)) and returns the EPG data.

### 2. Remote
This versions downloads a remote XML file (e.g. http://example.com/remote.xml) and returns the EPG data.

### 3. TimeFor.TV enabled
This version downloads a remote XML file from [TimeFor.TV](http://en.timefor.tv) (paid service), strips the `www.timefor.tv/tv/` part from `id`- and `channel`-attributes and then returns the EPG data. The last step is necessary because TVheadend contains a [bug](https://tvheadend.org/issues/1800) in its lastest release (3.4.27), which causes the EPG grabber to loose the association between XMLTV and PVR channels.

## How to install
1. Download the latest tv_grab_file that fits you best (don't forget to modify the TimeFor.TV / remote URL or local XML file location with a editor)

2. Copy the file to a share on your Synology NAS (e.g. `public`)

3. Enable SSH (DSM &rarr; Control panel &rarr; Terminal &rarr; Enable SSH)

4. SSH into your NAS  
`ssh root@192.168.1.2`

5. Create a symlink  
`ln -s /volume1/public/tv_grab_file /usr/bin/tv_grab_file`

6. Make the file executable  
`chmod +x /usr/bin/tv_grab_file`

7. Restart your PVR backend (e.g. Tvheadend) and update the configuration to use tv_grab_file

8. Assign the XMLTV EPG sources to your channel listing

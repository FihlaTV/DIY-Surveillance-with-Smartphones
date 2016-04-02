# DIY-Surveillance-with-old-smart-phones
> DIY hack for a complete CCTV solution using open source software and old smartphones.
----

## Open Source & Commercial Software Used:
- [Linux Deploy](https://github.com/meefik/linuxdeploy): Install complete linux distribution on Android Device
- [Zoneminder]
(https://zoneminder.com/): Suite of CCTV applications
- [IP Web Camera]
(https://play.google.com/store/apps/details?id=com.pas.webcam&hl=en): Turn your smartphone into a proper IP Camera

## Requirements:
- Couple of smartphones, 1 of which should be rootable so you can use it as the zoneminder server (mileage will vary depending on the smartphone - I Used Samsung Galaxy S4, Galaxy S2, Blackberry Z30 and HTC Panache, out of which S2 and S4 were the zoneminder servers)
- Mounts for the IP Camera Smartphones (I just used the cheap commercial ones for car dashboard)
- tech and linux saviness: you'll need to root your phone, possibly troubleshoot linux issues when installing Linux Deploy as well as the setup script, open up ports on your router if you want to access the web interface remotely, etc

## Why run the surveillance server on a smartphone?
- To avoid trusting 3rd party hosting services & having full control and flexibility
- Practically no cost and minimal energy consumption - just have usb cables charging the phones at all times!
- Because you can!

Disclaimer: Note that a smartphone is not meant to be run as a dedicated server and the CPU will likely be heating up with usage like this, monitoring is strongly advised! Use at your own risk!

## Steps:
1. Choose 1 of the android smartphones as the server: root it (I used Cyanogenmod), install Busy Boy app by meefik and Linux Deploy app.

2. Open Linux Deploy, install Debian container: https://github.com/meefik/linuxdeploy/wiki/Installing-Debian
  
  Set the following properties:
	- Installation path: /data/media/linux.img
	- Image Size: close to your devices max storage capacity
	- Select Component: SSH server alone is enough
	- Set GUI to off
	- Set Custom Mounts to on
	- Add mount points (use status option on main screen to view available mount paths)

3. Assign a static IP on each smartphone. enable ssh login without password (scp ~/.ssh/id_rsa.pub android@ip:~/.ssh/authorized_keys)

4. Start the Debian container on the server phone, SSH into it (ssh android@ip_address), sudo su, git clone this repo, chmod +x start.bash, edit config files in conf directory with your custom settings, and execute start.bash script to install zoneminder, dependencies, jobs, and common configurations.

5. Open Zoneminder in web browser at http://serverip/zm
<pre>
Click Options, 
	Click Images tab
		Check Is the (optional) cambozola java streaming client installed (?) 
		Click Save
	Click Paths
		Change PATH_ZMS from /cgi-bin/nph-zms to /zm/cgi-bin/nph-zms Click Save
		Optional: under Paths change PATH_SWAP to /dev/shm (puts this process in RAM drive) Click Sav
	Click Email
		Make sure OPT_EMAIL is checked
		EMAIL_SUBJECT and EMAIL_BODY are self explanatory
		In the EMAIL_ADDRESS field enter the email address you want to get these alarms
		Make sure NEW_MAIL_MODULES is checked
		EMAIL_HOST: put in localhost
		FROM_EMAIL: your email
Restart Zoneminder

6. Setup all smartphones running Ip Webcam as monitors. See setup guide [here](https://bkjaya.wordpress.com/2015/11/28/how-to-use-an-old-android-phone-as-an-ip-camera-on-zoneminder/) and general guide on zoneminder monitors [here](http://zoneminder.readthedocs.org/en/stable/userguide/definemonitor.html)

Optional: Setup port forwarding to access web console remotely

## Changelog:
* 15-Feb-2016 initial commit
* 25-Mar-2016 scripted email configurations
* 02-Apr-2016 added daily job to toggle night vision settings for all configured ip cameras

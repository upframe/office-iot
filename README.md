
# whosthere-iot
Whosthere Raspberry Daemon - This python script works as a presence detector that searches for known Upframe members' MAC addresses and translates them into our platform Office. We want Upframe members who arrive at Startup Lisboa to be automatically logged in so other members can know if they are reachable.

## Settings:

* Whosthere-daemon.py is executed instantly after boot
  * Network scans run every 30 seconds
  * A device is considered offline if it isn't detected within 15 minutes (30 rounds of the above scans)
* Checkwifi.sh is executed once every 5 minutes


## Instalation

```
git clone https://github.com/ulissesferreira/whosthere-iot.git
sudo nano /usr/local/bin/checkwifi.sh
```
Add the following to the checkwifi.sh:
```
ping -c4 IP_ADDRESS > /dev/null
if [ $? != 0 ]
then
  sudo /sbin/shutdown -r now
fi
```
After closing this new file:
```
sudo chmod 775 /usr/local/bin/checkwifi.sh
sudo crontab -e
```
Pick your favourite text editor (such as nano) and at the bottom of the file, 
under all of the comments, add:
```
 @reboot nohup sudo /usr/bin/python /$PathToScript$/whosthere-daemon.py 
 */5 * * * * /usr/bin/sudo -H /usr/local/bin/checkwifi.sh >> /dev/null 2>&1
 ```
We now have our python daemon plus another script to check if there is Wi-Fi avaliable. Both these scripts run at boot. If we ever lose Wi-Fi or detect some network problems the Raspberry will reboot and attempt to find devices again. 

Developed with :heart: by Upframe
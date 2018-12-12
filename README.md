# UnifiKPN
Use Unifi USG Gateway with KPN with support for IPTV. This repository contains my basic config you can load in your Controller to provision your USG to use the KPN internet and IPTV. I am no 'Unifi' expert and this repository is mainly for my own reference, but I do hope it will help people that want the same setup.

### Requirements
* You have KPN FTTH internet connection
* A Unifi USG P3 (my JSON is for the P3)
* One ore more Unifi Switches that support IGMP Snooping

### DNS
I've added Cloudflares public DNS (1.1.1.1 and 1.0.0.1) to the JSON for speed and security. If you don't wan't that and want to use the DNS from KPN remove or modify the following lines in the JSON:

```
    "system": {
        "name-server": [
            "1.1.1.1",
            "1.0.0.1"
        ]
    },
```

### My hardware setup
```
                 KPN FTTH
                    |
                   USG
                    |
               Unifi Switch 8P
                 |
    Unifi Switch 8 (Living room)
               |
          TV Decoder
```
Just make sure that any switches you use between the USG and the decoder supports IGMP Snooping.

### Setup with the JSON 
1. Adopt the USG in your Unifi network trough your controller.
2. In the controller go to Settings, Networks and edit LAN to enable IGMP Snooping
2. After adoption, connect trough your controller with SFTP.
3. Upload the JSON file to the folder /srv/unifi/data/sites/default/
4. Force provision of the USG, and after provision do a restart to get rid of any old data (not necessary, but better save then sorry!)
5. Restart any switches you have to make sure the reconnect within the DHCP range of the USG and get the latest IGMP snooping configuration.

### Next: get the IPTV router
1. After your USG connects to the internet (if all goes well) connect to it trough SSH
2. After login, use the following command: `show dhcp client leases interface eth0.4`
3. This will show a table of data. Copy the IP address next to "router:"
4. Modify the JSON file and edit line 95 with the address you copied. This is your local KPN router and need to be updates to use IPTV. And resave it to your controller.
5. Reprovision the USG

### Finish: restart decoder
After the steps above, make sure you restart your decoder. After a while it should boot up with TV.


## Common things that may go wrong

#### The TV signal hangs or is unstable
Reprovision or restart the USG. If this does not fix it run `sudo igmpproxy restart`.

#### TV signal suddenly gone
KPN probably changes the IP of your local TV router. Follow the steps at 'get the IPTV router' again.

#### No internet or response from the USG
Make sure you have a Unifi USG 3P. The ports we are matching in the JSON correlate to this model. The other USG models have other ports and you probably should change the JSON for the right ports.

#### Anything else or still not working
Restart everything. The USG, the FTTH modem, your Switches and Controller... and try it all again if it still doesn't work.

#### Something else...
I've probably spend over 12 hrs to get it stable. Run a lot of configs, reprovisionings and factory reset. It's part of the proces and it helps you to understand what is happening. There is no 'plug and play' solution for this... But here are some handy (dutch) links for help and reference:

* https://gathering.tweakers.net/forum/list_messages/1878543
* https://gathering.tweakers.net/forum/list_messages/1883441
* https://free2wifi.nl/2018/09/25/ubnt-usg-iptv/
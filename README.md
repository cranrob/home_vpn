## Step 1: Get the Raspberry Pi OS onto microSD card (Thanks to Muzamal)

1. Download Raspberry Pi Imager from [https://www.raspberrypi.org/downloads/](https://www.raspberrypi.org/downloads/)
2. install and launch Raspberry Pi Imager. 
    ![pi1](https://raw.githubusercontent.com/A3XX/dns_at_home/master/img/1.PNG)
3. Click on choose OS and select Raspberry Pi OS 32bit.
    ![pi1a](https://raw.githubusercontent.com/A3XX/dns_at_home/master/img/1a.png)
4. Select the micro SD card.
    ![pi2](https://raw.githubusercontent.com/A3XX/dns_at_home/master/img/2.PNG)
5. Make sure you have selected the correct drive, click Yes.
    ![pi3](https://raw.githubusercontent.com/A3XX/dns_at_home/master/img/3.PNG)
6. Confirm and continue. 
    ![pi4](https://raw.githubusercontent.com/A3XX/dns_at_home/master/img/4.PNG)
7. Once completed disconnect the microSD card and reconnect. create a blank **SSH** file wihout any extension on the microSD card on main directory. Make sure extension is not **.txt**,filename should be **SSH** not ssh.txt.
    ![pi5](https://raw.githubusercontent.com/A3XX/dns_at_home/master/img/5.PNG)
8. Safely remove the microSD card, insert the card into your Raspberry Pi. 

## Step 2: Connect Raspberry Pi to your Home network (Thanks to Muzamal)
1. Connect Ethernet cable and power adapter, there is no ON/OFF switch it will automatically power on.  
    ![step2p1](https://projects-static.raspberrypi.org/projects/raspberry-pi-getting-started/0e07cfe2a142a41e6c97611e94057de6dddde935/en/images/pi-plug-in.gif)
2. Figure out your RPi's IP address, there are several methods.
  - a. Pinging the default Raspbian hostname. open command prompt or terminal and type **ping raspberrypi**. You get the IP address. (Picture shows IPv6)
      ![pic6](https://raw.githubusercontent.com/A3XX/dns_at_home/master/img/6.PNG)
  - b. Check your router's DHCP lease page, you will need login/password for the router. 
  - c. Connect your Raspberry Pi to TV via HDMI, attached keyboard,use defaultusername:**pi** and password:**raspberry** and type**ifconfig** and enter.
3. Once you have the IP address 
  -a. On Windows pc open Putty and enter the IP address and clock oepn, a prompt will open click yes, default login as **pi** and  password **raspberry** and enter.
      ![pic7](https://raw.githubusercontent.com/A3XX/dns_at_home/master/img/7.PNG)
  -b. On Mac or Linux pc open terminal and connect ssh connection, replace IP address with your Raspberry Pi's IP addess.
     ```bash
     ssh pi@192.168.1.113
     ```
4. We will do most of our configurations using a handy tool called “raspi-config”.
    ```bash
    sudo raspi-config
    ```
 - a. Select 8 and enter, this will update the raspi-config tool. 
 - b. Select 1 and enter, change the default password. 
 - c. It's a good idea to change your timezone in Localization settings
 
5. Run the following command on terminal
    ```bash 
     sudo apt update && sudo apt full-upgrade
    ```
6. Reboot your Pi

## Step 3: Set up Unattended Upgrades
Your Rapberry Pi will be exposed to the Internet so it's important that it stays up-to-date!

1. Install the Unattended Upgrades package
    ```bash
    sudo apt install unattended-upgrades
    ```

2. If you wish, you can do some customization including having your system send out an upgrade log via email whenever an upgrade is done (see docs)

3. Activate Unattended Upgrades
    ```bash
    sudo dpkg-reconfigure --priority=low unattended-upgrades
    ```
    
## Step 4: Obtain a DNS Name for your Home Network
1. Get a free account at [FreeDNS](https://freedns.afraid.org/) (afraid.org)

2. Pick a name for your network (subdomain) and choose one of their many domain names. e.g., _robsnet.hack-house.com_

3. Keep your IP address up to date
- a. On the FreeDNS site, click on "Dynamic DNS" on the laft panel
- b. Near the bottom of the page, click on "Quick cron example"
- c. Copy the last line of the displayed file (line may span more than one line)

>`1,6,11,16,21,26,31,36,41,46,51,56 * * * * sleep 10 ; wget -O - http://freedns.afraid.org/dynamic/update.php?SecretCodeShhhh >> /tmp/freedns_robsnet_hack-house_com.log 2>&1 &`

- d. On the Pi, run `crontab -e` and paste the line at the end of the file

## Step 5: Install PiVPN
If you don’t already have a static IP address, the install script will assist you in setting that up. We'll be installing a Wireguard VPN.
1. Install required prerequisite packages
   ```bash
   sudo apt install -y dnsutils iptables-persistent
   ```
2. Run the download and install script for PiVPN
   ```bash
   curl -sSL https://install.pivpn.io | bash
   ```
3. When presented choose Wireguard (not OpenVPN)

4. You’ll be able to change the default port if you wish (51820)

5. For DNS provider, use “PiVPN-is-local-DNS” if running Pi-Hole. If not, you can point at your router or an external provider.

_Note: PiVPN should automatically detect a PiHole DNS on your Pi and give you the option of selecting PiHole DNS as your provider._

6. On the "Public IP or DNS" screen, select "DNS Entry" and enter the DNS name you chose in Step 4.

## Step 6: Prepare your VPN Client Configurations
It is recommended to create separate client identities for each device that may use your VPN. This way if a device is lost or the client key is compromised, you won't have  to change the keys for the rest of the devices.
1.   For each client, run
   ```bash
   pivpn add
   ```
2. A Wireguard configuration file will be placed in the _configs_ directory under your home directory.
3. In the next step you will use the configuration file to configure each client.
4. Phones can be configured by generating a QR code on the Pi:
    ```bash
    pivpn -qr <clientID>
    ```
![QR-pic](https://github.com/cranrob/home_vpn/raw/main/IMG/vpn-qr.jpg)

## Step 7: Set Up Port Forwarding On Your Router
This step will vary depending what brand of router you have. [This page](https://www.noip.com/support/knowledgebase/general-port-forwarding-guide/)
has information on how to configure port forwarding on popular routers.

You will need to forward UDP Port 51820 (or the port you selected during setup) to the IP address of your Raspberry Pi.

## Step 8: Install the Wireguard VPN Client on your devices and configure them
1. Wireguard Client is available for Windows, MacOS, IOS, Android, and manhy other platforms.
Check [the wireguard site](https://wireguard.com/install) for links.
2. On phones, you can add a configuration using the option "Create from QR Code"
3. On computers, you have to "Import tunnel from file" and give it the configuration file generated  in the previous step.

## Step 9: Enjoy!
# Basic Raspberry Pi Setup thanks to Muzamal Abadullah
## Step 1: Get the Raspberry Pi OS onto microSD card

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

## Step 2: Connect Raspberry Pi to your Home network
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
 - c. Run the following command on terminal
    ```bash 
     sudo apt update && sudo apt full-upgrade && sudo apt autoremove && sudo apt autoclean   
    ```
 - d. Assign a static IP address, check free IP via pining or DHCP lease on the router. for demo I am assigning 192.168.1.252
    ```bash 
     sudo nano /etc/dhcpcd.conf
    ```
 - e. Scroll to the end of the file and change the following lines according to your network setup for a static IP.
    ```bash 
     # Example static IP configuration:
     interface eth0
     static ip_address=192.168.1.252/24
     #static ip6_address=fd51:42f8:caae:d92e::ff/64
     static routers=192.168.1.1
     static domain_name_servers=192.168.1.1
    ``` 
- f. Save the changes by pressing ctrl + x keys, then press y and enter. then enter 
    ```bash
     sudo reboot
    ```
- g. Open a new SSH connection using new static IP we just assigned.


# Home Automation using Home Assistant

## Building out home automation
As a part of a cloud computing practicum, a team of six came together to build a home automation process using Home Assistant (HA) for three specific use cases: locks, curtains, and ventilation. We started testing with lights, so we kept those scripts in for fun. 

Our four primary objectives
- Create a HA server deployed via docker ideally in the cloud. 
- Implement smart curtains to use with black out curtains. 
- Devise a solution for a smart central ac/heat system. 
- Integrate with Google , so our client can use Google Assistant to invoke voice commands, including running HA routines (e.g. "Go to bed," which checks if front door is locked and if it is skips trying to lock it).

Use our work as a foundation to build out your own home automation by adding additional use cases. At this point, in the world of home automation IoT, the sky is the limit: lights, locks, solar panels, televisions, alarms, cameras, vents, curtains, blinds, switches, sprinklers, etc.

## Getting Started
Though most configuration sources found online highly recommend running HA on a local server (Raspberry Pi), our assignment was to deploy to a cloud service. As such, our CI/CD implementation is optimized for the cloud and automatically deploying repository changes to our AWS EC2 instance.

---> ToDo: find explanatory link




# Installation

## Prep
- Required Equipment
  -  [Raspberry Pi](https://www.adafruit.com/product/4295) , running ubunutu or similar
  -  [Kasa Smart Plugs](https://www.amazon.com/TP-LINK-HS103P2-Required-Google-Assistant/dp/B07B8W2KHZ?ref_=ast_sto_dp&th=1&psc=1)
  -  [Vent Damper](https://m.supplyworks.com/#/sku/311744680)
  -  [Smart Lock](https://store.google.com/product/nest_x_yale_lock?hl=en-US)
  -  [ZemiSmart Smart Curtain](https://www.zemismart.com/products/-bcm500ds-tyw)
- Required Accounts
  - [aws cloud services](https://aws.amazon.com/)
  - tplink Kasa (mobile application account)
  -  [Google Assistant](https://www.google.com/)
  -  [duck dns](https://www.duckdns.org/)

- We also reccomend downloading [this sheet]() for storing all information you will be copying and/or pasting in this process, and inputing the following information:
  - <pi IP>
  - <network IP>

## Deploying Home Assistant
### Instance Launch
1. Log into [aws dashboard](https://aws.amazon.com/). Select ec2 from the services menu and click launch new instance.
2. Select and record <instance name>
3. Select the 'My AMIs' tab, click 'Shared With Me' and choose 'HA_AwsUbuntuDocker'
4. Create and save new pem key, noting <key name> and <key location>
5. Create new security group, record <sg name>
6. Keeping the first security rule, add a second rule allowing custom tcp access to port 8123 from anywhere
7. Launch instance. On the following page, click 'connect to instance.' On this page, record the listed public IP as <HA IP>
### Instance Setup
8. Open the terminal locally. Navigate to the folder containing your key.
9.  Change the file permissions for the key using command `sudo chmod 600 <key name>`
10. Access the ec2's terminal with command `ssh -i <key name> ubuntu@<HA IP>`
11. Run the following 3 commands in succession:
    - `sudo apt update`  
    - `sudo apt upgrade` 
    - `sudo reboot` 
### Testing
12.  open your favorite web browser and navigate to <HA IP>:8123. You should be greeted by the Home Assistant login page
13.  login using the credentials below:
    - user: admin
    - password: homeautomation
14. Change the login information to your liking, recording <HA user> & <HA password>


## Connecting Smart Devices to Local Network
### Kasa Smart Plug
1. Use instructions provided by manufacturer
2. Navigate the mobile app to the device information page and record the <plug IP>

### Yale Smart Lock

### ZemiSmart Curtains

## Configure Wireguard VPN Tunnel
### Router Settings
1. Open your router settings and set port forwarding. This process will vary by router, but these are the important settings:
   - external port: 58133
   - internal port: 58133
   - internal IP: <pi IP>
### ec2 Wireguard setup
2. Open terminal and navigate to location of the pem key 
3. Access the ec2's terminal with command `ssh -i <key name> ubuntu@<HA IP>`
4. Install Wireguard using the command `sudo apt install wireguard`
5. Create private key by running `wg genkey > wg-aws.key`
6. Create public key by running `wg pubkey < wg-aws.key > wg-aws.pub`
7. View private key by running `cat wg-aws.key`
   - record as <aws private key> 
8. View public key by running `cat wg-aws.pub`
   - record as <aws public key>
9. Open a new terminal window locally. Do not close the terminal window connected to the aws instance.
### pi Wireguard setup
10. In the new terminal window, connect to the local pi device using the command `ssh <pi username>@<pi local IP>` and entering the device password
11. Install Wireguard using the command `sudo apt install wireguard`
12.  Create private key by running `wg genkey > wg-pi.key`
13. Create public key by running `wg pubkey < wg-pi.key > wg-pi.pub`
14. View private key by running `cat wg-pi.key`
   - record as <pi private key> 
15. View public key by running `cat wg-pi.pub`
   - record as <pi public key>
### ec2 tunnel configuration
16.  On your local machine, scroll to the section labelled <ec2 config> towards the bottom of the clipboard file
17. replace <ec2 private key>, <pi public key>, <network IP>, and <pi IP> with the corresponding recorded information. For any smartplugs used, add the <device IP>/32 to the allowedIPs list at the bottom (with each IP separated by a comma)
18. In your ec2 instance terminal, use your preferred text editor to create and edit a new Wireguard configuration file. Simply insert your favorite text editor in the place of 'vim' below
    -  `sudo vim /etc/wireguard/HA-tunnel.conf`
19. copy your edited <ec2config> text into the new document and save

### pi tunnel setup
20.  On your local machine, scroll to the section labelled <pi config> towards the bottom of the clipboard file 
21. replace <pi private key> and <ec2 public key> with the corresponding recorded information.
22. In your pi terminal, use your preferred text editor to create and edit a new Wireguard configuration file. Simply insert your favorite text editor in the place of 'vim' below
    -  `sudo vim /etc/wireguard/HA-tunnel.conf`
23. copy your edited <pi config> text into the new document and save

### Tunnel Startup

24. in your pi terminal, start the tunnel with the following command:
    - `sudo wg-quick up HA-tunnel`
25. in your ec2 instance terminal, start the tunnel with the following command:
    - `sudo wg-quick up HA-tunnel`  

### Testing 
26. in your ec2 instance terminal, ping your raspberry pi using the following command:
    - `ping <pi IP>`  
27. after a few seconds, press ctrl + c
28. if packets have no packets have successfully transmitted, check the troubleshooting section

## Connecting devices to Home Assistant

### Kasa Smart Plugs for Damper Control
1. navigate to Home Assistant dashboard at <HA IP>:8123
2. login using <HA user> & <HA password>
3. in the dashboard click settings -> devices and integrations
4. click add integration 
5. type 'kasa' into the search bar and select TP-Link Kasa Smart
6. paste <device id> of your smart plug and click submit
7. repeat 4-6 for each additional plug

### Checkout the Wiki page for more information. 
- [Home](https://github.com/brandondombrowsky/BastCastle/wiki) - Documention, videos, and products and how they work.
- [Wireguard VPN Setup](https://github.com/brandondombrowsky/BastCastle/wiki/Wireguard-VPN-Setup) - The nitty gritty of setting up your environment.


# Home Automation Using Home Assistant

## Building Out Home Automation
As a part of a cloud computing practicum, a team of six came together to build a home automation process using Home Assistant (HA) for three specific use cases: locks, curtains, and ventilation. We started testing with lights, so we kept those scripts in the repo for fun. 

Our four primary objectives:
- Create a HA server deployed via docker in the cloud. 
- Implement smart curtains to use with blackout curtains. 
- Devise a solution for a smart central HVAC system to control airflow to different zones. 
- Integrate with Google so client can use Google Assistant to invoke voice command, including running HA routines (e.g. "Go to bed," which checks front state and locks if needed, does nothing if locked).

Use our work as a foundation to build out your own home automation by adding additional use cases. At this point, in the world of home automation IoT, the sky is the limit: lights, locks, solar panels, televisions, alarms, cameras, vents, curtains, blinds, switches, sprinklers, so on and so forth.

## Getting Started
Though most configuration sources found online highly recommend running HA on a local server (Raspberry Pi; take a look at [this](https://community.home-assistant.io/t/home-assistant-in-the-cloud/436220/2) thread for an explanation as to why), our assignment was to deploy to a cloud service. As such, our CI/CD implementation is optimized for the cloud and automatically deploys repository changes to our AWS EC2 instance.


## Installation

### Prep
- Required Equipment
  - [Raspberry Pi](https://www.adafruit.com/product/4295) - running Ubunutu or similar
  - [Kasa Smart Strip Plug](https://www.amazon.com/TP-LINK-HS103P2-Required-Google-Assistant/dp/B07B8W2KHZ?ref_=ast_sto_dp&th=1&psc=1)
  - [Motorized Damper](https://suncourt.com/collections/automated-airflow-control-dampers)
  - [Yale Smart Lock](https://store.google.com/product/nest_x_yale_lock?hl=en-US)
  - [ZemiSmart Curtain Motor with Track](https://www.zemismart.com/products/-bcm500ds-tyw)
- Required Accounts
  - [AWS Cloud Services](https://aws.amazon.com/)
  - tplink Kasa (mobile application account) `What is this? Doesn't follow format.`
  - [Google Assistant](https://www.google.com/)
  - [Duck DNS](https://www.duckdns.org/)
- We also reccomend downloading `[this sheet]()` `What?` for storing all information you will be copying and/or pasting in this process, and inputing the following information:
  - `<pi IP>`
  - `<network IP>`

### Deploying Home Assistant

#### Instance Launch
1. Log into [AWS Dashboard](https://aws.amazon.com/). Select EC2 from the services menu and click launch new instance.
2. Select and record `<instance name>`.
3. Select the 'My AMIs' tab, click 'Shared With Me' and choose 'HA_AwsUbuntuDocker'.
4. Create and save new pem key, noting `<key name>` and `<key location>`.
5. Create new security group, record `<sg name>`.
6. Keeping the first security rule, add a second rule allowing custom TCP access to port :8123 from anywhere.
7. Launch instance. On the following page, click 'connect to instance.' On this page, record the listed public IP as `<HA IP>`.

![reference images](https://user-images.githubusercontent.com/73506948/224200068-04333ce1-e1dc-40ca-9b6d-50fd592f063b.png)

  
  
#### Instance Setup
8. Open the terminal locally. Navigate to the folder containing your key.
9.  Change the file permissions for the key using command `sudo chmod 600 <key name>`. 
10. Access the EC2's terminal with command `ssh -i `<key name> ubuntu@<HA IP>`.
11. Run the following 3 commands in succession:
    - `sudo apt update`  
    - `sudo apt upgrade` 
    - `sudo reboot` 

  ![reference images](https://user-images.githubusercontent.com/73506948/224200230-66b19bba-64e7-4679-96cd-018e800e2b7e.png)

  
#### Testing
12.  Open your favorite web browser and navigate to `<HA IP>`:8123. You should be greeted by the Home Assistant login page.
13.  Login using the credentials below:
    - user: admin
    - password: homeautomation
14. Change the login information to your liking, recording `<HA user>` & `<HA password>`.

### Connecting Smart Devices to Local Network

#### Kasa Smart Plug Strip
1. Use instructions provided by manufacturer.
2. Navigate the mobile app to the device information page and record the `<plug IP>`.

#### Yale Smart Lock
`Add something here?`

#### ZemiSmart Curtain Motor with Track
`Add something here?`

### Configure Wireguard VPN Tunnel

#### Router settings
1. Open your router settings and set port forwarding. This process will vary by router, but these are the important settings:

##### Example Configuration on TPLink Router
   - External port :58133 
   - Internal port :58133 
   - Internal IP: `<pi IP>` 
![router-ports](https://user-images.githubusercontent.com/38815390/224203819-95a9e31a-d55e-4864-a985-0aa4be1deb38.png)
  - Blue: local server (Pi) IP address
  - Red: Wireguard tunnel port
#### EC2 Wireguard setup
2. Open terminal and navigate to location of the pem key.
3. Access the EC2's terminal with command `ssh -i <key name> ubuntu@<HA IP>`.
4. Install Wireguard using the command `sudo apt install wireguard`.
5. Create private key by running `wg genkey > wg-aws.key`.
6. Create public key by running `wg pubkey < wg-aws.key > wg-aws.pub`.
7. View private key by running `cat wg-aws.key`.
   - Record as `<aws private key>`
8. View public key by running `cat wg-aws.pub`.
   - Record as `<aws public key>`
9. Open a new terminal window locally. Do not close the terminal window connected to the aws instance.

`--- todo: EC2 instance security rules (image; awaiting final "prod" changes to instance) ---`

#### Pi Wireguard setup
10. In the new terminal window, connect to the local Pi device using the command `ssh <pi username>@<pi local IP>` and entering the device password.
11. Install Wireguard using the command `sudo apt install wireguard`.
12. Create private key by running `wg genkey > wg-pi.key`.
13. Create public key by running `wg pubkey < wg-pi.key > wg-pi.pub`.
14. View private key by running `cat wg-pi.key`.
   - Record as `<pi private key>`
15. View public key by running `cat wg-pi.pub`.
   - Record as `<pi public key>`

![reference images](https://user-images.githubusercontent.com/73506948/224200404-9e979abe-7b9f-4bdf-a6fb-58a1e157f2b4.png)



#### EC2 tunnel configuration
16. On your local machine, scroll to the section labelled <ec2 config> towards the bottom of the clipboard file.
17. Replace <ec2 private key>, `<pi public key>`, `<network IP>`, and `<pi IP>` with the corresponding recorded information. For any smartplugs used, add the <device IP>/32 to the allowedIPs list at the bottom (with each IP separated by a comma).
18. In your EC2 instance terminal, use your preferred text editor to create and edit a new Wireguard configuration file. Simply insert your favorite text editor in the place of 'vim' below.
    -  `sudo vim /etc/wireguard/HA-tunnel.conf`
19. Copy your edited `<ec2config>` text into the new document and save.

#### Pi tunnel setup
20.  On your local machine, scroll to the section labelled `<pi config>` towards the bottom of the clipboard file.
21. Replace `<pi private key>` and `<ec2 public key>` with the corresponding recorded information.
22. In your Pi terminal, use your preferred text editor to create and edit a new Wireguard configuration file. Simply insert your favorite text editor in the place of 'vim' below.
    -  `sudo vim /etc/wireguard/HA-tunnel.conf`
23. Copy your edited `<pi config>` text into the new document and save.

![reference images](https://user-images.githubusercontent.com/73506948/224200508-f7cfadcd-e5b8-4a41-aebd-5cf8a9777132.png)

  
#### Tunnel Startup
24. In your Pi terminal, start the tunnel with the following command:
    - `sudo wg-quick up HA-tunnel`
25. In your EC2 instance terminal, start the tunnel with the following command:
    - `sudo wg-quick up HA-tunnel`  

#### Testing 
26. In your EC2 instance terminal, ping your Raspberry Pi using the following command:
    - `ping <pi IP>`
27. After a few seconds, press ctrl + c.
28. If packets have "no packets have successfully transmitted," check the troubleshooting section. 

`--- todo: duckdns + https ---`

### Connecting devices to Home Assistant

#### Kasa Smart Strip Plug for Damper Control
1. Navigate to Home Assistant dashboard at `<HA IP>`:8123. 
2. Login using `<HA user>` & `<HA password>`.
3. In the dashboard click Settings -> Devices and Integrations. 
4. Click "Add Integration." 
5. Type 'kasa' into the search bar and select TP-Link Kasa Smart.
6. Paste `<device id>` of your smart plug and click submit.
7. Repeat 4-6 times for each additional plug.

### Securing Devices in Home Assistant
Home Assistant has some wonderful (and simple) settings for securing device access. Take a look at some of them [here](https://github.com/brandondombrowsky/BastCastle/wiki/Editing-User-Permissions-in-Home-Assistant).

### Checkout the Wiki page for more information. 
- [Home](https://github.com/brandondombrowsky/BastCastle/wiki) - Documention, videos, and products and how they work.
- [Wireguard VPN Setup](https://github.com/brandondombrowsky/BastCastle/wiki/Wireguard-VPN-Setup) - The nitty gritty of setting up your environment.

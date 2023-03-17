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
  - Tuya Mobile App
  - Tuya Iot Platform
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
    
    
![Untitled22_20230314223409](https://user-images.githubusercontent.com/73506948/225216590-1fa80484-0376-44e8-8a8a-024969a81959.png)


#### Testing
12.  Open your favorite web browser and navigate to `<HA IP>`:8123. You should be greeted by the Home Assistant login page.
13.  Login using the credentials below:
    - user: admin
    - password: homeautomation
14. Change the login information to your liking, recording `<HA user>` & `<HA password>`.

### Connecting Smart Devices to Local Network

For each smart device it is best to refer to the manufacturer's installation process, as most include proprietary apps to connect to your local network. When connecting the Kasa smart plugs, you will need to record the '<device IP>' for each plug. To do this, find the device MAC adddress as listed in the Kasa app under the device info section of device settings.

![Untitled19_20230315231013](https://user-images.githubusercontent.com/73506948/225530664-cd28f826-b24b-487c-b8ec-5bfd6e963b91.png)

From there, use your router to cross reference the MAC address with the associated IP.

### Configure Wireguard VPN Tunnel

#### Router Settings
1. Open your router settings and set port forwarding. This process will vary by router, but these are the important settings:

##### Example Configuration On TPLink Router
   - External port :58133 
   - Internal port :58133 
   - Internal IP: `<pi IP>` 
![router-ports](https://user-images.githubusercontent.com/38815390/224203819-95a9e31a-d55e-4864-a985-0aa4be1deb38.png)
  - Blue: local server (Pi) IP address
  - Red: Wireguard tunnel port
#### EC2 Wireguard Setup
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
![Untitled14_20230314170134](https://user-images.githubusercontent.com/73506948/225216751-385fca6c-672f-45d8-9ea9-f586aa8c3717.png)

##### EC2 Security Group Rules
Inbound:
![ec2sec](https://user-images.githubusercontent.com/38815390/225781790-35b341ff-db82-4847-a8b5-83ff80796e7a.png)
Outbound: All traffic

#### Pi Wireguard Setup
10. In the new terminal window, connect to the local Pi device using the command `ssh <pi username>@<pi local IP>` and entering the device password.
11. Install Wireguard using the command `sudo apt install wireguard`.
12. Create private key by running `wg genkey > wg-pi.key`.
13. Create public key by running `wg pubkey < wg-pi.key > wg-pi.pub`.
14. View private key by running `cat wg-pi.key`.
   - Record as `<pi private key>`
15. View public key by running `cat wg-pi.pub`.
   - Record as `<pi public key>`

![Untitled14_20230314170039](https://user-images.githubusercontent.com/73506948/225216803-7178ed53-8589-423f-8aa1-2ef5c3c38961.png)


#### EC2 Tunnel Configuration
16. On your local machine, scroll to the section labelled <ec2 config> towards the bottom of the clipboard file.
17. Replace <ec2 private key>, `<pi public key>`, `<network IP>`, and `<pi IP>` with the corresponding recorded information. For any smartplugs used, add the <device IP>/32 to the allowedIPs list at the bottom (with each IP separated by a comma).
18. In your EC2 instance terminal, use your preferred text editor to create and edit a new Wireguard configuration file. Simply insert your favorite text editor in the place of 'vim' below.
    -  `sudo vim /etc/wireguard/HA-tunnel.conf`
19. Copy your edited `<ec2config>` text into the new document and save.

![Untitled21_20230314223146](https://user-images.githubusercontent.com/73506948/225218278-52efdfd9-dd4f-45fe-9387-8887bb2c0ccc.png)




#### Pi Tunnel Setup
20.  On your local machine, scroll to the section labelled `<pi config>` towards the bottom of the clipboard file.
21. Replace `<pi private key>` and `<ec2 public key>` with the corresponding recorded information.
22. In your Pi terminal, use your preferred text editor to create and edit a new Wireguard configuration file. Simply insert your favorite text editor in the place of 'vim' below.
    -  `sudo vim /etc/wireguard/HA-tunnel.conf`
23. Copy your edited `<pi config>` text into the new document and save.

![Untitled21_20230314223106](https://user-images.githubusercontent.com/73506948/225218372-a415684e-9077-465e-84db-8f60f148e833.png)

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

### Connecting Devices to Home Assistant

#### Kasa Smart Strip Plug for Damper Control
1. Navigate to Home Assistant dashboard at `<HA IP>`:8123. 
2. Login using `<HA user>` & `<HA password>`.
3. In the dashboard click Settings -> Devices and Integrations. 
4. Click "Add Integration." 
5. Type 'kasa' into the search bar and select TP-Link Kasa Smart.
6. Paste `<device id>` of your smart plug and click submit.
7. Repeat 4-6 times for each additional plug.

If adding new devices after 1st install, you will also need to add their local IPs to the EC2 instance's wireguard config file. Follow the steps from EC2 configuration, but instead of copying and pasting the whole document, simply add the device's local IP to the list of allowed IPs towards the bottom of the document.

#### ZemiSmart Curtain Motor
1. Log into Tuya IoT platform account. Select "Cloud" from left toolbar and click "Create Cloud Project" on the following screen.
2. Fil in the Create Cloud Project form as follows:
    `Project Name: Home Assistant`
    `Description: optional`
    `Industry: Smart Home`
    `Development Method: Smart Home`
    `Data Center: Western America Data Center`
3. Click "Create" button. Skip the configuration wizard and click Authorize.
4. Record `<Access ID>`, `<Access Secret>`, and `<Project Code>`

![Untitled17_20230314000248](https://user-images.githubusercontent.com/73506948/225218610-62a9c3dc-131f-4ac4-860b-138388bb2b67.png)


5. Navigate to the "Devices" tab. Select "Link Tuya App Account" and click "Add App Account." A barcode should appear.
6. Open the Tuya App on your mobile device. Click the "Me" tab on the bottom navigation bar and the barcode scan button on the resulting page

![6 TuyaMobile](https://user-images.githubusercontent.com/73506948/225525199-c3629973-88d8-401d-85c1-6067d1b999fd.jpeg)


7. Scan the barcode from step 5. A dialoue box will replace the barcode. Set "Device Linking Method" to Automatic Linking and "Device Permission"  to Read, write, and Manage. Press "OK"
8. Wait a moment. When the product name appears listed under devices, you will now the process has been successful.

![reference image](https://user-images.githubusercontent.com/73506948/225526847-730101a2-41e8-4928-9367-ae33229036b2.png)

When the product name appears listed under devices, you will now the process has been successful.

!reference image](https://user-images.githubusercontent.com/73506948/225527622-cc278b72-25e9-4c3e-abe9-eaee2cf7786e.png)



9. Navigate to Home Assistant dashboard at `<HA IP>`:8123. 
10. Login using `<HA user>` & `<HA password>`.
11. In the dashboard click Settings -> Devices and Integrations. 
12. Click "Add Integration." 
13. Type 'tuya' into the search bar and select Tuya.
14. Paste `<Access ID>` and `<Access Secret>` of your curtains as well as your Tuya Mobile App login credentials. Click submit. Be patient, registration may take up to a minute.

![reference image](https://user-images.githubusercontent.com/73506948/225218506-8ccca51f-1db5-49cd-bde4-e61a142cc78c.png)

15. Select the proper area for your curtains

#### Yale Smart Lock `-----> ToDo <----------`

### Google Integration
Home Assistant is compatible with Google Home/Assistant. This can be configured both ways: HA can be integrated into GH/A so that HA scripts can be run from GH/A and HA can send command requests to GH/A. Unfortunately, the process has too many variables for a succinct walkthrough; instead take a look at how this process works and view helpful guides[in our wiki](https://github.com/brandondombrowsky/BastCastle/wiki/Google-Integration).

### Securing Devices in Home Assistant
Home Assistant has some wonderful (and simple) settings for securing device access. Take a look at some of them [here](https://github.com/brandondombrowsky/BastCastle/wiki/Editing-User-Permissions-in-Home-Assistant).

### Checkout the Wiki Page for More Information. 
- [Home](https://github.com/brandondombrowsky/BastCastle/wiki) - Documention, videos, and products and how they work.
- [Wireguard VPN Setup](https://github.com/brandondombrowsky/BastCastle/wiki/Wireguard-VPN-Setup) - The nitty gritty of setting up your environment.

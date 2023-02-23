# Home Automation using Home Assistant

## Building out home automation
As a part of a cloud computing practicum, a team of six came together to build a home automation process using Home Assistant (HA) for three specific use cases: locks, curtains, and ventilation. We started testing with lights, so we kept those scripts in for fun. 

Our four primary objectives
- Create a HA server deployed via docker ideally in the cloud. 
- Implement smart curtains to use with black out curtains. 
- Devise a solution for a smart central ac/heat system. 
- Integrate with Google , so our client can use Google Assistant to invoke voice commands, including running HA routines (e.g. "Go to bed," which checks if front door is locked and if it is skips trying to lock it).

Use our work as a foundation to build out your own home automation by adding additional use cases. At this point, in the world of home automation IoT, the sky is the limit: lights, locks, solar panels, televisions, alarms, cameras, vents, curtains, blinds, switches, sprinklers, etc.

### TBD if you want to be featured ---> ToDo

## Getting Started
Though most configuration sources found online highly recommend running HA on a local server (Raspberry Pi), our assignment was to deploy to a cloud service. As such, our CI/CD implementation is optimized for the cloud and automatically deploying repository changes to our AWS EC2 instance.

---> ToDo: find explanatory link

## Installation 
1. Install HA locally. Though there are many ways to do this, these are the ones we found helpful:
    - Windows system was [here](https://www.youtube.com/watch?v=dp-0hVjEo6A). 
    - Mac ---> ToDo [here]. 
    - Linux [here](https://github.com/brandondombrowsky/BastCastle/wiki/Wireguard-VPN-Setup#install-docker) 
2. HA will scan for and find most of your devices. ---> ToDo check this
3. Fork the BastCastle repository or copy in the files you need. At a minimum, your configuration should include a .github/workflows and homeassistant.config folder, as a .gitignore file. ---> ToDo Alexi take a look at this
    - From the .github/workflows folder, copy in a minimum of the home-assistant.yml and yamllint.yml file
5. From the `homeassistant-config/` folder, copy in default HA files ---> ToDo what required default files?
6. Register for a cloud service (we're using AWS) if you don’t already have one. Skip this step for local server (Pi) installation.
7. Set up an EC2 instance (or alternative).
8. Test one of your devices to see if it responds as expected. ---> ToDo: what should we see/how should an average user test this? Maybe a dashboard photo with dets
9. Continue adding/configuring scripts until you live in the Jetson home ---> ToDo refine for fluff
    - ToDo: a few demos (curtains for sure!)

### Checkout the [Wiki](https://github.com/brandondombrowsky/BastCastle/wiki) page for more information. 

## Important Links
- [Home Assistant](https://www.home-assistant.io/) - Find out more about Home Assistant.
- [Frenck's CI](https://github.com/frenck/home-assistant-config) - Learn more about our chosen CI.
- [Custom yamllint](https://yamllint.readthedocs.io/en/stable/configuration.html) - We created our own linter. For more information about cusomizing our linter for your needs, check out this site.
- [Zemismart Curtain Track](https://www.zemismart.com/products/-bcm500ds-tyw) - The curtain rod we used.
- [Yale Smart Lock](https://shopyalehome.com/collections/smart-locks/products/yale-assure-lock-sl-with-wi-fi-and-bluetooth?variant=39341912981636) - The lock we used.
- Ventilation - We used smart switches to control vent power. ---> ToDo add product
- Lights - The lights we started out with. ---> ToDo add product

---> ToDo: link to Wiki on how the products work

---> ToDo: License??? Do we need one?

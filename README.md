# BastCastle Home Automation using Home Assistant

## Building out home automation
As a part of a cloud computing practicum, a team of six came together to build a home automation process using Home Assistant (HA) for three specific use cases: locks, curtains, and ventilation. We started testing with lights, so we kept those scripts in for fun. 

Our four primary objectives were to (1) create a Home Assistant server deployed via docker ideally in the cloud. (2) Implement smart curtains to use with black out curtains. (3) Devise a solution for a smart central ac/heat system. (4) Ideally integrate with Google Assistant, so our client can use Google Assistant to invoke voice commands, including running HA routines (e.g. "Go to bed," which checks if front door is locked and if it is skips trying to lock it).

Use our work as a foundation to build out your own home automation by adding additional use cases. At this point, in the world of home automation IoT, the sky is the limit: lights, locks, solar panels, televisions, alarms, cameras, vents, curtains, blinds, switches, sprinklers, etc.

### Contributing Developers
- Alexi Most
- Lucas Knezevich
- Jason Beutler
- Victor Navarro
- Ashray Thapa
- Brandon Dombrowsky

## Getting Started
Though most configuration sources found online highly recommend running HA locally on a Pi-hole, our assignment was to deploy to a cloud service. As such, our CI/CD implementation is optimized for the cloud and automatically deploying repository changes to our AWS EC2 instance.

## Installation 
1. Install Home Assistant locally. Though there are many ways to do this, the one I found the simplest for a Windows system was [here](https://www.youtube.com/watch?v=dp-0hVjEo6A). Mac here. Home Assistant will scan for and find most of your devices.
2. Fork the BastCastle repository or copy in the files you need. At a minimum, your configuration should include a .github/workflows and homeassistant.config folder, as a .gitignore file.
3. From the .github/workflows folder, copy in a minimum of the home-assistant.yml and yamllint.yml file
4. From the homeassistant.config folder, copy in a minimum of a (what?)
5. Register for a cloud service if you don’t already have one. Skip this step for Pi-hole installation.
6. Set up an EC2 instance. Alternatively, install the forked repo locally onto a Pi-hole.
7. Test one of your devices to see if it responds as expected.
8. Continue adding/configuring scripts.
9. Once everything is working to your satisfaction, you are done.

### Checkout the [Wiki page](https://github.com/brandondombrowsky/BastCastle/wiki) for more information. 

## Important Links
- [Home Assistant](https://www.home-assistant.io/) - Find out more about Home Assistant
- [Frenck CI](https://github.com/frenck/home-assistant-config) - Learn more about our chosen CI 
- [Custom yamllint](https://yamllint.readthedocs.io/en/stable/configuration.html) - We created our own linter, for more information about cusomizing our linter for your needs, check out this site.
- [Zemismart Curtain Track](https://www.zemismart.com/products/-bcm500ds-tyw) - The curtains we used.
- [Yale Smart Lock](https://shopyalehome.com/collections/smart-locks/products/yale-assure-lock-sl-with-wi-fi-and-bluetooth?variant=39341912981636) - The lock we used.
- Vents - The vents we used.
- Lights - The lights we started with.

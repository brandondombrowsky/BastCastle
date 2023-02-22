# BastCastle Simple Home Automation using Home Assistant

## The haHA Gateway Drug
As a part of a cloud computing practicum, a team of six came together to build a home automation
process using Home Assistant for three specific use cases: locks, curtains, and vents. We started testing
with lights, so we kept those scripts in for fun. Further, we built our process in the cloud within an AWS
EC2 instance but optimized it for migration to a Pi-hole.

Take our blood, sweat, and tears and build upon this foundation. Add additional use cases of your own.
At this point, in the world of home automation IoT, the sky is the limit: lights, locks, solar panels,
televisions, alarms, cameras, vents, curtains, blinds, switches, sprinklers, etc.

WARNING: once you get started with haHA, you won't stop! 

### Contributing Developers
- [Alexi Most](https://www.linkedin.com/in/alyoshenka/) 
- [Lucas Knezevich](https://www.linkedin.com/in/lucasknezevich/)
- [Jason Beutler](https://www.linkedin.com/in/jasonpbeutler/)
- Victor Navarro (need his link)
- [Ashray Thapa](https://www.linkedin.com/in/ashray-thapa/)
- [Brandon Dombrowsky](https://www.linkedin.com/in/brandondombrowsky/)

## Getting Started
Though most configuration sources found online highly recommend running HA locally on a Pi-hole, our
assignment was to deploy to a cloud service. As such, our CI/CD implementation is optimized for the
cloud and automatically deploying repository changes to our AWS EC2 instance.

## Installation 
1. Install Home Assistant locally. Though there are many ways to do this, the one I found the simplest for
a Windows system was [here](https://www.youtube.com/watch?v=dp-0hVjEo6A). Mac here. Home Assistant will scan for and find most of your devices.
2. Fork the BastCastle repository or copy in the files you need. At a minimum, your configuration should include a .github/workflows and homeassistant.config folder, as a .gitignore file.
3. From the .github/workflows folder, copy in a minimum of the home-assistant.yml and yamllint.yml file
4. From the homeassistant.config folder, copy in a minimum of a (what?)
5. Register for a cloud service if you don’t already have one. Skip this step for Pi-hole installation.
6. Set up an EC2 instance. Alternatively, install the forked repo locally onto a Pi-hole.
7. Test one of your devices to see if it responds as expected.
8. Continue adding/configuring scripts.
9. Once everything is working to your satisfaction, you are done.

## Checkout the [Wiki page](https://github.com/brandondombrowsky/BastCastle/wiki) for more information. 

## Shouting Out Where Shouting Is Due
- [Home Assistant](https://www.home-assistant.io/)
- [Frenck CI](https://github.com/frenck/home-assistant-config)
- [Custom yamllint](https://yamllint.readthedocs.io/en/stable/configuration.html)
- [Zemismart Curtain Track](https://www.zemismart.com/products/-bcm500ds-tyw)
- [Yale Smart Lock](https://shopyalehome.com/collections/smart-locks/products/yale-assure-lock-sl-with-wi-fi-and-bluetooth?variant=39341912981636)
- Vents (add a link)
- Lights (add a link)

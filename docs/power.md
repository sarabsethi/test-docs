# Power

## Overview

Bugg's typical power draw is between 1-2W, with short peaks of up to 5W. 

Exact power usage numbers will depend on a number of use-case specific variables, including input voltage, recording settings, mobile signal strength, etc. 

Bugg devices should be powered via a DC input through the [M12 connector](hardware.md#power-connector) at the bottom of the device, or via USB (for testing only). 

## Batteries and renewables

Bugg can be powered from any battery designed to deliver a stable 7-36V DC output (e.g., car batteries, motorcycle batteries, off-grid power sources). 

However, to get long unattended deployments without extremely heavy batteries, we recommend the use of renewable off-grid power sources to continuously top-up batteries during deployments.

### Solar powered deployments

Solar power is the most common choice for powering long-duration Bugg deployments. 

Here we provide guidance for the basic elements needed to deploy a solar powered Bugg device for year-round 24/7 data collection from an uncovered area in the UK. 

When deploying devices in other locations (e.g., with longer or shorter daylight hours), or where there are occlusions to sunlight (e.g., from leaves in the canopy), adjustments will likely be necessary. 

You will first need to acquire the following equipment (in addition to the Bugg and its power cable):

* Solar panel (50-100W)
* Deep-cycle battery (24Ah, 12V)
* Solar charge controller
* Solar panel mounting hardware
* Box for battery and charge controller
* Spare wiring

<mark>TODO: finish how to do solar power</mark>


## USB 

For testing, Bugg devices can be powered by USB at 5V by connecting a USB cable to the [appropriate pins](hardware.md#pinout) of the M12 connector. 

However, this is potentially unstable and may result in a decrease in audio quality, so is not recommended for real use-cases.

Please ensure the USB power source can deliver at least 2A at 5V (e.g., a decent quality power bank).

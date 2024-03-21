# Audio

Bugg can record audio from either the integrated microphone, or (in v2) from an external microphone connected through the M12 connector.

## Integrated microphone

A MEMs mic is built into each Bugg device, which is weatherproofed by an acoustic vent, and interfaced through a horn on the device enclosure.

![The integrated Knowles SPH0641LU4H-1 and protective membrane](img/bugg-mems-mic-and-membrane.png){align=left, width=100%}

### Knowles SPH0641LU4H-1 

The integrated microphone in Bugg is a [Knowles SPH0641LU4H-1](https://www.digikey.co.uk/en/products/detail/knowles/SPH0641LU4H-1/5332438) PDM microphone. 

Full specifications are in the microphone's [documentation](https://mm.digikey.com/Volume0/opasdata/d220001/medias/docus/930/SPH0641LU4H-1.PDF), but a few key details are:

* Omnidirectional
* Supported sampling frequencies: 100 Hz - 80 kHz
* Sensitivity: -26dB ±1dB @ 94dB SPL
* Signal to noise ratio: 64.3dB

### Acoustic membrane  
Between the microphone and the outside world is a breathable ePTFE membrane (similar to [GORE MEMS Protective Vents](https://www.gore.com/products/gore-mems-protective-vents)). 

Whilst the aim of this vent is to be acoustically transparent, in reality it will still attenuate recorded acoustic signals slightly. Nonetheless, it is absolutely essential for weatherproofing as any water ingress into the Knowles microphone is likely to result in a total device failure.   

### Microphone horn

The Bugg enclosure has a slight horn for the integrated microphone. This is primarily to amplify acoustic signals, however it comes at the cost of increased directionality of recordings. 

For an excellent deep dive into this topic, see [Darras et al. 2018](https://doi.org/10.12688/f1000research.17511.3){:target="_blank"}.


## External microphones

Coming soon..!
---
layout: default
title: "CMOS Characterization - Dragonfly Telephoto Array"
permalink: /CMOScharacterization/
---

<script>
MathJax = {
  tex: {
    inlineMath: [['$', '$'], ['\\(', '\\)']],
    displayMath: [['$$', '$$'], ['\\[', '\\]']]
  }
};
</script>
<script src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js"></script>


# CMOS Characterization

This project was done during my co-op term at the National Research Council of Canada (NRC) at the Herzberg Astronomy and Astrophysics research Center (HAA) in Victoria BC. This page is a brief overview. A much more **in depth writeup** can be found in the repository here: [Project Repository](https://github.com/aidanmacnichol/CMOS_Characterization)

An additional repository I made that contains utilities and complete documentation for interfacing with lab equipment using RS232 serial connections is here: [Project Repository](https://github.com/aidanmacnichol/serial_automation)

Publications from my work during this term can be seen here: 

[Publications](https://www.researchgate.net/scientific-contributions/Aidan-MacNichol-2285136742)

## Skills
- C++ & Python
- Mathmatics and Statistics
- Expirmental Research 
- Semiconductor Physics
- Optical Engineering
- Linux 

## Background 

This work was done for the Dragonfly Telephoto Array: 

![telescope-me](\assets\images\telescope-me.jpg)

The array currently uses an array of CCD sensors attached to Canon lenses. It is used to research ultra-low surface brightness structures at visible wavelengths. 

My work involved researching scientific Complementary Metal-Oxide Semiconductors (sCMOS) to replace the CCD sensors currently in use on the telescope. 

This involved coming up with experimental procedures and writing C++ and Python code to gather and analyze data. There is a lot of research and papers published about CCD sensors but less so for CMOS. This meant I had to research and adapt different experimental procedures in order to characterize the sensor I was researching. 

 The main characteristics looked at are: 

1. Readout Noise
2. Dark Current
3. Gain 
4. Linearity
7. Charge Persistence
8. Salt & Pepper Noise (Random Telegraph Noise)
9. Quantum Effeciency 

Most of these characteristics just involved writing Python and come C++ scripts to interface with the sensor and gather data. From There more Python is used to analyze the data mostly using simple statistics. 

## Quantum Efficiency 

I want to highlight Quantum Efficiency (QE) here as that was the most difficult but interesting characteristic. QE is a measure of how many photons need to hit a pixel to produce one electron. The QE for a sensor can vary over the visible light spectrum so readings must be taken across all wavelengths between 380-700 nm. 

The general experimental workflow went like: 

1. Use a Monochromatic lamp (light contains the full visible spectrum) 
2. Shine the lamp into a Monochromoter (A monochromoter uses diffraction grates to only produce a single wavelength of light at the output)
3. Use some optics to focus the output of the monochromoter onto a small spot. 
4. First place a photodiode and power meter then take readings (~5 minutes of readings at each wavelength)
5. Replace the photodiode with the CMOS sensor in the exact same orientation and repeat the same readings. 

QE can be then calculated: 

![qeMath](\assets\images\QEMath.png)

Unfortunately I was not able to get very promising results within the time I had. QE is a notoursly difficult measurment to take. I had to but a blackout drape over the entire setup and turn off all the lights in the lab as any ambient light absolutly ruins the readings. 

Some of the technical work I did for this expirment mainly involved writing Python scripts to interface with the RS232 of the monochrometer and power meter, as well as writing clean organized code to save and analyze the GB of data that come from this expiriment. 
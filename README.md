#Written by Benjamin MacLellan

# QOQI Lab Control Software Suite

## Overview
This library contains software to control experimental equipment, as well as simulate, analyze, and visualize data 
in the Quantum Optics & Quantum Information (QOQI) research group at the Institute for Quantum Computing.
The goal of this library is to leverage the ease-of-use of Python to
develop standard interfaces for interacting with hardware, write scripts for experimental measurements,
standardize the data formats for data longevity/reproducibility, and provide common functions and functionality
to simulate and analyze experimental results.

Some key experimental equipment used in experiments include the time tagger (UQD), 
XPS motion controllers (Newport), lasers (TOptica, among others), function generators (Tektronix),
power meters (Thorlabs), and digital acquisitions cards (NI DAQ).

## Installing
The software has been developed and tested on Windows 8/10/11 with Python 3.x.
All packages required are open-source and available to download via `pip`. 
The requirements are split into two files: `requirements.txt` and `requirements-lab.txt`. 
If installing the software for controlling a new experiment, use both. If just performing analyses, just use the first.
To install the dependencies, `pip install -r requirements` and/or `pip install -r requirements-lab.txt`.
Please use `virtualenv` to manage the packages. 
See [here](https://realpython.com/python-virtual-environments-a-primer/) for a quick primer for Python virtual environments,
It's possible `conda` environments can cause issues with certain packages, so it is recommended to use virtualenv instead.


## Structure
The `qoqi` folder contains common analysis and control software. 
* `analysis`: Common analysis tools, such as MLE state tomography, cross-correlation histograms, and fitting models
* `control`: Device-level control of lab equipment, including time taggers, lasers, motion controllers, and power meters 
* `interfaces`: Common elements for building user interfaces, or stand-alone imterfaces for specific tasks (e.g., zeroing waveplates)
* `utils` Important tools across measurements, projects, and tables - most importantly the IO for saving & loading data
* `visualizations`: Standard plotting routines, such as density matrices, and default plot settings

The top-level directory, `projects`, is more dedicated software for a single project.
Here, we have three main files: 
* `_system.py`: Here we define a class which incorporates all lab instruments together for the Bell system,
and provides common functionality such as setting projection measurements
* `_config.py`: This stores all important constants relating to the system. 
This could include time tagger delays and thresholds, waveplate zeros, laser powers, etc.
It is read by the `System` class and overwritten when defaults are changes.
* `_interface.py`: Wraps everything into a single interface. 
Aims to have both fine control over the system (waveplate control, delay and threshold control),
as well as higher-level tools for running measurements (e.g., state tomoography, cross-correlations)

The top-level directory, `tutorials`, contains an assortment of Jupyter notebooks (please add to them!).

The top-level directory, `jobs`, contains bash scripts for running analysis on Compute Canada clusters.

The top-level directory, `papers`, contains scripts for generating publication-quality figures from collected data.

## Experimental equipment
Below are some links to more information on the libraries for different hardware.

* **Time tagger (Universal Quantum Devices):** 
The UQD Logic 16 time tagger is an analog-to-digital time converter. 
It records, on the picosecond range, when the pulse from a photon detection arrives. 
Inside is a high-speed FPGA, which has one processor to record the time of photon clicks,
and another to send/receive information with the control PC.
[Software and dcoumentation download](https://uqdevices.com/software/)

* **XPS Newport Motion Controller:** 
[Programming manual for XPS unit](https://www.newport.com/medias/sys_master/images/images/hb9/h1d/8797307699230/XPS-Q8-Software-Drivers-Manual.pdf)
control software developed by Matt Newville (https://github.com/pyepics/newportxps)
The XPS is a control unit for multiple rotation & translation devices. 
These are used for rotating waveplates, moving translation stages, and others. 

* **TOptica lasers**: These lasers connects via an RSA connection, which then require a USB converter. The connection is , [code examples](https://toptica.github.io/python-lasersdk/getting_started.html)

* **Thorlabs powermeters** Useful for plenty of activities: alignment, feedback loops.

## Experimental equipment control
Each of the pieces of hardware above is controlled with by an `object` in Python. 
For example, the TOptica diode laser is connected via serial (USB). 
Common functions are to turn the emission on or off, change the laser power.
To do this, you first creat an **instance** of the object, for example,
```
from qoqi.control.laser_toptica import TOpticaLaser
laser = TOpticaLaser(port='COM3)
```
Now, you can change the laser settings as such,
```
laser.on()
laser.off()
laser.power(15)  # in mW
```
Every other instrument is connected to in a similar way, but the type of port will change. 
Many instruments need to be closed at the end of the measurement, such as,
```
laser.close()
```

## Computational resources
### Compute Canada
When fitting large or many models, computational time & resources can be a potential bottleneck. 
We have an account with Compute Canada (CC), which is a group of computational clusters available at no cost to Canadian academic researchers.
Register for an account following the instructions [here](https://www.computecanada.ca/research-portal/account-management/apply-for-an-account/)
and see the CC wiki for more information on how to use the clusters [here](https://docs.computecanada.ca/wiki/Compute_Canada_Documentation).
In the `qoqi/jobs` folder are examples of bash scripts used to run a job on a CC cluster. 

### IQC Workstations
Another option for running large data simulations/analyses is to reserve one of the IQC workstations.
See the QOQI Wiki for more details.

## Contributing
You can contribute by: using the code, pointing out bugs, writing more code :) 

Benjamin MacLellan, 2022 | [Email](benjamin.maclellan@uwaterloo.ca)

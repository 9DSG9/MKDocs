## Introduction

This documentation will guide you through setting up your own smart mirror powered by a Raspberry Pi. A smart mirror combines a two-way mirror with a digital display, creating an interactive surface that shows personalized information while functioning as a regular mirror.

This project aims to boost your productivity by turning a regular mirror into a morning information hub that displays calendar events, weather updates, news headlines, personal reminders, and other customizable widgets—all while you complete your morning routine.

## Who Should Use This Guide

This guide is designed specifically for first and second-term Computer Science students who possess working knowledge of:

## Prerequisites

To follow these instructions, you will need:

* A computer with macOS operating system
* A myBCIT email account
* Basic knowledge of mouse, keyboard, and trackpad terminologies

## Hardware Requirements

Before beginning this installation, you'll need:

* A Raspberry Pi (3B+ or newer recommended)
* SD card with Raspberry Pi OS installed
* Monitor or TV with HDMI input (for initial setup)
* USB keyboard and mouse (for initial setup)
* Internet connection (Wi-Fi or Ethernet)

## Software Requirements

Before proceeding, ensure you have the following installed:

* Raspberry Pi OS (Bullseye or newer)
* Git (for cloning repositories)
* Node.js (v14.x or later recommended)
* npm Package Manager (v7.x or later)
* Text editor for configuration (nano or vim)

## Typographical Conventions
In these smart mirror setup instructions, I'll use the following conventions to help you navigate the documentation:

1. Terminal commands will be displayed in code blocks:

		``` $ sudo apt install ```
	 

2. File paths and configuration files will be formatted: 
	
	config.js or /etc/MagicMirror/config
	 


3. User interface elements will be displayed in [square brackets]:

	[Settings] or [Display]
	

4. Step-by-step instructions will be numbered and use bold for actions:

	1. **Open** the terminal
	2. **Type** the command to update your system

5. Important changes to existing files will be highlighted in yellow:

	```js title="Original.js"
	function greet(name) {
		const message = `Hello, ${name}!`;
		console.log(message);
		return message;
	}
	```

	```js title="Modified.js" 
	function greet(name) {
  		const message = `<!-- highlight-span -->Hello<!-- /highlight-span -->, ${name}!`;
  		console.log(message);
  		return message;
	}
	```


## Notes and Warning Messages


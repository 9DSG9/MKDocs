# Installation Guide

## Overview

This section guides you through installing the MagicMirror² software on your Raspberry Pi. MagicMirror² is an open-source platform that transforms your mirror into a customizable display with various modules for different types of information.

## Preparing Your Raspberry Pi

### Update Your System

Before installing any new software, ensure your system is up to date:

1. **Open** a terminal window
2. **Run** the following commands: 
   ```
   sudo apt update
   sudo apt upgrade -y
   ```
3. **Wait** for the process to complete (may take several minutes)

!!! note "System Updates"
    Keeping your system updated ensures compatibility with the latest software and improves security.

### Install Required Dependencies

MagicMirror² requires several dependencies to function properly:

1. **Install** Node.js and npm:
   ```
   curl -sL https://deb.nodesource.com/setup_14.x | sudo -E bash -
   sudo apt install -y nodejs
   ```

2. **Verify** the installation:
   ```
   node -v
   npm -v
   ```

3. **Install** other required packages:
   ```
   sudo apt install -y git python3-pip
   ```

## Installing MagicMirror²

### Clone the Repository

1. **Navigate** to your home directory:
   ```
   cd ~
   ```
     
2. **Clone** the MagicMirror repository:
   ```
   git clone https://github.com/MichMich/MagicMirror.git
   ```
     
3. **Navigate** to the MagicMirror directory:
   ```
   cd MagicMirror
   ```

### Install Dependencies and Run Setup

1. **Install** the required npm packages:
   ```
   npm install
   ```

!!! warning "Installation Time"
    This process may take 10-15 minutes on a Raspberry Pi. Be patient and don't interrupt the process.

2. **Copy** the sample configuration:
   ```
   cp config/config.js.sample config/config.js
   ```
     
3. **Test** the installation:
   ```
   npm start
   ```

If successful, you should see the MagicMirror interface appear on screen.

4. **Press** `Ctrl+Q` to exit the application

## Conclusion

You have now successfully installed MagicMirror² on your Raspberry Pi and configured it to start automatically at boot. In the next section, we'll configure the basic modules and customize the display layout.


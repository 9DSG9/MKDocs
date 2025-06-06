# Raspberry Pi OS Setup

## Overview

This section guides you through installing and configuring Raspberry Pi OS on your SD card, which serves as the foundation for your smart mirror software. Raspberry Pi OS (formerly called Raspbian) is the official operating system for Raspberry Pi devices and provides the Linux environment needed to run the MagicMirror² software.

## Preparing the SD Card

### Download and Install Raspberry Pi Imager

1. **Open** your web browser and go to the [Raspberry Pi website](https://www.raspberrypi.com/software/)
   
2. **Download** the Raspberry Pi Imager software for your operating system (Windows, macOS, or Linux)
   
3. **Install** the downloaded software by following the installation prompts

    !!! note
        Raspberry Pi Imager is the official tool for installing Raspberry Pi OS and is regularly updated with the latest versions.

    ![Raspberry Pi Imager download page](./images/pi-imager-download.gif)

### Flash Raspberry Pi OS to the SD Card

1. **Insert** your microSD card into your computer using an adapter if necessary
   
2. **Launch** Raspberry Pi Imager

3. **Click** [CHOOSE DEVICE] and select your Raspberry Pi model from the list
   
4. **Click** [CHOOSE OS] and select "Raspberry Pi OS (64-bit)" from the list of operating systems
   
    !!! note
        Using the 64-bit version is recommended for Raspberry Pi 3 or newer, as it allows better performance for the smart mirror software.

5. **Click** [CHOOSE STORAGE] and select your SD card from the list

    !!! warning "Data Loss Risk"
        All data on the selected SD card will be erased during this process. Make sure you have selected the correct storage device.

     ![Selecting OS and storage in Pi Imager](./images/pi-imager-selection.gif){: markdown="1" }

!!! note "Customization Settings"
      If you don’t configure your Raspberry Pi via OS customization settings, Raspberry Pi OS will ask you for the same information at first boot during the configuration wizard. You can click the No button to skip OS customisation

7. **Configure** the following settings:
    * **Check** "Set hostname" and enter a name (e.g., "smartmirror")
    * **Check** "Enable SSH" and select "Use password authentication"
    * **Check** "Set username and password" and create a username and secure password
    * **Check** "Configure wireless LAN" if you plan to use Wi-Fi
    * **Enter** your Wi-Fi network name (SSID) and password
    * **Select** your country from the dropdown menu
    * **Check** "Set locale settings" and select your time zone and keyboard layout

     ![Advanced options configuration](./images/pi-imager-advanced.gif)

8. **Click** [SAVE] to save your advanced options

9. **Click** [WRITE] to begin the OS installation process
   
10. **Confirm** by clicking [YES] when prompted

11. **Wait** for the writing and verification process to complete (this may take several minutes)

12. **Click** [CONTINUE] when prompted that the write was successful

      ![Preparing to write progress in Pi Imager](./images/Preparing_To_Write.jpg)

      ![Writing progress in Pi Imager](./images/pi-imager-writing.jpg)

      ![Verifying progress in Pi Imager](./images/pi-imager-verifying.jpg)

      ![Write successful in Pi Imager](../images/pi-imager-write-successful.jpg)

13. **Remove** the SD card from your computer

    !!! success
        The Raspberry Pi software has been successfully written to the SD card, and we can now move forward!

## Initial Boot and Configuration

### Connect Your Raspberry Pi

1. **Insert** the prepared microSD card into your Raspberry Pi

2. **Connect** the following to your Raspberry Pi:
    * Display (via HDMI)
    * Keyboard and mouse (via USB)
    * Power supply

3. **Power on** your Raspberry Pi by connecting the power supply

    !!! note
         If you configured SSH and Wi-Fi in the previous steps, you can technically skip the physical keyboard, mouse, and display connections and access your Raspberry Pi remotely. However, for initial setup, a direct connection is recommended.

    ![Raspberry Pi Peripherals](./images/peripherals.png)

### First Boot Setup

The Raspberry Pi will automatically boot and perform initial setup based on the configurations you set in the Raspberry Pi Imager:

1. **Wait** for the Raspberry Pi to complete its first boot process (may take a few minutes)

2. **Verify** your connection to Wi-Fi (if configured) by checking the Wi-Fi icon in the top-right corner of the screen

3. **Open** Terminal by clicking on the terminal icon in the taskbar or pressing `Ctrl+Alt+T`

4. **Update** your system by running the following commands:
   ```
   sudo apt update
   sudo apt upgrade -y
   ```

5. **Wait** for the update process to complete (this may take 10-15 minutes depending on your internet speed)

    ![Raspberry Pi Sudo Apt Update](./images/sudo-apt-update.png)

## Optimizing Your Raspberry Pi for Smart Mirror Use

### Configure Display Settings

1. **Open** Raspberry Pi Configuration by clicking the Raspberry Pi menu icon (top-left) > Preferences > Raspberry Pi Configuration

2. **Click** on the [Display] tab

3. **Check** "Disable screen blanking" to prevent the display from turning off

4. **Click** [OK] to save changes

    ![Raspberry Pi Config Display](./images/Displayblanking-.gif)

### Configure Boot Options

1. **Open** Terminal if not already open

2. **Enter** the following command to edit the boot configuration:
   ```
   sudo raspi-config
   ```

    ![Raspberry Pi Boot Configuration](./images/bootconfigmenu.png)

3. **Select** "System Options" using arrow keys and press Enter

4. **Select** "Boot / Auto Login" and press Enter

5. **Select** "Desktop Autologin" to make the Raspberry Pi automatically log in on startup

6. **Navigate** to "Finish" and press Enter to save changes

7. **Reboot** when prompted

    ![Raspberry Pi Reboot](./images/rebootyes.png)

### Configure Power Management

To optimize performance for the smart mirror:

1. **Open** Terminal

2. **Edit** the config.txt file:
   ```
   sudo nano /boot/config.txt
   ```

    !!! note "Pi5 command"
         You may need to use /boot/firmware/config.txt as the path if using a Raspberry Pi 5.

3. **Add** the following lines at the end of the file to allocate more memory to the GPU for better display performance:
   ```
   # Optimize for display
   gpu_mem=128
   ```

4. **Save** the file by pressing `Ctrl+O`, then Enter

5. **Exit** nano by pressing `Ctrl+X`

6. **Reboot** your Raspberry Pi:
   ```
   sudo reboot
   ```

    ![Raspberry Pi config.txt Edit](./images/addlinestoconfigtxt.png)

## Enable Remote Access (Optional)

If you didn't enable SSH during the initial setup with Raspberry Pi Imager, you can do it now:

1. **Open** Terminal

2. **Run** the following command:
   ```
   sudo raspi-config
   ```

3. **Select** "Interface Options" and press Enter

4. **Select** "SSH" and press Enter

5. **Select** "Yes" to enable SSH

6. **Navigate** to "Finish" and press Enter

7. **Reboot** when prompted

    ![Raspberry Pi SSH Configuration](./images/SSHenable.gif)

## Finding Your Raspberry Pi's IP Address

To connect to your Raspberry Pi remotely or for the next steps in the smart mirror setup:

1. **Open** Terminal

2. **Run** the following command:
   ```
   hostname -I
   ```

3. **Note** the IP address displayed (e.g., 192.168.1.100)

    ![Raspberry Pi IP Address](./images/IPAddress.png)

## Conclusion

You have now successfully:

- Downloaded and installed Raspberry Pi OS on your SD card
- Completed the initial boot and configuration
- Updated your system with the latest software
- Optimized display and boot settings for smart mirror use
- Enabled remote access (if desired)
- Located your Raspberry Pi's IP address for future reference

Your Raspberry Pi is now prepared for the next step: installing the required packages for your smart mirror. In the next section, we'll guide you through installing Node.js, Git, and other dependencies needed for the MagicMirror² software.

![Raspberry Pi Initial Screen](./images/piinitalscreen.png)
![Raspberry Pi User Setup](./images/usersetup.png)
![Raspberry Pi Update Check](./images/startupdate.png)
![Raspberry Pi Wifi Select](./images/wifistart.png)
![Raspberry Pi Wifi Password](./images/wifestartpassword.png)
![Raspberry Pi Country](./images/location.png)
![Raspberry Pi Download Updates](./images/downloadstartprogress.png)
![Raspberry Pi Setup Complete](./images/startend.png)
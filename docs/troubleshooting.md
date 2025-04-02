# Troubleshooting Guide

## Screen Not Turning On/Off Automatically

**Problem**: The mirror display doesn't turn on or off at the expected times.

**Solution**:

1. Check that the automatic screen management module is correctly configured:
   ```
   nano ~/MagicMirror/config/config.js
   ```

2. Ensure the MMM-ScreenManager module is included and properly configured:
   ```javascript
   {
      module: "MMM-ScreenManager",
      position: "bottom_bar",
      config: {
         turnOnHour: 7,     // Turn on at 7 AM
         turnOffHour: 23,   // Turn off at 11 PM
      }
   },
   ```

3. If using PIR motion sensor for screen management, verify connections and test the sensor independently:
   ```
   gpio -g read 17  // Assuming PIR connected to GPIO 17
   ```
     
4. Restart MagicMirror:
   ```
   pm2 restart MagicMirror
   ```

## Modules Not Loading Properly

**Problem**: Some modules display error messages or fail to load.

**Solution**:

1. Check the MagicMirror logs for errors:
   ```
   pm2 logs MagicMirror
   ```
     
2. Verify module dependencies are installed:
   ```
   cd ~/MagicMirror/modules/[module-name]
   npm install
   ```
     
3. Check the module's GitHub page for known issues or required configurations

4. Update the module to the latest version:
   ```
   cd ~/MagicMirror/modules/[module-name]
   git pull
   npm install
   ```
     
5. If a module continues to cause problems, try disabling it temporarily by commenting it out in the config file:
   ```javascript
   // {
   //  module: "problematic-module",
   //  position: "top_right",
   //  config: { /* ... */ }
   // },
   ```

## Touch Interface Not Responding

**Problem**: The touch screen doesn't register touches or responds incorrectly.

**Solution**:

1. Verify that the touch drivers are installed:
   ```
   sudo apt install -y xserver-xorg-input-evdev
   ```

2. Recalibrate the touch screen:
   ```
   DISPLAY=:0 xinput_calibrator
   ```
     
3. Check if the touch interface is detected:
   ```
   xinput list
   ```

4. If using a USB touch overlay, ensure it's properly connected and powered
     
5. Restart the X server:
   ```
   sudo systemctl restart lightdm
   ```
     
6. If problems persist, try setting up the touch interface from scratch following the guide in the Configuration section

## Voice Commands Not Being Recognized

**Problem**: Alexa voice commands aren't recognized or don't trigger actions.

**Solution**:

1. Check that your microphone is working:
   ```
   arecord -l
   ```

2. Test audio recording:
   ```
   arecord -d 5 test.wav
   aplay test.wav
   ```

3. Verify the MMM-awesome-alexa module configuration:
   ```
   nano ~/MagicMirror/config/config.js
   ```
     
4. Check that the wake word detection is properly set:
   ```javascript
   {    
      module: "MMM-awesome-alexa",    
      config: {    
         wakeWord: "Alexa",
      }
   },
   ```

5. Re-run the authentication process:
   ```
   cd ~/MagicMirror/modules/MMM-awesome-alexa
   npm run authenticate
   ```

6. Consider adjusting the microphone sensitivity or using a USB microphone with better quality

## Wi-Fi Connectivity Issues

**Problem**: The smart mirror loses internet connection or can't connect to Wi-Fi.

**Solution**:

1. Check your Wi-Fi connection status:
   ```
   iwconfig
   ```

2. Verify that the network is available:
   ```
   sudo iwlist wlan0 scan | grep ESSID
   ```

3. Check your network configuration:
   ```
   nano /etc/wpa_supplicant/wpa_supplicant.conf
   ```
     
4. Ensure your Wi-Fi credentials are correct:
   ```
   network={
      ssid="YourNetworkName"
      psk="YourPassword"
   }
   ```

5. Restart the networking service:
   ```
   sudo systemctl restart networking
   ```

6. If problems persist, consider using a Wi-Fi repeater or a USB Wi-Fi adapter with better range

## MagicMirror Crashes or Freezes

**Problem**: The MagicMirror application crashes, freezes, or shows a blank screen.

**Solution**:

1. Check system resources:
   ```
   top
   ```

2. Verify that you have enough free memory:
   ```
   free -h
   ```
     
3. Check the SD card for corruption:
   ```
   sudo fsck -y /dev/mmcblk0p2
   ```

4. Disable some modules to reduce resource usage by commenting them out in the config file
     
5. Consider using PM2 to manage MagicMirror:
   ```
   npm install -g pm2
   pm2 start ~/MagicMirror/installers/pm2_MagicMirror.json
   ```

6. Set up automatic recovery:
   ```
   pm2 startup
   pm2 save
   ```
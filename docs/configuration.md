# Configuration Guide

## Overview

This section covers how to configure the basic modules of your smart mirror, set up the touch interface, and integrate voice control capabilities.

## Configuring Basic Modules

MagicMirror² comes with several default modules. We'll configure the most useful ones for your smart mirror.

### Weather Module

1. **Open** the configuration file:
   ```
   nano ~/MagicMirror/config/config.js
   ```
     
2. **Locate** the weather module configuration section

3. **Modify** the settings with your location:
   ```javascript
   {
      module: "weather",
      position: "top_right",
      config: {
         weatherProvider: "openweathermap",
         type: "current",
         location: "New York",
         locationID: "5128581",
         apiKey: "YOUR_OPENWEATHERMAP_API_KEY"
      }
   },
   ```

4. **Get** an API key by:
   * Going to [OpenWeatherMap](https://openweathermap.org/)
   * Creating a free account
   * Generating an API key from your account dashboard

!!! warning "API Key Security"
    Never share your API keys publicly. They should be kept private and secure.

### Calendar Module

1. **While** still in the config file, locate the calendar module section

2. **Modify** it to include your calendar:
   ```javascript
   {
      module: "calendar",
      header: "Upcoming Events",
      position: "top_left",
      config: {
         calendars: [
            {
               symbol: "calendar-check",
               url: "YOUR_ICAL_URL"
            }
         ]
      }
   },
   ```

3. **Replace** `YOUR_ICAL_URL` with your Google Calendar, Outlook, or other iCal URL

### News Feed Module

1. **Find** the newsfeed module section in the config file

2. **Update** it with your preferred news sources:
   ```javascript
   {
      module: "newsfeed",
      position: "bottom_bar",
      config: {
         feeds: [
            {
               title: "CNN",
               url: "http://rss.cnn.com/rss/edition.rss"
            },
            {
               title: "BBC",
               url: "https://feeds.bbci.co.uk/news/world/rss.xml"
            }
         ],
         showSourceTitle: true,
         showPublishDate: true,
         broadcastNewsFeeds: true,
         broadcastNewsUpdates: true
      }
   }
   ```

3. **Save** the file by pressing `Ctrl+O`, then `Enter`

4. **Exit** the editor by pressing `Ctrl+X`

## Setting Up Touch Interface

To enable touch functionality on your mirror:

### Installing the Touch Module

1. **Navigate** to the modules directory:
   ```
   cd ~/MagicMirror/modules
   ```
     
2. **Clone** the MMM-Touch module:
   ```
   git clone https://github.com/sheyabernstein/MMM-Touch.git
   ```

3. **Install** the module dependencies:
   ```
   cd MMM-Touch
   npm install
   ```

4. **Return** to the MagicMirror directory:
   ```
   cd ~/MagicMirror
   ```
     
5. **Edit** the configuration file to add the touch module:
   ```
   nano config/config.js
   ```

6. **Add** the following to your `modules` array:
   ```javascript
   {
      module: "MMM-Touch",
      position: "bottom_center",
      config: {
         debug: false,
         gestures: {
            swipeRight: {
               notification: "SHOW_ALERT",
               payload: {
                  title: "Swiped Right",
                  message: "You swiped right!",
                  timer: 2000
               }
            },
            swipeLeft: {
               notification: "SHOW_ALERT",
               payload: {
                  title: "Swiped Left",
                  message: "You swiped left!",
                  timer: 2000
               }
            }
         }
      }
   },
   ```

7. **Save** and exit the editor

### Calibrating the Touch Screen

If your touch screen requires calibration:

1. **Install** the calibration tool:
   ```
   sudo apt install -y xinput-calibrator
   ```
     
2. **Run** the calibration tool:
   ```
   DISPLAY=:0 xinput_calibrator
   ```
     
3. **Follow** the on-screen instructions to touch the targets

4. **Copy** the calibration data displayed after completion

5. **Create** a calibration configuration file:
   ```
   sudo nano /etc/X11/xorg.conf.d/99-calibration.conf
   ```
     
6. **Paste** the calibration data into this file

7. **Save** and exit

8. **Restart** your Raspberry Pi:
   ```
   sudo reboot
   ```

## Setting Up Voice Control with Alexa

To integrate Alexa voice capabilities with your smart mirror:

### Installing Required Dependencies

1. **Install** required packages:
   ```
   sudo apt install -y python3-pyaudio libatlas-base-dev portaudio19-dev
   ```

2. **Clone** the Alexa module:
   ```
   cd ~/MagicMirror/modules
   git clone https://github.com/dolanmiu/MMM-awesome-alexa.git
   ```
     
3. **Install** module dependencies:
   ```
   cd MMM-awesome-alexa
   npm install
   ```

### Register with Amazon Developer

1. **Create** an Amazon developer account at [developer.amazon.com](https://developer.amazon.com/)
2. **Register** a new Alexa device in the developer console
3. **Note** your Amazon Client ID, Secret, and Device ID

### Configure the Alexa Module

1. **Edit** the MagicMirror configuration:
   ```
   nano ~/MagicMirror/config/config.js
   ```
     
2. **Add** the Alexa module to your modules array:
   ```javascript
   {
      module: "MMM-awesome-alexa",
      position: "bottom_bar",
      config: {
         wakeWord: "Alexa",
         clientId: "YOUR_AMAZON_CLIENT_ID",
         clientSecret: "YOUR_AMAZON_CLIENT_SECRET",
         deviceId: "YOUR_AMAZON_DEVICE_ID"
      }
   },
   ```

3. **Replace** the placeholder values with your actual Amazon credentials

4. **Save** and exit the editor

5. **Start** the authentication process:
   ```
   cd ~/MagicMirror/modules/MMM-awesome-alexa
   npm run authenticate
   ```

6. **Follow** the on-screen instructions to complete authentication

## Conclusion

You have now configured the basic modules, enabled touch interface, and set up voice control for your smart mirror. In the next section, we'll cover customizing the layout and adding additional modules to enhance functionality.

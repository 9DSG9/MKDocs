# Customization Guide

## Overview

This section will guide you through customizing the layout of your smart mirror, adding additional modules, and creating your own custom modules to further enhance your smart mirror experience.

## Customizing the Layout

The MagicMirror² layout is defined in the configuration file and can be easily modified.

### Understanding Position Values

The default positions available in MagicMirror² are:

* `top_bar`: Spans the entire top of the mirror
* `top_left`: Top left corner
* `top_center`: Top center
* `top_right`: Top right corner
* `upper_third`: Upper third of the mirror, spanning the entire width
* `middle_center`: Center of the mirror
* `lower_third`: Lower third of the mirror, spanning the entire width
* `bottom_left`: Bottom left corner
* `bottom_center`: Bottom center
* `bottom_right`: Bottom right corner
* `bottom_bar`: Spans the entire bottom of the mirror
* `fullscreen_above`: Fullscreen, above all other modules
* `fullscreen_below`: Fullscreen, below all other modules

### Modifying Module Positions

1. **Open** the configuration file:
   ```
   nano ~/MagicMirror/config/config.js
   ```

2. **Locate** the `modules` array

3. **Change** the `position` property of any module to adjust its placement:
   ```javascript
   {
      module: "clock",
      position: "top_center", // Change this to any position value
      config: {
         // Module configurations
      }
   },
   ```

4. **Save** your changes and exit the editor

### Creating Custom Layouts

You can create a custom layout by modifying the regions in the MagicMirror² CSS:

1. **Create** a custom CSS file:
   ```
   nano ~/MagicMirror/css/custom.css
   ```
     
2. **Add** custom positioning rules:
   ```css
   .region.custom_top {
      top: 0;
      left: 25%;
      right: 25%;
      height: 100px;
   }

   .region.custom_middle {
      top: 50%;
      left: 10%;
      right: 10%;
      transform: translateY(-50%);
   }
   ```

3. **Save** the file and exit

4. **Link** the custom CSS in your HTML by editing:
   ```
   nano ~/MagicMirror/index.html
   ```
     
5. **Add** this line in the `<head>` section:
   ```html
   <link rel="stylesheet" type="text/css" href="css/custom.css">
   ```
     
6. **Save** and exit

7. **Use** your custom positions in your config file by changing module positions to match your custom class names (e.g., `"custom_top"`, `"custom_middle"`)

## Adding Additional Modules

MagicMirror² has a thriving community with hundreds of additional modules available.

### Finding Modules

1. **Browse** the [MagicMirror Module Wiki](https://github.com/MichMich/MagicMirror/wiki/3rd-Party-Modules)
2. **Search** for modules that match your needs

### Installing a Spotify Module Example

1. **Clone** the module repository:
   ```
   cd ~/MagicMirror/modules
   git clone https://github.com/skuethe/MMM-Spotify.git
   ```
     
2. **Install** module dependencies:
   ```
   cd MMM-Spotify
   npm install
   ```
     
3. **Register** a Spotify Developer application at [developer.spotify.com](https://developer.spotify.com/)

4. **Note** your Client ID and Client Secret

5. **Configure** the module in your config file:
   ```
   nano ~/MagicMirror/config/config.js
   ```
     
6. **Add** the module configuration:
   ```javascript
   {
      module: "MMM-Spotify",
      position: "bottom_left",
      config: {
         clientID: "YOUR_SPOTIFY_CLIENT_ID",
         clientSecret: "YOUR_SPOTIFY_CLIENT_SECRET",
         showCoverArt: true
      }
   },
   ```

7. **Save** and exit

8. **Authenticate** with Spotify by following the module's instructions

### Installing a Compliment Module

1. **Clone** the module:
   ```
   cd ~/MagicMirror/modules
   git clone https://github.com/timdows/MMM-DailyBibleVerse.git
   ```
     
2. **Install** dependencies:
   ```
   cd MMM-DailyBibleVerse
   npm install
   ```
     
3. **Add** the module to your config:
   ```javascript
   {
      module: "MMM-DailyBibleVerse",
      position: "lower_third",
      config: {
         version: "NIV"
      }
   },
   ```

## Creating Your Own Custom Module

You can create your own module to add functionality specific to your needs.

### Creating the Module Structure

1. **Create** a new module directory:
   ```
   cd ~/MagicMirror/modules
   mkdir -p MMM-MyCustomModule
   cd MMM-MyCustomModule
   ```
     
2. **Create** the main module file:
   ```
   nano MMM-MyCustomModule.js
   ```
     
3. **Add** the basic module code:
   ```javascript
   Module.register("MMM-MyCustomModule", {
      // Default module configuration
      defaults: {
         text: "Hello World!",
         updateInterval: 60000  // milliseconds
      },
    
      // Override start method
      start: function() {
         Log.info("Starting module: " + this.name);
         this.loaded = false;
         this.scheduleUpdate();
      },
    
      // Override getDom method
      getDom: function() {
         var wrapper = document.createElement("div");
      
         if (!this.loaded) {
            wrapper.innerHTML = "Loading...";
            return wrapper;
         }
      
         var header = document.createElement("header");
         header.innerHTML = "My Custom Module";
         wrapper.appendChild(header);
      
         var content = document.createElement("div");
         content.className = "module-content";
         content.innerHTML = this.config.text;
         wrapper.appendChild(content);
      
         return wrapper;
      },
    
      // Schedule next update
      scheduleUpdate: function() {
         var self = this;
         setInterval(function() {
            self.updateDom();
         }, this.config.updateInterval);
      }
   });
   ```

4. **Save** and exit

5. **Create** a styles file:
   ```
   nano MMM-MyCustomModule.css
   ```
     
6. **Add** some basic styles:
   ```css
   .MMM-MyCustomModule .module-content {
      font-size: 20px;
      color: white;
      padding: 10px;
   }
   ```

7. **Save** and exit

8. **Add** your module to your config:
   ```javascript
   {
      module: "MMM-MyCustomModule",
      position: "middle_center",
      config: {
         text: "This is my first custom module!",
         updateInterval: 30000
      }
   },
   ```

### Making Your Module Interactive

To add interactivity to your module:

1. **Update** your module file:
   ```
   nano MMM-MyCustomModule.js
   ```

2. **Add** notification handling:
   ```javascript
   // Add this to your module definition
   notificationReceived: function(notification, payload, sender) {
      if (notification === "CUSTOM_ACTION") {
         // Handle the notification
         this.config.text = "Action received: " + payload;
         this.updateDom();
      }
   },

   // Add a method to send notifications
   sendNotification: function(message) {
      this.sendSocketNotification("SEND_MESSAGE", message);
   }
   ```

3. **Create** a node helper for background tasks:
   ```
   nano node_helper.js
   ```
      
4. **Add** the node helper code:
   ```javascript
   var NodeHelper = require("node_helper");

   module.exports = NodeHelper.create({
      socketNotificationReceived: function(notification, payload) {
         if (notification === "SEND_MESSAGE") {
            console.log("Received message: ", payload);
            // Process the message and send result back
            this.sendSocketNotification("MESSAGE_RESULT", 
            "Processed: " + payload);
         }
      }
   });
   ```

5. **Save** and exit

## Conclusion

You have now learned how to customize the layout of your smart mirror, add additional modules from the community, and create your own custom modules. With these skills, you can tailor your smart mirror to display exactly the information you want in the way you want it.
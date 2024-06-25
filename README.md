# 🦻 OpenEarable - Dashboard v1.4.1
[Open Dashboard ↗️](https://thiastux.github.io/openearable-dashboard/)

This repository offers a simplified version of the OpenEarable dashboard. The interface is meant to be used to control OpenEarable that are using firmware >= 1.4.0 from this [repo](https://github.com/ThiasTux/open-earable). A hosted version of this dashboard is available [here](https://thiastux.github.io/openearable-dashboard/). The dashboard offers the minimal functionalities to start/stop a data collection on the sensor SD card. This version also supports the left/right naming of the Openearable (OELeft-XXXX/OERight-XXXX).

This repository also includes the [OpenEarable.js](https://github.com/OpenEarable/dashboard#openearablejs-library) JavaScript library in the `assets/js/OpenEarable.js` folder. This way, researchers and developers can easily integrate OpenEarable into their own workflows.

<kbd> <br> [Get OpenEarable device now!](https://forms.gle/R3LMcqtyKwVH7PZB9) <br> </kbd>

## Usage
The dashboard runs directly in your browser. You can connect to OpenEarable without having to install anything as it works via WebBLE (latest Chrome version is recommended).

<img src="assets/images/screenshot.jpg" style="width: 100%;">


If you want to run the dashboard yourself and have python3 installed, you can use the following command to run the website from the root of this repository. This will start the webserver at `http://localhost:8000`.
```bash
python3 -m http.server
```

## OpenEarable.js Library
OpenEarable.js is a JavaScript library that provides a seamless interface to connect, manage, and interact with OpenEarable. The library abstracts the complexities of BLE communication, making it easy to fetch device details, manage sensors, LED, and audio, and subscribe to device state changes.

### Installation
To use OpenEarable.js simply integrate the library found under `assets/js/OpenEarable.js` into your project as follows.
```html
<script src="./OpenEarable.js"></script>
```

### Usage Examples
The following example shows how to use the OpenEarable library.

#### Connect to OpenEarable
```js
const openEarable = new OpenEarable();

// Read device information once connected
openEarable.bleManager.subscribeOnConnected(async () => {
    const deviceId = await openEarable.readDeviceIdentifier();
    const firmwareVersion = await openEarable.readFirmwareVersion();
    const hardwareVersion = await openEarable.readHardwareVersion();
});

// Connect to OpenEarable
openEarable.bleManager.connect();

// Disconnect from OpenEarable
openEarable.bleManager.disconnect();
```

#### Subscribe to Sensor Data
```js
// Subscribe to the sensor data
openEarable.subscribeOnSensorDataReceived((data) => {
      // Process the received IMU data
      console.log(data);
});

// Enable IMU at 30 Hz
sensorManager.writeSensorConfig(0, 30, 0); // 0 sensorId, 30 samplingRate
```

#### Play Audio
OpenEarable allows playing mono, 16-bit, 44.1kHz *.wav files from the internal microSD card. In addition, it is possible to generate a constant frequency on the OpenEarable directly or play one of the built-in jingles.

```js
// Supported audio states: AUDIO_STATE.PLAY, AUDIO_STATE.PAUSE, AUDIO_STATE.STOP

// Play audio file from microSD card with the name "music.wav"
openEarable.audioPlayer.wavFile(AUDIO_STATE.PLAY, "music.wav");

// Play a constant frequency sine wave at 22 kHz
// Wave types: WAVE_TYPE.SINE, WAVE_TYPE.TRIANGLE, WAVE_TYPE.SQUARE, WAVE_TYPE.SAW
// Loudness: 0 to 1.0
openEarable.audioPlayer.frequency(AUDIO_STATE.PLAY, WAVE_TYPE.SINE, 22000, 0.5);

// Play a default NOTIFICATION jingle from the internal OpenEarable storage
openEarable.audioPlayer.jingle(AUDIO_STATE.PLAY, JINGLE.NOTIFICATION);
```

#### Control RGB LED
```js
// set the LED color to red (accepts values from 0 to 255 for R, G, B)
openEarable.rgbLed.writeLedColor(255, 0, 0); 
```

#### Receive Button Events
```js
openEarable.buttonManager.subscribeOnButtonStateChanged((state) => {
    // process button state here
})
```

#### Receive Battery Events
```js
openEarable.subscribeBatteryLevelChanged((batteryLevel) => {
    // process battery level here
});

// Supported battery states: BATTERY_STATE.CHARGING, BATTERY_STATE.CHARGED, BATTERY_STATE.NOT_CHARGING
openEarable.subscribeBatteryStateChanged((batteryState) => {
    // process battery state here
});
```

## License
Distributed under the MIT License. See LICENSE for more information.

## Acknowledgements
- developed by the TECO research group at the Karlsruhe Institute of Technology
- icons by Bootstrap Icons

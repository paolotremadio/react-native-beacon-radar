# react-native-beacon-radar

Package to scan for iBeacons on both Android and IOS. This module is fully compatible with Expo (Will not work with Expo Go, but will work with development build.)

## Installation

```sh
npm install react-native-beacon-radar
```

## Basic usage

```js
import { DeviceEventEmitter } from 'react-native';
import { startScanning } from 'react-native-beacon-radar';

// ...

startScanning('YOUR UUID', {
  useForegroundService: true,
  useBackgroundScanning: true,
});

DeviceEventEmitter.addListener('onBeaconsDetected', (beacons) => {
  console.log('onBeaconsDetected', beacons);
});
```

## Current API:
| Method                            | Description                                                                                                                                                                                                           |
|:----------------------------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **startScanning**                 | This method starts scanning for a certain beacon based on its UUID.                                                                                                                                                   |
| **startRadar (Android only)**     | This method starts scanning for all beacons in range. This is only available on Android.                                                                                                                              |


| Event                       | Description                                                                                                                                                                                                                                                      |
|:----------------------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **onBeaconsDetected**       | This event gets called when the beacon you are searching for is in range. Please note, this event will fire with an empty array if you have Geofencing active on your app; also note: there's no clear "exit area" event; the beacon will just drop off the list |
| **onBluetoothStateChanged** | Get the state of bluetooth on the device (Requires `bluetooth-central` mode to be set on `UIBackgroundModes`                                                                                                                                                     |

## iOS UIBackgroundModes
For more details about the modes, see https://developer.apple.com/library/archive/documentation/NetworkingInternetWeb/Conceptual/CoreBluetooth_concepts/CoreBluetoothBackgroundProcessingForIOSApps/PerformingTasksWhileYourAppIsInTheBackground.html

Add the following to `UIBackgroundModes`:
- `location` REQUIRED
- `bluetooth-central` required if you want to get the `onBluetoothStateChanged` events
- `bluetooth-central` and `bluetooth-peripheral` required if you want to get ranging info (RSSI and distance)


## Expo
This module will work with the Expo managed workflow. It will not however work with Expo Go, since it needs native features. You can use the development build of Expo to test this module. More about this can be found [here](https://docs.expo.dev/develop/development-builds/create-a-build/). To use this module in expo managed add the following to your app.json:
```json
"expo": {
  "ios": {
    "infoPlist": {
      "NSLocationWhenInUseUsageDescription": "We need your location to detect nearby beacons.",
      "NSLocationAlwaysUsageDescription": "We need your location to detect nearby beacons even when the app is in the background.",
      "NSLocationAlwaysAndWhenInUseUsageDescription": "We need your location to detect nearby beacons even when the app is in the background."
    }
  },
  "plugins": [
    "react-native-beacon-radar"
  ]
}
```

## Contributing

See the [contributing guide](CONTRIBUTING.md) to learn how to contribute to the repository and the development workflow.

## License

MIT

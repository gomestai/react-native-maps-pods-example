```
react-native init ReactNativeMapsPodsExample
rm -rf node_modules
npm i -S react-native@0.49 # there was an issue with the new RN version compartibility
npm i
npm install react-native-maps --save
cd ios
pod init
```

Pod dosyasına gir şuradan sonrasını değiştir : `target 'ProjeIsmi' :

```
rn_path = '../node_modules/react-native'

    pod 'yoga', path: "#{rn_path}/ReactCommon/yoga/yoga.podspec"
    pod 'React', path: rn_path, subspecs: [
      'Core',
      'RCTActionSheet',
      'RCTAnimation',
      'RCTGeolocation',
      'RCTImage',
      'RCTLinkingIOS',
      'RCTNetwork',
      'RCTSettings',
      'RCTText',
      'RCTVibration',
      'RCTWebSocket',
      'BatchedBridge'
    ]

    pod 'GoogleMaps'  # Remove this line if you don't want to support GoogleMaps on iOS
    pod 'react-native-maps', path: '../node_modules/react-native-maps'
    pod 'react-native-google-maps', path: '../node_modules/react-native-maps'  # If you need GoogleMaps support on iOS
end

post_install do |installer|
  installer.pods_project.targets.each do |target|
    if target.name == 'react-native-google-maps'
      target.build_configurations.each do |config|
        config.build_settings['CLANG_ENABLE_MODULES'] = 'No'
      end
    end
    if target.name == "React"
      target.remove_from_project
    end
  end
end
```

Çalıştır:

```
pod install
```

API-KEY şuradan al: https://developers.google.com/maps/documentation/ios-sdk/get-api-key

Add to AppDelegate.m:

```
+ @import GoogleMaps; //add this line
@implementation AppDelegate
...

- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
{

+  [GMSServices provideAPIKey:@"AIzaSyDeKw2TRjlNMeYVApb2tR7hFPjSI_gdf5s"]; // add this line
```

Minimal App.js:

```
import React, { Component } from 'react';
import {
  StyleSheet
} from 'react-native';
import MapView, {PROVIDER_GOOGLE} from 'react-native-maps';

export default class App extends Component {
  render() {
    return (
      <MapView
        provider={PROVIDER_GOOGLE}
        style={StyleSheet.absoluteFillObject}
      >
        <MapView.Marker
          title="Greenwich"
          coordinate={{
            latitude: 51.48,
            longitude: 0
          }}
          calloutOffset={{
            x: -50,
            y: -50
          }}
        />
      </MapView>
    );
  }
}
```

# cwflutter

**Note:** ConfigWise SDK flutter plugin is only supported by mobile devices with A9 or later processors 
(iPhone 6s/7/SE/8/X, iPad 2017/Pro) on iOS 12 and newer.

## Getting Started

This project is a starting point for a Flutter
[plug-in package](https://flutter.dev/developing-packages/),
a specialized package that includes platform-specific implementation code for
Android and/or iOS.

For help getting started with Flutter, view our 
[online documentation](https://flutter.dev/docs), which offers tutorials, 
samples, guidance on mobile development, and a full API reference.

To use this plugin, add `cwflutter` as a [dependency in your pubspec.yaml file](https://flutter.io/platform-plugins/).

Get the [COMPANY_AUTH_TOKEN Credentials](https://manage.configwise.io) for your AR project. For more details go to [https://docs.configwise.io/api/setup-authentication-token//](https://docs.configwise.io/api/setup-authentication-token/)

Import `package:cwflutter/cwflutter.dart`, and initiate `Cwflutter Plugin` with your credentials.

### Integration

```dart
    try {
      await Cwflutter.initialize(
        "YOUR_COMPANY_AUTH_TOKEN",
        1 * 60 * 60,       // 1 hr
        400 * 1024 * 1024, // 400 Mb - we recommend to set 400 Mb or more for androidLowMemoryThreshold
        true
      );
    } on PlatformException catch (e) {
      print('Failed to initialize ConfigWise SDK due error: ${error.message}');
    }

    try {
      await Cwflutter.signIn();
    } on PlatformException catch (e) {
      print('Unable to pass authorization due error: ${error.message}');
    }
```

**NOTICE:** See examples (how to initialize and pass authorization) in our example project - see `_initConfigWiseSdk()` function in the 
[example/lib/main.dart](example/lib/main.dart)  

## Usage

We highly recommend to use our example project (code) to get first experience.
See: [example/lib/main.dart](example/lib/main.dart)

### Depend on it

Follow the [installation instructions](https://pub.dartlang.org/packages/cwflutter#-installing-tab-) from Dart Packages site.

### Update Info.plist

The plugin use native view from ARKit, which is not yet supported by default. To make it work add the following code to `Info.plist`:
```xml
    <key>io.flutter.embedded_views_preview</key>
    <string>YES</string>
```
ARKit uses the device camera, so do not forget to provide the `NSCameraUsageDescription`. You may specify it in `Info.plist` like that:
```xml
    <key>NSCameraUsageDescription</key>
    <string>Describe why your app needs AR here.</string>
```

### Let's add Metal (shader rendering) code to ios Runner in your Flutter project.

Finally, add [NodeRender.metal](example/ios/Runner/NodeRender.metal) file (as `metal` source code) to ios Runner in your Flutter project.
`NodeRender.metal` is [Apple Metal API](https://developer.apple.com/metal/) based code to show 'glow' visualisation of highlighted 3D objects in the AR scene.
Unfortunately, there are some known issues (restrictions) to pack `.metal` code inside of ConfigWiseSDK framework - that's why `.metal` code must 
be part of your project to pass compilation steps.  

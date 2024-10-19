# compass
flutter project

## 개인정보처리방침

## 내질문 : flutter 프로젝트를 새로 시작하게 도와줘.

내 컴퓨터는 'Linux ubuntu 5.15.0-124-generic #134~20.04.1-Ubuntu SMP Tue Oct 1 15:27:33 UTC 2024 x86_64 x86_64 x86_64 GNU/Linux'
내 프로젝트 루트 디렉토리는 '/home/ubuntu/compass' 이야.

## 내질문 : 스마트폰에서 사용할 나침판 앱을 만들어줘.

내 컴퓨터는 'Linux ubuntu 5.15.0-124-generic #134~20.04.1-Ubuntu SMP Tue Oct 1 15:27:33 UTC 2024 x86_64 x86_64 x86_64 GNU/Linux'
내 프로젝트 루트 디렉토리는 '/home/ubuntu/compass' 이야.

네, 스마트폰용 나침판 앱을 Flutter를 사용하여 만들어보겠습니다. 이 앱은 기기의 자력계 센서를 사용하여 방향을 표시할 것입니다. 다음은 기본적인 나침판 앱을 만드는 단계입니다:

1. 먼저, 프로젝트 디렉토리로 이동합니다:

```bash
cd /home/ubuntu/compass
```

2. `pubspec.yaml` 파일을 열어 필요한 패키지를 추가합니다:

```yaml
dependencies:
  flutter:
    sdk: flutter
  flutter_compass: ^0.7.0
  permission_handler: ^10.0.0
```

3. 패키지를 설치합니다:

```bash
flutter pub get
```

4. `lib/main.dart` 파일을 다음과 같이 작성합니다:

```dart
import 'package:flutter/material.dart';
import 'package:flutter_compass/flutter_compass.dart';
import 'dart:math' as math;
import 'package:permission_handler/permission_handler.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: '나침판 앱',
      theme: ThemeData(primarySwatch: Colors.blue),
      home: CompassPage(),
    );
  }
}

class CompassPage extends StatefulWidget {
  @override
  _CompassPageState createState() => _CompassPageState();
}

class _CompassPageState extends State<CompassPage> {
  bool _hasPermissions = false;
  double? _direction;

  @override
  void initState() {
    super.initState();
    _fetchPermissionStatus();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      backgroundColor: Colors.white,
      appBar: AppBar(
        title: const Text('나침판'),
      ),
      body: Builder(builder: (context) {
        if (_hasPermissions) {
          return Column(
            children: <Widget>[
              Expanded(child: _buildCompass()),
            ],
          );
        } else {
          return _buildPermissionSheet();
        }
      }),
    );
  }

  Widget _buildCompass() {
    return StreamBuilder<CompassEvent>(
      stream: FlutterCompass.events,
      builder: (context, snapshot) {
        if (snapshot.hasError) {
          return Text('오류: ${snapshot.error}');
        }

        if (snapshot.connectionState == ConnectionState.waiting) {
          return Center(
            child: CircularProgressIndicator(),
          );
        }

        double? direction = snapshot.data!.heading;

        // if direction is null, then device does not support this sensor
        // show error message
        if (direction == null)
          return Center(
            child: Text("기기가 이 센서를 지원하지 않습니다"),
          );

        return Material(
          shape: CircleBorder(),
          clipBehavior: Clip.antiAlias,
          elevation: 4.0,
          child: Container(
            padding: EdgeInsets.all(16.0),
            alignment: Alignment.center,
            decoration: BoxDecoration(
              shape: BoxShape.circle,
            ),
            child: Transform.rotate(
              angle: (direction * (math.pi / 180) * -1),
              child: Image.asset('assets/compass.png'),
            ),
          ),
        );
      },
    );
  }

  Widget _buildPermissionSheet() {
    return Center(
      child: ElevatedButton(
        child: Text('위치 권한 요청'),
        onPressed: () {
          Permission.locationWhenInUse.request().then((ignored) {
            _fetchPermissionStatus();
          });
        },
      ),
    );
  }

  void _fetchPermissionStatus() {
    Permission.locationWhenInUse.status.then((status) {
      if (mounted) {
        setState(() => _hasPermissions = status == PermissionStatus.granted);
      }
    });
  }
}
```

5. `assets` 폴더를 만들고 나침반 이미지를 추가합니다. 예를 들어, `assets/compass.png` 파일을 추가하세요.

6. `pubspec.yaml` 파일에 assets를 등록합니다:

```yaml
flutter:
  assets:
    - assets/compass.png
```

7. Android 권한 설정을 위해 `android/app/src/main/AndroidManifest.xml` 파일에 다음 줄을 추가합니다:

```xml
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
```

8. iOS 권한 설정을 위해 `ios/Runner/Info.plist` 파일에 다음 줄을 추가합니다:

```xml
<key>NSLocationWhenInUseUsageDescription</key>
<string>이 앱은 나침반 기능을 위해 위치 권한이 필요합니다.</string>
```

이제 앱을 실행할 수 있습니다:

```bash
flutter run
```

이 앱은 기기의 방향에 따라 나침반 이미지를 회전시킵니다. 사용자에게 위치 권한을 요청하고, 권한이 부여되면 나침반을 표시합니다.

참고: 실제 기기에서 테스트해야 합니다. 에뮬레이터에서는 자력계 센서를 시뮬레이션하지 않기 때문에 제대로 작동하지 않을 수 있습니다.

## 내질문 :

내 컴퓨터는 'Linux ubuntu 5.15.0-124-generic #134~20.04.1-Ubuntu SMP Tue Oct 1 15:27:33 UTC 2024 x86_64 x86_64 x86_64 GNU/Linux'
내 프로젝트 루트 디렉토리는 '/home/ubuntu/compass' 이야.


## 내질문 : 
내 컴퓨터는 'Linux ubuntu 5.15.0-124-generic #134~20.04.1-Ubuntu SMP Tue Oct 1 15:27:33 UTC 2024 x86_64 x86_64 x86_64 GNU/Linux'
내 프로젝트 루트 디렉토리는 '/home/ubuntu/compass' 이야.






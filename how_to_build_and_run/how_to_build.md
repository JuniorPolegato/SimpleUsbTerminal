./gradlew build
```
> Task :app:compileReleaseJavaWithJavac
Note: Some input files use or override a deprecated API.
Note: Recompile with -Xlint:deprecation for details.

> Task :app:compileDebugJavaWithJavac
Note: Some input files use or override a deprecated API.
Note: Recompile with -Xlint:deprecation for details.

> Task :app:lintReportDebug
Wrote HTML report to file:///.../SimpleUsbTerminal/app/build/reports/lint-results-debug.html

BUILD SUCCESSFUL in 9s
76 actionable tasks: 76 executed
```

find -name "*apk"
```
./app/build/outputs/apk
./app/build/outputs/apk/debug/app-debug.apk
./app/build/outputs/apk/release/app-release-unsigned.apk
```

keytool -genkey -v -keystore ~/.keystore -keyalg RSA -keysize 2048 -validity 10000 -alias juniorpolegato
```
Enter keystore password:  
Re-enter new password: 
What is your first and last name?
  [Unknown]:  Junior Polegato
What is the name of your organizational unit?
  [Unknown]:  Polegato TI  
What is the name of your organization?
  [Unknown]:  Polegato Digital Systems
What is the name of your City or Locality?
  [Unknown]:  Ribeirão Preto
What is the name of your State or Province?
  [Unknown]:  SP
What is the two-letter country code for this unit?
  [Unknown]:  BR
Is CN=Junior Polegato, OU=Polegato TI, O=Polegato Digital Systems, L=Ribeirão Preto, ST=SP, C=BR correct?
  [no]:  yes

Generating 2,048 bit RSA key pair and self-signed certificate (SHA256withRSA) with a validity of 10,000 days
	for: CN=Junior Polegato, OU=Polegato TI, O=Polegato Digital Systems, L=Ribeirão Preto, ST=SP, C=BR
[Storing /home/junior/.keystore]
```

keytool -list
```
Enter keystore password:  
Keystore type: PKCS12
Keystore provider: SUN

Your keystore contains 1 entry

juniorpolegato, Feb 25, 2024, PrivateKeyEntry, 
Certificate fingerprint (SHA-256): E8:2F:F7:...:AA:4B:47
```

apksigner sign --ks ~/.keystore --out simpleusbserial.apk app/build/outputs/apk/release/app-release-unsigned.apk ; ls -alh *apk
```
Keystore password for signer #1: 
-rw-r--r-- 1 junior junior 4.5M Feb 25 08:02 simpleusbserial.apk
```

Now you can send to your Android device by e-mail, Telegram, WhatsApp, etc.<br>
Or you can send and run in your device or emulator via `adb`.<br>
Devices can be emulators, usb devices or ip devices.<br>
For usb and ip devices, you need to enable developer menu, and then `USB debugging` and/or `Wireless debugging`.<br>
For ip devices, you need to open `Wireless debugging`, pair with code and connect:<br>

adb pair 192.168.18.62:43565
```
Enter pairing code: 524910
Successfully paired to 192.168.18.62:43565 [guid=adb-RQCWA011B9H-ImXST3]
```
adb connect 192.168.18.62:34283
```
connected to 192.168.18.62:34283
```
adb devices
```
List of devices attached
192.168.18.62:34283	device
emulator-5554	device
```
To install signed release in one device: `adb -s 192.168.18.62 install simpleusbserial.apk`
```
Performing Incremental Install
Serving...
All files should be loaded. Notifying the device.
Success
Install command complete in 3107 ms
```
To run, just plug a usb serial device in OTG adaptor, select baudrate and then the USB<=>TTL device.<br>
You also can start it with adb:
```
adb -s 192.168.18.62 shell am start -n de.kai_morich.simple_usb_terminal/de.kai_morich.simple_usb_terminal.MainActivity
Starting: Intent { cmp=de.kai_morich.simple_usb_terminal/.MainActivity }
```
To uninstall: `adb -s 192.168.18.62 uninstall de.kai_morich.simple_usb_terminal`
```
Success
```
For debug version in all devices: `./gradlew installDebug`
```
> Task :app:installDebug
Installing APK 'app-debug.apk' on 'Pixel_4a_API_34(AVD) - 14' for :app:debug
Installing APK 'app-debug.apk' on 'SM-A546E - 14' for :app:debug
Installed on 2 devices.

BUILD SUCCESSFUL in 5s
31 actionable tasks: 1 executed, 30 up-to-date
```
To uninstall: `./gradlew uninstallAll`:
```
> Task :app:uninstallDebug
Uninstalling de.kai_morich.simple_usb_terminal (from app:debug) from device 'Pixel_4a_API_34(AVD) - 14' (emulator-5554).
Uninstalling de.kai_morich.simple_usb_terminal (from app:debug) from device 'SM-A546E - 14' (192.168.18.62:34283).
Uninstalled de.kai_morich.simple_usb_terminal from 2 devices.

> Task :app:uninstallDebugAndroidTest
Uninstalling de.kai_morich.simple_usb_terminal.test (from app:debugAndroidTest) from device 'Pixel_4a_API_34(AVD) - 14' (emulator-5554).
Uninstalling de.kai_morich.simple_usb_terminal.test (from app:debugAndroidTest) from device 'SM-A546E - 14' (192.168.18.62:34283).
Uninstalled de.kai_morich.simple_usb_terminal.test from 2 devices.

> Task :app:uninstallRelease
Uninstalling de.kai_morich.simple_usb_terminal (from app:release) from device 'Pixel_4a_API_34(AVD) - 14' (emulator-5554).
Uninstalling de.kai_morich.simple_usb_terminal (from app:release) from device 'SM-A546E - 14' (192.168.18.62:34283).
Uninstalled de.kai_morich.simple_usb_terminal from 2 devices.
```

Reference: https://developer.android.com/build/building-cmdline

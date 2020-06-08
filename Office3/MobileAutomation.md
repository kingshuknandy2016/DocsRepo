# Mobile Automation

### Initial Setup

* Developer option has to be enabled in the device
* USB Debugging Should be on
* ***Appium*** has to be installed. Desktop version as well as the appium node package.
* Adb command has to be installed

### Steps
* Connect the Mobile Device
* CMD --> adb devices (You will be able to see the devices)

### For Android Ways to get the app package and App activity
##### Method 1:
For Mac/Linux:
```cmd
   adb shell dumpsys window | grep -E 'mCurrentFocus' 
```
For Windows:
```cmd
adb shell dumpsys window | find "mCurrentFocus"
```
##### Method 2:
Trace the entire Mobile Logs
```cmd
   adb logcat
```

### Verify the App package and app name
```cmd
adb shell am start -W -n com.package.name/com.zinier.app.ui.activities.activity -S -a android.intent.action.MAIN -c android.intent.category.LAUNCHER -f 0x10200000
```
If the above command is successful the we have correctly foud the app-package and app-activity




```json
{
  "deviceName": "16898cd3",
  "appPackage": "com.name.app",
  "appActivity": "com..app.ui.activities.login.LoginActivity",
  "platformName": "android"
}
```

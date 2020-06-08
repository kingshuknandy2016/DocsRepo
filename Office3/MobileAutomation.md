# Mobile Automation

### Initial Setup

* Developer option has to be enabled in the device
* USB Debugging Should be on
* ***Appium*** has to be installed. Desktop version as well as the appium node package.
* Adb command has to be installed

Need to install the Appium Application.
For Mac/Linux:

adb shell dumpsys window | grep -E 'mCurrentFocus' 
For Windows:

adb shell dumpsys window | find "mCurrentFocus"

{
  "deviceName": "16898cd3",
  "appPackage": "com.zinier.app",
  "appActivity": "com.zinier.app.ui.activities.login.LoginActivity",
  "platformName": "android"
}

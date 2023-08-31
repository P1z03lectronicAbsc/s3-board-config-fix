# A guide on fixing the Board Confugiration for the ESP32 s3
2nd edit: This problem was thankfully fixed on Arduino IDE 2.2! From now on you can use the "edit" button for the board names and it will remember your preferences.


This guide is for fixing the Arduino IDE if it shows up random boards whenever you connect it through the USB Type C.

You can just force Arduino IDE to recognise your board by locating your *Boards Configuration File*, putting in your VID and PID values, repackage and host the configurations and Point the Arduino IDE to your custom board manager URL.

On my MacBook device I have used `ioreg -p IOUSB -l` after plugging in a device and copy the idVendor, idProduct properties. These are the VID and PID values. On Windows you may open Device Manager, look under the Ports (COM & LPT), right click on your board (it might appear as a COM port) and choose "properties", go to "details" menu, and there from the dropdown menu, select "Hardware Ids". you should see values like VID_XXXX&PID_YYYY. these are your VID and PID. on Linux you may just use "lsusb" and find the vendor ID and PID there.

After finding those you must go to ESP32 board package directory and find boards.txt (https://github.com/espressif/arduino-esp32/blob/master/boards.txt). Find the line that starts with `esp32s3.name=ESP32S3 Dev Module`. There you must modify the PID and VID values with your actual VID and PID values that you collected as mentioned above. Replace `0xXXXX` with your VID and `0xYYYY` with your PID.

## ⚠️ ALTERNATIVELY JUST DOWNLOAD THE "boards.txt" FILE ON THIS REPO
I have already changed the values for the S3 model we use which is "ESP-32-WROOM-1 V1.3. You can just download the boards.txt and pack the whole directory after compressing. **I have changed other board PID and VID values to 0x1111!!!!!!** The details are as follows:

```bash
    +-o USB JTAG/serial debug unit@14300000  <class AppleUSBDevice, id 0x1000011e6, registered, matched, active, busy 0 (2 ms), retain 16>
        {
          "sessionID" = 7895763373992
          "idProduct" = 4097
          "iManufacturer" = 1
          "bDeviceClass" = 239
          "bMaxPacketSize0" = 64
          "bcdDevice" = 257
          "iProduct" = 2
          "iSerialNumber" = 3
          "bNumConfigurations" = 1
          "Bus Power Available" = 250
          "USB Address" = 1
          "Built-In" = No
          "locationID" = 338690048
          "bDeviceSubClass" = 2
          "bcdUSB" = 512
          "USB Product Name" = "USB JTAG/serial debug unit"
          "PortNum" = 3
          "non-removable" = "no"
          "kUSBSerialNumberString" = "EC:DA:3B:8E:3D:81"
          "bDeviceProtocol" = 1
          "AppleUSBAlternateServiceRegistryID" = 4294929872
          "IOCFPlugInTypes" = {"9dc7b780-9ec0-11d4-a54f-000a27052861"="IOUSBHostFamily.kext/Contents/PlugIns/IOUSBLib.bundle"}
          "IOPowerManagement" = {"DevicePowerState"=0,"CurrentPowerState"=3,"CapabilityFlags"=65536,"MaxPowerState"=4,"DriverPowerState"=3}
          "Device Speed" = 1
          "USB Vendor Name" = "Espressif"
          "idVendor" = 12346
          "kUSBCurrentConfiguration" = 1
          "IOGeneralInterest" = "IOCommand is not serializable"
          "kUSBProductString" = "USB JTAG/serial debug unit"
          "USB Serial Number" = "EC:DA:3B:8E:3B:84"
          "kUSBVendorString" = "Espressif"
          "IOClassNameOverride" = "IOUSBDevice"
        }
```

After modifying, you'll need to compress the entire board package directory into a ZIP archive. Make sure the directory structure remains intact so that the Arduino IDE can recognize and install it correctly.

Then you can create a new repository on GitHub, upload the ZIP archive to this repository, then click on the ZIP file inside the repository, then click the "Download" button. Copy the link.

Now you must go to the Arduino IDE, go to `file` -> `preferences`. in the "Additional Boards Manager URLs" field, click on the icon on the right to open a window where you can insert new lines. Add the direct link to your ZIP file and click OK. Go to Tools -> Board -> Boards Manager. then search for ESP32. your custom package should appear in the list. Click "Install". after it installs you are good to go.


# Important Note:
Upon further investigation, we've found that these "random board names" all share the same VID and PID values. The real issue isn't the wrong VID and PID for the ESP32 S3, but that multiple boards have the same values.

A temporary solution is to assign random values to the VID and PID entries for all boards other than the one you primarily use. This ensures the correct auto-detection of your board. However, always remember to revert these changes if you switch to a long-term development on another board. Otherwise, boards like the u-blox NORA-W10 s3 won't be auto-detected.

# How to fix wrong detection of package
This guide is for fixing the Arduino IDE if it shows up random boards whenever you connect it through the USB Type C.

You can just force Arduino IDE to recognise your board by locating your *Boards Configuration File*, putting in your VID and PID values, repackage and host the configurations and Point the Arduino IDE to your custom board manager URL.

On my MacBook device I have used `ioreg -p IOUSB -l` after plugging in a device and copy the idVendor, idProduct properties. These are the VID and PID values. On Windows you may open Device Manager, look under the Ports (COM & LPT), right click on your board (it might appear as a COM port) and choose "properties", go to "details" menu, and there from the dropdown menu, select "Hardware Ids". you should see values like VID_XXXX&PID_YYYY. these are your VID and PID. on Linux you may just use "lsusb" and find the vendor ID and PID there.

After finding those you must go to ESP32 board package directory and find boards.txt (https://github.com/espressif/arduino-esp32/blob/master/boards.txt). Find the line that starts with `esp32s3.name=ESP32S3 Dev Module`. There you must modify the PID and VID values with your actual VID and PID values that you collected as mentioned above. Replace `0xXXXX` with your VID and `0xYYYY` with your PID.

After modifying, you'll need to compress the entire board package directory into a ZIP archive. Make sure the directory structure remains intact so that the Arduino IDE can recognize and install it correctly.

Then you can create a new repository on GitHub, upload the ZIP archive to this repository, then click on the ZIP file inside the repository, then click the "Download" button. Copy the link.

Now you must go to the Arduino IDE, go to `file` -> `preferences`. in the "Additional Boards Manager URLs" field, click on the icon on the right to open a window where you can insert new lines. Add the direct link to your ZIP file and click OK. Go to Tools -> Board -> Boards Manager. then search for ESP32. your custom package should appear in the list. Click "Install". after it installs you are good to go.

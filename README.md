# Connected Plant Kit for experiments

Welcome to the Connected Plant Kit Software repository! Here you can find all software contents for the project, so you can use this code for your experiments, and collaborate with the project by submitting your contributions.

## Getting started

The Plant Kit's compute is based on an ESP32 device, which we will program using Arduino through a Serial connection. Hence, you will need to have these things ready for the hardware to connect successfully to your computer:

### Prerequisites

* Arduino IDE. You can download it [here](https://www.arduino.cc/en/software). The IDE is used to write the device's firmware and to communicate to it to update it.
* [FTDI USB-Serial drivers](https://ftdichip.com/drivers/): Used to communicate with your device using a Serial connection over USB.
* A WiFi connection nearby your device.
* An AWS Account with permissions to create AWS IoT Core resources.

### Configuration

The device firmware contents are in `packages/device`. In there you'll find two files - i.e. `config.sample.h` and `secrets.sample.h`. You need to modify these files and enter your settings. Then, save them as `config.h` and `secrets.h`, respectively.

```h
// config.h

const char WIFI_SSID[] =          "YOUR_WIFI_SSID_HERE"; // Enter the SSID of your WiFi so the device can connect to it
const char WIFI_PASSWORD[] =      "YOUR_WIFI_PASSWORD_HERE"; // The password of your WiFi network

const char AWS_IOT_ENDPOINT[] =   "YOUR_AWS_IOT_ENDPOINT_HERE"; // The IoT endpoint address for your AWS account and desired region.
const char AWS_IOT_THING_NAME[] = "YOUR_CAMERA_NAME_HERE"; // Enter the name of your device in AWS IoT. 

```

```h
// secrets.h
// ...

// Amazon Root CA 1
// Get from https://docs.aws.amazon.com/iot/latest/developerguide/server-authentication.html#server-authentication-certs
static const char AWS_CERT_CA[] PROGMEM = R"EOF(
-----BEGIN CERTIFICATE-----

PASTE YOUR ROOT CA CONTENTS HERE

-----END CERTIFICATE-----
)EOF";

// Device Certificate
static const char AWS_CERT_CRT[] PROGMEM = R"KEY(
-----BEGIN CERTIFICATE-----

PASTE YOUR CERTIFICATE CONTENTS HERE

-----END CERTIFICATE-----
)KEY";

// Device Private Key
static const char AWS_CERT_PRIVATE[] PROGMEM = R"KEY(
-----BEGIN RSA PRIVATE KEY-----

PASTE YOUR PRIVATE KEY CONTENTS HERE

-----END RSA PRIVATE KEY-----
)KEY";

```

### Firmware uploading

Once you've created your settings file, you can upload the firmware to your device. To do so, plug the device to the computer through the serial programmer, and reset it into download mode by connecting the `RST` and `IO0` pins and resetting. Then, click _Upload_ on the Arduino IDE.

Once upload finishes, remove the jumper connection for the download mode, and reset again the device. It should immediately start the execution process, connect to your WiFi and your AWS IoT broker, and submit its status.

### Verification

To verify your device is correctly connected to WiFi, you can check the logs using Arduino's _Serial Monitor_, which will inform you about the connectivity status and the IP of the device, once connected. Alternatively, you can verify your router.

To verify the device is transmitting information correctly to AWS IoT, you can check the device shadow of your IoT Thing, which should reflect the latest status of the device.

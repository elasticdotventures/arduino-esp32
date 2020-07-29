Installation instructions for using PlatformIO
=================================================

- [What is PlatformIO?](https://docs.platformio.org/en/latest/what-is-platformio.html?utm_source=github&utm_medium=arduino-esp32)
- [PlatformIO IDE](https://platformio.org/platformio-ide?utm_source=github&utm_medium=arduino-esp32)
- [PlatformIO Core](https://docs.platformio.org/en/latest/core.html?utm_source=github&utm_medium=arduino-esp32) (command line tool)
- [Advanced usage](https://docs.platformio.org/en/latest/platforms/espressif32.html?utm_source=github&utm_medium=arduino-esp32) -
  custom settings, uploading to SPIFFS, Over-the-Air (OTA), staging version
- [Integration with Cloud and Standalone IDEs](https://docs.platformio.org/en/latest/ide.html?utm_source=github&utm_medium=arduino-esp32) -
  Cloud9, Codeanywhere, Eclipse Che (Codenvy), Atom, CLion, Eclipse, Emacs, NetBeans, Qt Creator, Sublime Text, VIM, Visual Studio, and VSCode
- [Project Examples](https://docs.platformio.org/en/latest/platforms/espressif32.html?utm_source=github&utm_medium=arduino-esp32#examples)

## Notes about espidf 4.0 & platform.io
 
It is possible (but uncommon) to use the arduino compatibility library `<Arduino.h>` with platformio.ini setting `framework=espidf`.

In this configuration

### platformio.ini
First install this repo (change the branch to match the espidf version you are using)
```bash
cd $platform/lib
git submodule add -b idf-release/v4.0 https://github.com/espressif/arduino-esp32.git
```

```platformio.ini
;
; note - this installs/works with: 
; PLATFORM: Espressif 32 1.12.4 #72b15f6 > AI Thinker ESP32-CAM
; PACKAGES:
; - arduinoespressif32 475208e
; - framework-espidf 3.40001.200521 (4.0.1)
; - tool-cmake 3.16.4
; - tool-esptoolpy 1.20600.0 (2.6.0)
; - tool-idf 1.0.1
; - tool-mconf 1.4060000.20190628 (406.0.0)
; - tool-ninja 1.9.0
; - toolchain-esp32ulp 1.22851.190618 (2.28.51)
; - toolchain-xtensa32 2.80200.200226 (8.2.0)
;
platform=https://github.com/platformio/platform-espressif32.git#develop

framework=espidf,arduino
  
platform_packages =
  framework-arduinoespressif32 @ https://github.com/elasticdotventures/arduino-esp32.git#idf-release/v4.0 
 
```

### Possible Build Error:

```
_\.platformio\packages\arduinoespressif32\libraries\WiFiClientSecure\src\ssl_client.cpp:23:4: error: #error "Please configure IDF framework to include mbedTLS -> Enable pre-shared-key ciphersuites and activate at least one cipher"
 #  error "Please configure IDF framework to include mbedTLS -> Enable pre-shared-key ciphersuites and activate at 
least one cipher"
Enable CMAC mode for block ciphers   
```
Solution: Component config -> mbedTLS -> TLS Key Exchange Methods -> [*] Enable pre-shared-key ciphersuites

### Possible Build Error:

```
__\.platformio\packages\arduinoespressif32\libraries\WiFi\src\ETH.cpp: In member function 'bool ETHClass::begin(uint8_t, int, int, int, eth_phy_type_t)':
__\.platformio\packages\arduinoespressif32\libraries\WiFi\src\ETH.cpp:160:19: error: 'esp_eth_mac_new_esp32' was not declared in this scope
         eth_mac = esp_eth_mac_new_esp32(&mac_config);
                   ^~~~~~~~~~~~~~~~~~~~~
```
Solution: Component config -> Ethernet -> Use ESP32_EMAC & USE_SPI_ETHERNET  (not sure exactly which ones)

                   

# Pi4J V2 :: Minimal "FAT JAR" example application
==================================================

GitHub Actions: ![Maven build](https://github.com/pi4j/pi4j-example-fatjar/workflows/Maven/badge.svg)

This project contains a minimal example application which uses the Pi4J (V2) library and uses a digital output (LED) 
and digital input (push button). Full description is available on [the Pi4J website](https://v2.pi4j.com/getting-started/minimal-example-application)

## PROJECT OVERVIEW

The goal of the example project is to show how to set up a Pi4J Maven / Gradle project for the Raspberry Pi.

## MAVEN SHADE PLUGIN

Because of the use of the ServiceLoader to dynamically load the plugins which are part of the project, the FAT JAR created
by this code, all the files in `META-INF/services` need to be merged into one. This is not possible with the `Maven Assembly
Plugin` that could be used with Pi4J V1.

To create a correct FAT JAR the `Maven Shade Plugin` needs to be used, with the additional 
[ServicesResourceTransformer](https://maven.apache.org/plugins/maven-shade-plugin/examples/resource-transformers.html#ServicesResourceTransformer). 
As described on the Apache documentation page: *JAR files providing implementations of some interfaces often ship with 
a META-INF/services/ directory that maps interfaces to their implementation classes for lookup by the service locator. 
To relocate the class names of these implementation classes, and to merge multiple implementations of the same interface 
into one service entry, the ServicesResourceTransformer can be used.*

## WIRING

The application needs a LED connected on BCM 22 and button on BCM 24. 

![Breadboard schematics used in this example](assets/led-button_bb.png)

## RUNTIME DEPENDENCIES

This project uses Pi4J V.2 which has the following runtime dependency requirements:
- [**SLF4J (API)**](https://www.slf4j.org/)
- [**SLF4J-SIMPLE**](https://www.slf4j.org/)
- [**PIGPIO Library**](http://abyz.me.uk/rpi/pigpio) (for the Raspberry Pi) - This 
dependency comes pre-installed on recent Raspbian images.  However, you can also 
download and install it yourself using the instructions found 
[here](http://abyz.me.uk/rpi/pigpio/download.html).

## MAVEN BUILD DEPENDENCIES & INSTRUCTIONS

This project can be built with [Apache Maven](https://maven.apache.org/) 3.6 
(or later) and Java 11 OpenJDK (or later). These prerequisites must be installed 
prior to building this project. The following command can be used to download 
all project dependencies and compile the Java module. You can build this 
project on a development PC or directly on a Raspberry Pi with Java 11+.  

Build with:

```
mvn clean package
```

Once the build is complete and was successful, you can find the compiled FAT JAR `pi4j-example-fatjar.jar` in the
`target` directory. You can build directly on your Raspberry Pi or if you are developing on a different computer, copy the file 
to your Raspberry Pi with (in this example the Pi has IP 192.168.0.252):

```
scp target/pi4j-example-fatjar.jar pi@192.168.0.252://home/pi
```

On the Raspberry Pi open a terminal, or via SSH from your PC, execute this command:

```
$ sudo java -jar pi4j-example-fatjar.jar 

[main] INFO com.pi4j.util.Console - 

[main] INFO com.pi4j.util.Console - ************************************************************
[main] INFO com.pi4j.util.Console - ************************************************************
[main] INFO com.pi4j.util.Console - 
[main] INFO com.pi4j.util.Console -                   <-- The Pi4J Project -->                  
[main] INFO com.pi4j.util.Console -                    Minimal Example project                  
[main] INFO com.pi4j.util.Console - 
[main] INFO com.pi4j.util.Console - ************************************************************
[main] INFO com.pi4j.util.Console - ************************************************************
[main] INFO com.pi4j.util.Console - 
[main] INFO com.pi4j.Pi4J - New auto context
[main] INFO com.pi4j.Pi4J - New context builder
[main] INFO com.pi4j.platform.impl.DefaultRuntimePlatforms - adding platform to managed platform map [id=raspberrypi; name=RaspberryPi Platform; priority=5; class=com.pi4j.plugin.raspberrypi.platform.RaspberryPiPlatform]
[main] INFO com.pi4j.util.Console - --------------------
[main] INFO com.pi4j.util.Console - |  Pi4J PLATFORMS  |
[main] INFO com.pi4j.util.Console - --------------------
[main] INFO com.pi4j.util.Console - 
PLATFORMS: [1] "Pi4J Runtime Platforms" <com.pi4j.platform.impl.DefaultPlatforms> 
└─PLATFORM: "RaspberryPi Platform" {raspberrypi} <com.pi4j.plugin.raspberrypi.platform.RaspberryPiPlatform> {Pi4J Platform for the RaspberryPi series of products.} 
[main] INFO com.pi4j.util.Console - 
[main] INFO com.pi4j.util.Console - ---------------------------
[main] INFO com.pi4j.util.Console - |  Pi4J DEFAULT PLATFORM  |
[main] INFO com.pi4j.util.Console - ---------------------------
[main] INFO com.pi4j.util.Console - 
PLATFORM: "RaspberryPi Platform" {raspberrypi} <com.pi4j.plugin.raspberrypi.platform.RaspberryPiPlatform> {Pi4J Platform for the RaspberryPi series of products.} 
[main] INFO com.pi4j.util.Console - 
[main] INFO com.pi4j.util.Console - --------------------
[main] INFO com.pi4j.util.Console - |  Pi4J PROVIDERS  |
[main] INFO com.pi4j.util.Console - --------------------
[main] INFO com.pi4j.util.Console - 
PROVIDERS: [12] "I/O Providers" <com.pi4j.provider.impl.DefaultProviders> 
├─SPI: [2] <com.pi4j.io.spi.SpiProvider> 
│ ├─PROVIDER: "PiGpio SPI Provider" {pigpio-spi} <com.pi4j.plugin.pigpio.provider.spi.PiGpioSpiProviderImpl> {com.pi4j.plugin.pigpio.provider.spi.PiGpioSpiProviderImpl} 
│ └─PROVIDER: "RaspberryPi SPI Provider" {raspberrypi-spi} <com.pi4j.plugin.raspberrypi.provider.spi.RpiSpiProviderImpl> {com.pi4j.plugin.raspberrypi.provider.spi.RpiSpiProviderImpl} 
├─ANALOG_INPUT: [0] <com.pi4j.io.gpio.analog.AnalogInputProvider> 
├─SERIAL: [2] <com.pi4j.io.serial.SerialProvider> 
│ ├─PROVIDER: "PiGpio Serial Provider" {pigpio-serial} <com.pi4j.plugin.pigpio.provider.serial.PiGpioSerialProviderImpl> {com.pi4j.plugin.pigpio.provider.serial.PiGpioSerialProviderImpl} 
│ └─PROVIDER: "RaspberryPi Serial Provider" {raspberrypi-serial} <com.pi4j.plugin.raspberrypi.provider.serial.RpiSerialProviderImpl> {com.pi4j.plugin.raspberrypi.provider.serial.RpiSerialProviderImpl} 
├─DIGITAL_INPUT: [2] <com.pi4j.io.gpio.digital.DigitalInputProvider> 
│ ├─PROVIDER: "RaspberryPi Digital Input (GPIO) Provider" {raspberrypi-digital-input} <com.pi4j.plugin.raspberrypi.provider.gpio.digital.RpiDigitalInputProviderImpl> {com.pi4j.plugin.raspberrypi.provider.gpio.digital.RpiDigitalInputProviderImpl} 
│ └─PROVIDER: "PiGpio Digital Input (GPIO) Provider" {pigpio-digital-input} <com.pi4j.plugin.pigpio.provider.gpio.digital.PiGpioDigitalInputProviderImpl> {com.pi4j.plugin.pigpio.provider.gpio.digital.PiGpioDigitalInputProviderImpl} 
├─I2C: [2] <com.pi4j.io.i2c.I2CProvider> 
│ ├─PROVIDER: "RaspberryPi I2C Provider" {raspberrypi-i2c} <com.pi4j.plugin.raspberrypi.provider.i2c.RpiI2CProviderImpl> {com.pi4j.plugin.raspberrypi.provider.i2c.RpiI2CProviderImpl} 
│ └─PROVIDER: "PiGpio I2C Provider" {pigpio-i2c} <com.pi4j.plugin.pigpio.provider.i2c.PiGpioI2CProviderImpl> {com.pi4j.plugin.pigpio.provider.i2c.PiGpioI2CProviderImpl} 
├─ANALOG_OUTPUT: [0] <com.pi4j.io.gpio.analog.AnalogOutputProvider> 
├─DIGITAL_OUTPUT: [2] <com.pi4j.io.gpio.digital.DigitalOutputProvider> 
│ ├─PROVIDER: "RaspberryPi Digital Output (GPIO) Provider" {raspberrypi-digital-output} <com.pi4j.plugin.raspberrypi.provider.gpio.digital.RpiDigitalOutputProviderImpl> {com.pi4j.plugin.raspberrypi.provider.gpio.digital.RpiDigitalOutputProviderImpl} 
│ └─PROVIDER: "PiGpio Digital Output (GPIO) Provider" {pigpio-digital-output} <com.pi4j.plugin.pigpio.provider.gpio.digital.PiGpioDigitalOutputProviderImpl> {com.pi4j.plugin.pigpio.provider.gpio.digital.PiGpioDigitalOutputProviderImpl} 
└─PWM: [2] <com.pi4j.io.pwm.PwmProvider> 
  ├─PROVIDER: "RaspberryPi PWM Provider" {raspberrypi-pwm} <com.pi4j.plugin.raspberrypi.provider.pwm.RpiPwmProviderImpl> {com.pi4j.plugin.raspberrypi.provider.pwm.RpiPwmProviderImpl} 
  └─PROVIDER: "PiGpio PWM Provider" {pigpio-pwm} <com.pi4j.plugin.pigpio.provider.pwm.PiGpioPwmProviderImpl> {com.pi4j.plugin.pigpio.provider.pwm.PiGpioPwmProviderImpl} 
[main] INFO com.pi4j.util.Console - 
[main] INFO com.pi4j.util.Console - -------------------
[main] INFO com.pi4j.util.Console - |  Pi4J REGISTRY  |
[main] INFO com.pi4j.util.Console - -------------------
[main] INFO com.pi4j.util.Console - 
REGISTRY: [2] "I/O Registered Instances" <com.pi4j.registry.impl.DefaultRegistry> 
├─IO: "LED Flasher" {led} <com.pi4j.plugin.pigpio.provider.gpio.digital.PiGpioDigitalOutput> {DOUT-22} 
└─IO: "Press button" {button} <com.pi4j.plugin.pigpio.provider.gpio.digital.PiGpioDigitalInput> {DIN-24} 
[main] INFO com.pi4j.util.Console - 
[main] INFO com.pi4j.util.Console - LED high
[main] INFO com.pi4j.util.Console - LED low
[main] INFO com.pi4j.util.Console - LED high
[main] INFO com.pi4j.util.Console - LED low
[main] INFO com.pi4j.util.Console - LED high
...
[main] INFO com.pi4j.util.Console - LED high
[Thread-1] INFO com.pi4j.util.Console - Button was pressed for the 1th time
[main] INFO com.pi4j.util.Console - LED low
[main] INFO com.pi4j.util.Console - LED high
[main] INFO com.pi4j.util.Console - LED low
[main] INFO com.pi4j.util.Console - LED high
[main] INFO com.pi4j.util.Console - LED low
[main] INFO com.pi4j.util.Console - LED high
[Thread-3] INFO com.pi4j.util.Console - Button was pressed for the 2th time
[main] INFO com.pi4j.util.Console - LED low
[main] INFO com.pi4j.util.Console - LED high
...
```

## LICENSE

 Pi4J Version 2.0 and later is licensed under the Apache License,
 Version 2.0 (the "License"); you may not use this file except in
 compliance with the License.  You may obtain a copy of the License at:
      http://www.apache.org/licenses/LICENSE-2.0

 Unless required by applicable law or agreed to in writing, software
 distributed under the License is distributed on an "AS IS" BASIS,
 WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
 See the License for the specific language governing permissions and
 limitations under the License.


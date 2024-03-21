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
$ java -jar pi4j-example-fatjar.jar 

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
[main] INFO com.pi4j.runtime.impl.DefaultRuntime - Initializing Pi4J context/runtime...
[main] WARN com.pi4j.runtime.impl.DefaultRuntime - Replacing provider DIGITAL_OUTPUT RaspberryPi Digital Output (GPIO) Provider with priority 0 with provider GpioD Digital Output (GPIO) Provider with higher priority 150
[main] WARN com.pi4j.runtime.impl.DefaultRuntime - Replacing provider DIGITAL_INPUT RaspberryPi Digital Input (GPIO) Provider with priority 0 with provider GpioD Digital Input (GPIO) Provider with higher priority 150
[main] INFO com.pi4j.library.gpiod.internal.GpioDContext - Using chip gpiochip0 pinctrl-bcm2711
[main] INFO com.pi4j.platform.impl.DefaultRuntimePlatforms - adding platform to managed platform map [id=raspberrypi; name=RaspberryPi Platform; priority=5; class=com.pi4j.plugin.raspberrypi.platform.RaspberryPiPlatform]
[main] INFO com.pi4j.runtime.impl.DefaultRuntime - Pi4J context/runtime successfully initialized.
[main] INFO com.pi4j.util.Console - --------------------
[main] INFO com.pi4j.util.Console - |  Pi4J PLATFORMS  |
[main] INFO com.pi4j.util.Console - --------------------
[main] INFO com.pi4j.util.Console - 
PLATFORMS: [1] "Pi4J Runtime Platforms" <com.pi4j.platform.impl.DefaultPlatforms> 
    PLATFORM: "RaspberryPi Platform" {raspberrypi} <com.pi4j.plugin.raspberrypi.platform.RaspberryPiPlatform> {Pi4J Platform for the RaspberryPi series of products.} 
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
PROVIDERS: [6] "I/O Providers" <com.pi4j.provider.impl.DefaultProviders> 
    DIGITAL_INPUT: [1] <com.pi4j.io.gpio.digital.DigitalInputProvider> 
       PROVIDER: "GpioD Digital Input (GPIO) Provider" {gpiod-digital-input} <com.pi4j.plugin.gpiod.provider.gpio.digital.GpioDDigitalInputProviderImpl> {com.pi4j.plugin.gpiod.provider.gpio.digital.GpioDDigitalInputProviderImpl} 
    SERIAL: [1] <com.pi4j.io.serial.SerialProvider> 
       PROVIDER: "RaspberryPi Serial Provider" {raspberrypi-serial} <com.pi4j.plugin.raspberrypi.provider.serial.RpiSerialProviderImpl> {com.pi4j.plugin.raspberrypi.provider.serial.RpiSerialProviderImpl} 
    I2C: [1] <com.pi4j.io.i2c.I2CProvider> 
       PROVIDER: "RaspberryPi I2C Provider" {raspberrypi-i2c} <com.pi4j.plugin.raspberrypi.provider.i2c.RpiI2CProviderImpl> {com.pi4j.plugin.raspberrypi.provider.i2c.RpiI2CProviderImpl} 
    DIGITAL_OUTPUT: [1] <com.pi4j.io.gpio.digital.DigitalOutputProvider> 
       PROVIDER: "GpioD Digital Output (GPIO) Provider" {gpiod-digital-output} <com.pi4j.plugin.gpiod.provider.gpio.digital.GpioDDigitalOutputProviderImpl> {com.pi4j.plugin.gpiod.provider.gpio.digital.GpioDDigitalOutputProviderImpl} 
    SPI: [1] <com.pi4j.io.spi.SpiProvider> 
       PROVIDER: "RaspberryPi SPI Provider" {raspberrypi-spi} <com.pi4j.plugin.raspberrypi.provider.spi.RpiSpiProviderImpl> {com.pi4j.plugin.raspberrypi.provider.spi.RpiSpiProviderImpl} 
    ANALOG_OUTPUT: [0] <com.pi4j.io.gpio.analog.AnalogOutputProvider> 
    ANALOG_INPUT: [0] <com.pi4j.io.gpio.analog.AnalogInputProvider> 
    PWM: [1] <com.pi4j.io.pwm.PwmProvider> 
      PROVIDER: "RaspberryPi PWM Provider" {raspberrypi-pwm} <com.pi4j.plugin.raspberrypi.provider.pwm.RpiPwmProviderImpl> {com.pi4j.plugin.raspberrypi.provider.pwm.RpiPwmProviderImpl} 
[main] INFO com.pi4j.util.Console - 
[main] INFO com.pi4j.util.Console - -------------------
[main] INFO com.pi4j.util.Console - |  Pi4J REGISTRY  |
[main] INFO com.pi4j.util.Console - -------------------
[main] INFO com.pi4j.util.Console - 
REGISTRY: [2] "I/O Registered Instances" <com.pi4j.registry.impl.DefaultRegistry> 
    IO: "Press button" {button} <com.pi4j.plugin.gpiod.provider.gpio.digital.GpioDDigitalInput> {DIN-24} 
    IO: "DOUT-22" {DOUT-22} <com.pi4j.plugin.gpiod.provider.gpio.digital.GpioDDigitalOutput> {DOUT-22} 
[main] INFO com.pi4j.util.Console - 
[main] INFO com.pi4j.util.Console - LED high
[main] INFO com.pi4j.util.Console - LED low
[main] INFO com.pi4j.util.Console - LED high
[main] INFO com.pi4j.util.Console - LED low
[Pi4J.RUNTIME-1] INFO com.pi4j.util.Console - Button was pressed for the 1th time
[main] INFO com.pi4j.util.Console - LED high
[main] INFO com.pi4j.util.Console - LED low
[main] INFO com.pi4j.util.Console - LED high
[main] INFO com.pi4j.util.Console - LED low
[Pi4J.RUNTIME-1] INFO com.pi4j.util.Console - Button was pressed for the 2th time
[main] INFO com.pi4j.util.Console - LED high
[main] INFO com.pi4j.util.Console - LED low
[main] INFO com.pi4j.util.Console - LED high
[main] INFO com.pi4j.util.Console - LED low
[main] INFO com.pi4j.util.Console - LED high
[Pi4J.RUNTIME-1] INFO com.pi4j.util.Console - Button was pressed for the 3th time
[main] INFO com.pi4j.util.Console - LED low
[main] INFO com.pi4j.util.Console - LED high
[main] INFO com.pi4j.util.Console - LED low
[Pi4J.RUNTIME-1] INFO com.pi4j.util.Console - Button was pressed for the 4th time
[main] INFO com.pi4j.util.Console - LED high
[main] INFO com.pi4j.util.Console - LED low
[main] INFO com.pi4j.util.Console - LED high
[main] INFO com.pi4j.util.Console - LED low
[main] INFO com.pi4j.util.Console - LED high
[Pi4J.RUNTIME-1] INFO com.pi4j.util.Console - Button was pressed for the 5th time
[main] INFO com.pi4j.runtime.impl.DefaultRuntime - Shutting down Pi4J context/runtime...
[main] INFO com.pi4j.plugin.gpiod.provider.gpio.digital.GpioDDigitalInput - Shutdown input listener for button
[main] INFO com.pi4j.util.ExecutorPool - Shutting down executor pool Pi4J.RUNTIME
[main] INFO com.pi4j.runtime.impl.DefaultRuntime - Pi4J context/runtime successfully shutdown. Dispatching shutdown event.
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


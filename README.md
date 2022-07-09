# About

quantum-hr100-cc2530 - fan controller board with mains power and zigbee

quantum-hr100-cc2530 is a open source hardware board.
It controls pendulum fan Quantum HR 100 over zigbee.

# Warning

This project uses dangerous mains voltage. Be carefull.

## Structure

 * Terminal blocks for input power, fan supply and pwm signal
 * non isolated ACDC power supply
 * cc2530 rf-module
 * switchable fan supply with triac
 * cc2530 programming connector
 * i2c connector for e.g. temperature/humidity sensor

## Quantum HR 100

The pendulum fan Quantum HR 100 from aerauliqa has a two direction fan motor with pwm control unit.

The original pwm control works with 10Hz. A duty cycle of 50% stops the motor from running.
Maximum tested pwm frequency with 50% duty cylce to stop the motor was 400Hz.

The pwm input circuit of the motor clamps the voltage to 2.65V with about 5mA current at 3.3V supply voltage.

The pwm control voltage threshols are:
on-threshold: 2.0V
off-threshold: 1.0V

The pwm duty cycle thresholds are:
<= 43% on
>= 47% off
<= 53% off
>= 57% on

PWM frequency of 4Hz, 8Hz, 15Hz didn't work. 30Hz works fine.

## cc2530

CC2530 SZ1V5 module
30x20x4mm pitch 1.25mm

GND   1    24 RST#
GND   2    23 P0_0
3V3   3    22 P0_1
3V3   4    21 P0_2
P2_2  5    20 P0_3
P2_1  6    19 P0_4
P2_0  7    18 P0_5
P1_7  8    17 P0_6
P1_6  9    16 P0_7
P1_5 10    15 P1_0
P1_4 11    14 P1_1
P1_3 12    13 P1_2

A working configurable firmware for the zigbee module can be found at:
github.com/ptvoinfo/zigbee-configurable-firmware

## programming cc2530

### Raspberry Pi 1B

www.zigbee2mqtt.io/guide/adapters/flashing/alternative_flashing_methods.html

Rasphberry Pi need wiringPi (apt install wiringPi) - includes command gpio: "gpio readall" shows physical connector pins to wiring number mapping (wPi)

physical pin 13 -> wPi 2 (clock) -> DEBUG_CLK (DC) cc2530_P2_2
physical pin 15 -> wPi 3 (data) -> DEBUG_DATA (DD) cc2530_P2_1
physical pin 18 -> wPI 5 (reset) -> cc2530_RST#

 * read chip ID: cc_chipid -c 2 -d 3 -r 5
 * read firmware: cc_read -c 2 -d 3 -r 5
 * write firemware: cc_write -c 2 -d 3 -r 5 <firmware-hex-file>

### USB programmer

2x5 pin header assignment:
GND  1  2 VDD (3V3)
DC   3  4 DD
CS#  5  6 SCLK
RST# 7  8 MOSI
VDD  9 10 MISO


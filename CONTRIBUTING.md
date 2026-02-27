# Contributing

> **This project was discontinued in September 2018 and is no longer actively maintained.**
> The repository is preserved as a historical reference. No new features or bug fixes are planned.

## Historical Build Information

This project was built using the following tools and technologies:

- **Language:** Arduino C++ (`.ino` sketch)
- **Platform:** Arduino (ATmega-based) with Ethernet Shield (W5100/SPI)
- **Protocols:** HTTP/1.1 web server, MJPEG stream
- **Hardware:** Arduino board, Ethernet Shield, H-bridge motor driver (L298N or similar), two DC motors, IP camera

### Build Steps (Historical)

1. Install the [Arduino IDE](https://www.arduino.cc/en/software)
2. Open `main.ino` in the Arduino IDE
3. Update the static IP address (`192.168.1.101`) and camera IP (`192.168.1.200`) to match your network
4. Upload to the Arduino board
5. Open a browser and navigate to `http://<arduino-ip>`

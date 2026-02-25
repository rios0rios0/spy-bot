<h1 align="center">Spy Bot</h1>
<p align="center">
    <a href="https://github.com/rios0rios0/spy-bot/releases/latest">
        <img src="https://img.shields.io/github/release/rios0rios0/spy-bot.svg?style=for-the-badge&logo=github" alt="Latest Release"/></a>
</p>

An Arduino-based surveillance robot that hosts a web server for real-time motor control and IP camera viewing from any browser on the local network. The Arduino Ethernet shield serves an HTML control panel with directional buttons that drive four GPIO-connected motor outputs, while an embedded iframe streams live video from a networked IP camera with pan/tilt control.

**Language:** Arduino C++ | **Platform:** Arduino with Ethernet Shield | **Status:** Discontinued (2018-09-13)

## Features

- **Embedded web server** -- runs an HTTP server on port 80 at a static IP (`192.168.1.101`) using the Arduino Ethernet library, serving a full HTML control interface to any connected browser
- **Directional motor control** -- four movement directions (forward, backward, left, right) plus an off command, controlled via HTTP query parameters (`?go=F`, `?go=T`, `?go=E`, `?go=D`, `?go=O`)
- **H-bridge motor wiring** -- drives two DC motors through digital pins 4, 5, 6, and 7, using pin combinations to achieve forward, reverse, left turn, and right turn:
  - Forward: pins 7 + 5 HIGH
  - Backward: pins 4 + 6 HIGH
  - Left: pins 7 + 4 HIGH
  - Right: pins 6 + 5 HIGH
  - Off: all pins LOW
- **IP camera integration** -- embeds a live MJPEG video stream from a networked IP camera (`192.168.1.200:81`) via an iframe, with pan/tilt controls that send CGI commands (`decoder_control.cgi`) to the camera
- **Touch-friendly controls** -- movement buttons support both mouse events (`onmousedown`/`onmouseup`) and touch events (`ontouchstart`/`ontouchend`) for mobile browser compatibility
- **Hold-to-move interface** -- motors activate on button press and stop on release, providing intuitive real-time control

## Technologies

| Component | Technology |
|-----------|------------|
| Microcontroller | Arduino (ATmega-based) |
| Networking | Arduino Ethernet Shield (W5100) with SPI |
| Protocol | HTTP/1.1 web server |
| Motor driver | H-bridge via 4 digital GPIO pins |
| Camera | IP camera with CGI control interface (MJPEG stream) |
| Client interface | Inline HTML/CSS/JavaScript served by Arduino |

## Project Structure

```
main.ino              # Arduino sketch: Ethernet server setup, HTTP request parsing,
                      #   GPIO motor control, and inline HTML page generation
files/
  index.html          # Standalone HTML control panel prototype (SecurityBot v1.0)
                      #   with camera pan/tilt and robot movement controls
  testehtml.html      # Test page for hold-to-click button behavior
  hold.js             # JavaScript library implementing long-press detection
                      #   with configurable delay (300ms) and event prevention
```

## Technical Details

### HTTP Request Parsing

The Arduino reads incoming HTTP requests character by character, buffering characters that follow a `G` (the start of `GET`). Every 10 characters, it checks the buffer for query parameter patterns (`?go=E`, `?go=D`, `?go=F`, `?go=T`, `?go=O`) and activates the corresponding motor pins. This lightweight parser avoids string allocation overhead on the constrained microcontroller.

### Motor Control Logic

The robot uses a differential-drive scheme with two DC motors. Each motor has two control pins (one for each direction). Turning is achieved by running one motor forward and the other backward (e.g., left turn activates pins 7 and 4, spinning the motors in opposite directions).

### Network Architecture

```
[Browser] --HTTP--> [Arduino @ 192.168.1.101:80] --GPIO--> [H-Bridge] --> [Motors]
    |
    +--iframe/CGI--> [IP Camera @ 192.168.1.200:81] --> [MJPEG Stream]
```

## Installation

### Hardware Requirements

- Arduino board (Uno, Mega, or compatible)
- Arduino Ethernet Shield (W5100-based)
- H-bridge motor driver (L298N or similar)
- Two DC motors
- IP camera with CGI control support (optional, for video stream)
- Power supply for motors

### Software Setup

1. Install the [Arduino IDE](https://www.arduino.cc/en/software)
2. Open `main.ino` in the Arduino IDE
3. Update the static IP address (`192.168.1.101`) and camera IP (`192.168.1.200`) to match your network
4. Update the MAC address if needed
5. Upload to the Arduino board
6. Open a browser and navigate to `http://192.168.1.101`

## Contributing

This project is no longer actively maintained. You may submit small bug fixes or documentation improvements via Pull Request, but reviews may be infrequent and contributions are not guaranteed to be merged.
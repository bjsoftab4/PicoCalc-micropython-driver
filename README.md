

# MicroPython Drivers for PicoCalc
Forked from https://github.com/zenodante/PicoCalc-micropython-driver
For more information, see the original web page.

## Difference from orignal
Add RGB565 support for VT100 terminal
(Under develop)

## Build Instructions
In my system something is wrong.\
I added micropython.cmake in parent directory.  And
```sh
cd micropython/ports/rp2
make USER_C_MODULES="Path/To/PicoCalc-micropython-driver/micropython.cmake" \
  BOARD=[TARGET_BOARD]
```

Supported `TARGET_BOARD` values:
- `RPI_PICO`
- `RPI_PICO2`
- `RPI_PICO2_W`  (tested)

---

## Features (Modified)

### VT100 Emulator Mode
- Runs in **16-bit color ** mode.  Change boot.py as followed.
```sh
import framebuf
try:
#    pc_display = PicoDisplay(320, 320)
    pc_display = PicoDisplay(320, 320,framebuf.RGB565)
```
---

## Credits
- [zenodante/PicoCalc-micropython-driver](https://github.com/zenodante/PicoCalc-micropython-driver)

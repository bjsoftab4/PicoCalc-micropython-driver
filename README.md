

# MicroPython Drivers for PicoCalc
Forked from https://github.com/zenodante/PicoCalc-micropython-driver
For more information, see the original web page.

## Difference from orignal
Add RGB565 support for VT100 terminal (Tested on Pico2W)

## Fixed glitchs
Improve stability when uploading file with Thonny.\
To speed up SDcard (SDcard baudrate is set correctly).\
To speed up SDcard (SDcard wait time change 1ms to 20us).\
To prevent buffer overrun with terminal.\
To avoid Thonny hangs.\
To avoid hangs after multiple softreset.\
Fix cursor blink timing after softreset.

## Build Instructions
At first, copy files from pico_files/modules to your micropython build environment, micropython/ports/rp2/modules.

In my system something is going wrong with original instruction.\
I added micropython.cmake in parent directory.  And use make insted of cmake.
```sh
cd micropython/ports/rp2
make USER_C_MODULES="Path/To/PicoCalc-micropython-driver/micropython.cmake" \
  BOARD=[TARGET_BOARD]
```

Supported `TARGET_BOARD` values:
- `RPI_PICO`
- `RPI_PICO2`
- `RPI_PICO2_W`  (tested)

At last, copy file pico_files/root/boot.py to your picocalc root directory.


---
## Features (Modified)

### VT100 Emulator Mode
- Original runs in **16-bit color ** mode.  When Runs in 16bit, change boot.py as followed.
```sh
import framebuf
try:
#    pc_display = PicoDisplay(320, 320)
    pc_display = PicoDisplay(320, 320,framebuf.RGB565)
```

---

## Fixed glitches
### Improve stability at writing FLASH
- To write FLASH, core1 must be stopped with calling multicore_lockout_victim_init().  But the function was commented in core1_main() in picocalcdisplay.c

---

### SDcard baudrate
- Class PicoSD in picocalc.py was not pass baudrate to sdcard.py.  When you want to change baudrate from default, Change boot.py as followed.
```sh
    #pc_sd = PicoSD()
    pc_sd = PicoSD(baudrate=5_000_000)
```
---

### SDcard wait time
- Original sdcard.py wait for response of SDcard.  But its time is 1ms, it causes slow accessing.

---

### Prevent buffer overrun failure with terminal putchar.
- When micropython putchar to bottom line of the terminal, some function(not identified) calls fill_rect_4bpp() with invalid parameter.  So it writes to python variable memory, and micropython hangs.  Check its parameter before writing memory in vtterminal.c .

---

### To avoid Thonny hangs.
- Thonny hangs with error.  PROBLEM IN THONNY'S BACK-END: Internal error (thonny.plugins.micropython.mp_back.ProtocolError: Unexpected read during raw paste).  Disable os.dupterm() when USB connected in boot.py
---
### To avoid hangs after multiple softreset.
- Soft reset with Ctrl+D or Thonny stop/restart button cause hang micropython after repeating 12 or 16 times.  Release DMA channel after soft reset in picodisplay.c
---
### Fix cursor blink timing after softreset.
- After soft reset, cursor blink timing is unstable.  Stop interval timer after soft reset in vtterminal.c
---

## Credits
- [zenodante/PicoCalc-micropython-driver](https://github.com/zenodante/PicoCalc-micropython-driver)

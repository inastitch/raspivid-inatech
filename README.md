This repository contains **Inatech** modifications to ``raspivid`` for camera synchronization over network.

## Build
On Raspberry Pi:
    
    sudo apt install cmake
    
    git clone https://github.com/inastitch/raspivid-inatech.git
    
    mkdir build && cd build/
    cmake ../raspivid-inatech/
    
    # If you type 'make' here, the whole 'userland' code will be compiled
    # That's a lot of libraries and binaries.
    
    # Only build 'raspivid' and its dependencies:
    make raspivid
    
    # 'raspivid' is found at:
    ../raspivid-inatech/build/bin/raspivid
    
    # We rename it to avoid confusion with the original binary
    sudo mv ../raspivid-inatech/build/bin/raspivid /usr/bin/raspivid-inatech

## Run
Notice the new PLL parameters:

    -pll, --pll-on	: Enable software PLL
    -pllv, --pll-v	: Enable PLL debug output
    -plls1, --pll-step1	: PLL framerate correction when drift > 0.1ms
    -plls2, --pll-step2	: PLL framerate correction when drift > 1ms

Note: the higher the framerate, the stronger the correction for the software PLL to lock.

### Examples
To record H264 at ``640x480 @ 30fps``, along with *timing information* (PTS file):

    # With RPi camera v2, this will capture on the full sensor.
    raspivid-inatech                                 \
        -t 0 -w 640 -h 480 -fps 30 -md 4             \
        --pll-on --pll-step1 150 --pll-step2 300     \
        -cd H264 -o video.h264 -pts video.h264.pts

To record MJPEG at `` @ 100fps``:

    # With RPi camera v2, this will capture on a reduced area
    # of the full sensor.
    raspivid-inatech                                 \
        -t 0 -w 640 -h 480 -fps 100 -md 7            \
        --pll-on --pll-step1 500 --pll-step2 1000    \
        -cd MJPEG -o video.mjpeg -pts video.mjpeg.pts

See RPi camera documentation at:
 - https://www.raspberrypi.org/documentation/raspbian/applications/camera.md

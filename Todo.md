运行所有的example
移植midi/audio到keyboard项目


## TinyUsb是如何实现跨平台支持的

## TinyUsb是如何进行测试的持续集成的
0.17.0 Add CodeQL Workflow for Code Security Analysis

## Getting Started
### 编译pico报错
运行
    python3 tools/get_deps.py rp2040
会创建这些目录:
        hw/mcu/raspberry_pi/
        lib/FreeRTOS-Kernel/
        lib/lwip/
        tools/uf2/  
但是实际的代码并未下载，导致报错。为何不用git submodule update --init --recursive来管理这些库？
结果是手工下载这些库。

然后尝试编译报错：
cd examples/device/cdc_msc
make BOARD=raspberry_pi_pico
cmake -S . -B _build/raspberry_pi_pico -DFAMILY=rp2040 -DBOARD=raspberry_pi_pico -DPICO_BUILD_DOCS=0
make: cmake: Permission denied
make: *** [/mnt/c/git/tinyusb/hw/bsp/rp2040/family.mk:11: _build/raspberry_pi_pico] Error 127

结果发现是要手动下载pico-sdk
git clone  https://github.com/raspberrypi/pico-sdk
export PICO_SDK_PATH=/mnt/c/git/tinyusb/pico-sdk/
cmake -S . -B _build/raspberry_pi_pico -DFAMILY=rp2040 -DBOARD=raspberry_pi_pico -DPICO_BUILD_DOCS=0

然后make BOARD=raspberry_pi_pico才成功。



### 编译stm32f401
make BOARD=stm32f401blackpill get-deps照样出错

cd /mnt/c/git/tinyusb/hw/mcu/st
rm -rf ./*
git clone https://github.com/STMicroelectronics/stm32f4xx_hal_driver.git
git clone https://github.com/STMicroelectronics/stm32f4xx_hal_driver.git
make BOARD=stm32f401blackpill all

烧录(注意pyocd不支持stm32f401，改用stm32f412xe)
pyocd flash -t stm32f412xe _build/stm32f401blackpill/cdc_msc.bin
or
make BOARD=stm32f401blackpill PYOCD_TARGET=stm32f412xe flash-pyocd




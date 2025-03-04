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





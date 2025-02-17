# meta-st-stm32mpu-app-logicanalyser
This layer is used to bring the logic analyzer demonstration (see details in https://wiki.st.com/stm32mpu/wiki/How_to_exchange_large_data_buffers_with_the_coprocessor_-_example) on top of STM32MP1 MMDV-3.1.0.

This layer is linked with the https://github.com/STMicroelectronics/logicanalyser project (STM32MP157 Cortex-M4 firmware for the logic analyzer demonstration).

This demonstration is planned to be executed on the STM32MP15 Discovery Kits boards.

The "mx machine" is not used anymore: instead, a patch on the Linux device tree is provided.

## Table of Contents
1. How to add the meta-st-stm32mpu-app-logicanalyser layer to your build?
2. How to run the demonstration?

## 1. How to add the meta-st-stm32mpu-app-logicanalyser layer to your build ?
Get the meta-st-stm32mpu-app-demos-cam layer:
```
PC $> cd <root of the baseline>/layers/meta-st/
PC $> git clone https://github.com/STMicroelectronics/meta-st-stm32mpu-app-logicanalyser.git -b dunfell
```
Configure your machine, your distro, and source your environment:
```
DISTRO=openstlinux-weston MACHINE=stm32mp1 source layers/meta-st/scripts/envsetup.sh
```
Add the meta-st-stm32mpu-app-logicanalyser layer to your environment:
```
PC $> cd <root of the baseline/build-openstlinuxweston-stm32mp1
PC $> bitbake-layers add-layer ../layers/meta-st/meta-st-stm32mpu-app-logicanalyser
```
Then, you can build the baseline and flash the built image as usual.

## 2. How to run the demonstration?
1. Press the "USER1" button to start (resp. stop) the demonstration
2. Select the sampling frequency (4 MHz per default)
3. Start the sampling:
- For high data rate (more than 5 MHz sampling), it relies on a SDB Linux driver which provides DDR buffers allocations, and DDR DMA transfers<br>
- For low data rate (less or equal to 5MHz sampling), it relies on virtual UART over RPMSG<br>
- Data compression algorithm is done on Cortex-M4 side<br>
- Compressed buffers are transfered to DDR by DMA or virtual UART<br>
- In order to insure dynamic input data on PE8..12, these ports are initialized as output. Values are changed every 23 times<br>
- On user interface, refresh is done every new MB of compressed data

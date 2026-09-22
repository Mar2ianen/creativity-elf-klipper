# MCU firmware build

- Klipper source revision: ce7002bedf37e938bb483572949f3703ac6476cb
- MCU: STM32F103VET6, MKS Robin Nano V1.2
- Bootloader offset: 28 KiB (application address 0x08007000)
- Clock: 8 MHz crystal
- Communication: USART3, PB11/PB10
- Klipper serial baud: 250000
- Build output: Robin_nano.bin
- SHA-256: bdc1eb9aac96c93ddd050bcdbea19089e952e709e824a7a2fed7080627c81371

The firmware was built from the upstream Klipper repository at the revision above, then processed with:

    ./scripts/update_mks_robin.py out/klipper.bin out/Robin_nano.bin

The same firmware bytes were copied to the SD card under the filename Robin_nano35.bin for the MKS bootloader. Klipper's board config documents Robin_nano.bin; the MKS filename was used on the card during the successful flash.

# Nucleo-H743ZI + Ethernet + LwIP (without RTOS)
About:
* Working (tested) example of **LwIP** stack usage (without RTOS).
* This example uses static IP address **192.168.1.100** (/24).
* If you want to make a **web server**, see the **httpd** branch.

How to build?
* You can build this projest using **Eclipse** (SW4STM32) IDE
* Or just use the firmware **HEX** file from the **Debug** folder (:

How to test it?
* Power up the **Nucleo-H743ZI** board (connect to USB port or use external 5V/3.3V)
* Upload the firmware to the **STM32H743ZIT6** using **ST-LINK**
* Connect **Nucleo-H743ZI** board to your PC (or router) using **Ethernet** cable
* Setup **IP / network mask** for the PC as **192.168.1.XXX / 255.255.255.0** (XXX = 1-99 or 99-254)
* Open console/terminal window and use commad - **ping 192.168.1.100**

Pay attention to the code lines below. It will help you to understand how to configure **H7** for the correct **Ethernet/LwIP** work.

[STM32H743ZITx_FLASH.ld](/STM32H743ZITx_FLASH.ld):
- [#L35-L39](/STM32H743ZITx_FLASH.ld#L35-L39) : we need more RAM for the stack/heap when using LwIP
- [#L134](/STM32H743ZITx_FLASH.ld#L134), [#L151](/STM32H743ZITx_FLASH.ld#L151), [#L162](/STM32H743ZITx_FLASH.ld#L162) : we need to use a public RAM domain (for example, D1) when using Ethernet
- [#L166-L175](/STM32H743ZITx_FLASH.ld#L166-L175) : we need to store LwIP Rx/Tx buffers in the public RAM domain (for example, D2)

[main.c](/Src/main.c):
- [#L133-L141](/Src/main.c#L133-L141) : HAL_Delay() & MX_LWIP_Process() usage
- [#L237-L262](/Src/main.c#L237-L262) : MPU configuration

[ethernetif.c](/Src/ethernetif.c):
- [#L189](/Src/ethernetif.c#L189), [#L196](/Src/ethernetif.c#L196), [#L203](/Src/ethernetif.c#L203), [#L210](/Src/ethernetif.c#L210) : Ethernet GPIOs speed

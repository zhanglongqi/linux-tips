# LED
You can manage a led connected to a GPIO pin. The LED management is similar with the standard GPIO sysfs driver, but you have some new features like triggers (e.g. "heartbeat" LED blinks like a heart at the rate oh the CPU load) . Here are the GPIO used for the user LED for Beaglebone Black:
```c
pinmux_userled_pins {
			pinctrl-single,pins = <0x98 0x7 0x9c 0x7>;
			linux,phandle = <0x3>;
			phandle = <0x3>;
		};
```
This is the led **pinmux** defination of pinmux@44e10800 section in the "dts" file
the `0x98` and `0x9c` is the offset, the `0x7`,`0x7` is the mode of this gpio pin.

```c
ocp{
    ...

    gpio@481ac000 {
			compatible = "ti,omap4-gpio";
			ti,hwmods = "gpio3";
			gpio-controller;
			#gpio-cells = <0x2>;
			interrupt-controller;
			#interrupt-cells = <0x1>;
			reg = <0x481ac000 0x1000>;
			interrupts = <0x20>;
			linux,phandle = <0x16>;
			phandle = <0x16>;
		};
	}
```
`linux,phandle` and `phandle` is just a 32bit unique number and will be used as `GPIO specifier` in the next section for:

```c
gpio-leds {
			compatible = "gpio-leds";
			pinctrl-names = "default";
			pinctrl-0 = <0x3>;

			led0 {
				label = "beaglebone:green:usr0";
				gpios = <0x16 5 0>;
				linux,default-trigger = "heartbeat";
				default-state = "off";
			};
		};
```

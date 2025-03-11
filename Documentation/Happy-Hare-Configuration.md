#### Page Sections:
-Configuration Options for ERCFv2.5:
  - [Necessary Updates](#---necessary-configuration-updates-for-v25)
  - [Filament Cutter Options](#---filament-cutter-options)
  - [Filament Sensor Options](#---filament-sensor-options)
  - [Pregate Sensor Options](#---pregate-sensor-options)

*\[This guide was adapted from the [Happy Hare Wiki](https://github.com/moggieuk/Happy-Hare/wiki) for ERCF v2.5. Thanks Moggie!\]*


## ![#f03c15](https://github.com/moggieuk/Happy-Hare/wiki/resources/f03c15.png) ![#c5f015](https://github.com/moggieuk/Happy-Hare/wiki/resources/c5f015.png) ![#1589F0](https://github.com/moggieuk/Happy-Hare/wiki/resources/1589F0.png) Necessary Configuration Updates for v2.5

1.  Open the file `config / mmu / base / mmu.cfg`. At the top of the file, beneath `\[mcu mmu\]`, add your Local MCU's CANBus UUID. If you are using USB to connect, Happy Hare should have detected and filled your serial number in for you. If not, you must add it in at this point. Save and close the file.

2. Open the file `config / mmu / base / mmu_hardware.cfg`. If you are using a beefy NEMA17 motor for Direct Drive, you need to increase your motor currents starting on line 128. You want the current to be set to 70% of the motor's rated current, up to the stepper driver's max rated current. The max for for EZ Drivers and most motors is 1.6 amps.

3. Still in the file `config / mmu / base / mmu_hardware.cfg`. If you are using Direct Drive, comment out the Gear Ratio on line 143 by adding a hash, like this:

`#gear_ratio: 80:20			# E.g. ERCF 80:20, Tradrack 50:17`

If you are using legacy Geared Drive, you instead need the Gear Ratio to be set to match your physical gear ratio, usually 80:20 or 60:20. These numbers represent the number of teeth on the 80- or 60-tooth gears, and the 20-tooth pulley. Save and close the file.


## ![#f03c15](https://github.com/moggieuk/Happy-Hare/wiki/resources/f03c15.png) ![#c5f015](https://github.com/moggieuk/Happy-Hare/wiki/resources/c5f015.png) ![#1589F0](https://github.com/moggieuk/Happy-Hare/wiki/resources/1589F0.png) Filament Cutter Options

If you are using a Filament Cutter, you must update `config / mmu / base / mmu_parameters.cfg` line 334. Change the `form_tip_macro` value to `_MMU_CUT_TIP`.

> [IMPORTANT]
> If you are using a fixed cutter depressor on a gantry printer (eg Voron v2.4), there is a chance that your drag chain can catch on the depressor! You will need to create a custom Macro to wrap `_MMU_CUT_TIP` into that safely moves the toolhead into position with the depressor, like so:

```yml
[gcode_macro _MMU_SAFE_CUT_TIP]
gcode:
  G0 X50
  G0 Y305
  _MMU_CUT_TIP
  G0 X50
  G0 Y250
```

**(Do not use this without changing it! Your movement values are guaranteed to differ!)**

Instead of setting the `form_tip_macro` to `_MMU_CUT_TIP` as above, put your "safe" cutting macro with the rest of your non-MMU macros (eg. in `printer.cfg`), then set `form_tip_macro` to the name of your safe cutting macro, in this example, `_MMU_SAFE_CUT_TIP`.

Next, you will need to go through the entire file `config / mmu / base / mmu_macro_vars.cfg`. Read it, understand it, and change all of the options to suit your unique build and needs. The values you use depend on which extruder / toolhead you are using. There are also instructions for semi-automatic calibration for almost all of the cutter and toolhead length values in the Happy Hare Wiki's [Blobbing and Stringing page](https://github.com/moggieuk/Happy-Hare/wiki/Blobbing-and-Stringing)!

Critical variables to change include setting `variable_simple_tip_forming` to `False` and setting the `variable_pin_loc_xy`, but you should really *at least* review the **Movement** and **Cut Tip** sections. These control parking and tip cutting settings and variables.


## ![#f03c15](https://github.com/moggieuk/Happy-Hare/wiki/resources/f03c15.png) ![#c5f015](https://github.com/moggieuk/Happy-Hare/wiki/resources/c5f015.png) ![#1589F0](https://github.com/moggieuk/Happy-Hare/wiki/resources/1589F0.png) Filament Sensor Options

If you are using a toolhead with either an extruder sensor (before the extruder gears), a Toolhead sensor (after the Extruder gears), or both, you need to set those up.

1. Open the file `config / mmu / base / mmu_hardware.cfg`. Add the pin number(s) that your sensor(s) are connected to on lines 278-279. Save and close the file.

2. If you are using an extruder sensor specifically: Open the file `config / mmu / base / mmu_parameters.cfg`. Update the `extruder_homing_endstop` to use the value`extruder`. Save and close the file.

<!--> [!NOTE]
> If you are using a toolhead board like Nitehawk SB or the HartK ERCF toolhead board, all of the config for the toolhead board must be included *before* the MMU config, so that the MMU config can reference the pins needed!--> 

3. If you are using **both** an extruder **and** a toolhead sensor: reopen the file `config / mmu / base / mmu_parameters.cfg`. Update the `extruder_force_homing` to use the value`1`. This will ensure that during loading, the filament slows down before hitting the extruder gears, instead of crashing through them to the toolhead sensor. Save and close the file.

There are instructions for nearly-automatic calibration for almost all of the cutter and toolhead length values in the Happy Hare Wiki's [Blobbing and Stringing page](https://github.com/moggieuk/Happy-Hare/wiki/Blobbing-and-Stringing). Automatic calibration will make your life easier, you should do it.


## ![#f03c15](https://github.com/moggieuk/Happy-Hare/wiki/resources/f03c15.png) ![#c5f015](https://github.com/moggieuk/Happy-Hare/wiki/resources/c5f015.png) ![#1589F0](https://github.com/moggieuk/Happy-Hare/wiki/resources/1589F0.png) Pregate Sensor Options

Coming soon! I don't use these yet, so I need help writing this section!


### ERCF Setup Steps:
- [Flashing Your Local MCU](https://github.com/Enraged-Rabbit-Community/ERCFv2.5/blob/main/Documentation/Flashing-Local-MCU.md)
- [Installing Happy Hare](https://github.com/Enraged-Rabbit-Community/ERCFv2.5/blob/main/Documentation/Installing-Happy-Hare.md)
- Happy Hare Configuration
- [Hardware Configuration Checks](https://github.com/Enraged-Rabbit-Community/ERCFv2.5/blob/main/Documentation/Hardware-configuration-checks.md)
- [Hardware Calibration](https://github.com/Enraged-Rabbit-Community/ERCFv2.5/blob/main/Documentation/Hardware-Calibration.md)
- [Installing KlipperScreen Happy Hare](https://github.com/Enraged-Rabbit-Community/ERCFv2.5/blob/main/Documentation/Installing-KlipperScreen.md)
- [Slicer Setup](https://github.com/Enraged-Rabbit-Community/ERCFv2.5/blob/main/Documentation/Slicer-Setup.md)
- [Further Mods to Consider](https://github.com/Enraged-Rabbit-Community/ERCFv2.5/blob/main/Documentation/Further-Mods.md)

#### Even more Happy Hare info can be found at:
- [Happy Hare Wiki](https://github.com/moggieuk/Happy-Hare/wiki)
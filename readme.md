
# PMU 905
For more docs/info look in the e3-ecmc_plugin_socketcan plugin.

# Install
If you use a kvaser usb can interface you need to install their cansocket driver.
Follow instrunctions in e3-ecmc_plugin_socketcan plugin.

# Get started

## Electrical connections

### CANbus
Connect the pmu905 canbus interface to the kvaser interface
Note: the canbus needs to be supplied with 12V.
Note: The DB15 also includes some I/O (not only CAN). 

### Connect pmu to power
Read pmu905 manual and then connect power to pmu905.


## Software

### Services
Start services:
```
$ sudo modprobe can_dev
$ sudo modprobe can
$ sudo modprobe can_raw
```

### Setup interface

seems kvaser leaf0 is now called can0 (not sure why, probably handled by socketcan)
```
$ sudo ip link set can0 type can bitrate 125000
$ sudo ip link set up can0
$ ip link show can0

# check that interface is there
$ ip addr
```

### monitor canbus with candump
```
$ candump can0
  can0  280   [0] 
  can0  280   [0] 
  can0  280   [0] 
  can0  280   [0] 
  can0  280   [0] 
  can0  703   [1]  05
  can0  280   [0] 
  can0  683   [4]  00 00 00 00
  can0  280   [0] 
  can0  280   [0] 
  can0  183   [8]  00 00 00 00 0B 40 04 20
  can0  280   [0] 
  can0  703   [1]  05
  can0  280   [0] 
  can0  280   [0] 
  can0  280   [0] 
  can0  703   [1]  05
  can0  280   [0] 
  can0  280   [0] 
  can0  280   [0] 
  
  ...
  can0  280   [0] 
  can0  280   [0] 
  can0  703   [1]  05
  can0  280   [0] 
  can0  280   [0] 
  can0  683   [4]  00 0NMT0 00 00 0B 40 04 20
  can0  703   [1]  05
  can0  280   [0] 
  ..
```

# PVs

## Status PVs

### Digital
```
IOC_TEST:CAN03-Stat-SortLine1
IOC_TEST:CAN03-Stat-SortLine2
IOC_TEST:CAN03-Stat-SwUpdate
IOC_TEST:CAN03-Stat-SwHwAlrm
IOC_TEST:CAN03-Stat-FREQ_FAIL
IOC_TEST:CAN03-Stat-REDUCED_PWR
IOC_TEST:CAN03-Stat-PSU_RES
IOC_TEST:CAN03-Stat-FAN_FAIL
IOC_TEST:CAN03-Stat-SUPPLY_FAIL_1
IOC_TEST:CAN03-Stat-SUPPLY_FAIL_2
IOC_TEST:CAN03-Stat-PWR_CALIBRATED
IOC_TEST:CAN03-Stat-DET_INHIB_MON
IOC_TEST:CAN03-Stat-AMPLIFIER_ON
IOC_TEST:CAN03-Stat-RF_IN_FAIL
IOC_TEST:CAN03-Stat-MUTE
IOC_TEST:CAN03-Stat-REFELCTION
IOC_TEST:CAN03-Stat-RF_POWER_FAIL
IOC_TEST:CAN03-Stat-REFLECTION_INT
IOC_TEST:CAN03-Stat-TEMP_FAIL
IOC_TEST:CAN03-Stat-TEMP_FAIL_INT
IOC_TEST:CAN03-Stat-PA_FAIL
IOC_TEST:CAN03-Stat-DRV_FAIL
IOC_TEST:CAN03-Stat-TRANSISTOR_FAIL
IOC_TEST:CAN03-Stat-PEAK_AV
IOC_TEST:CAN03-Stat-RF_INHIB
IOC_TEST:CAN03-Stat-AC_OK_1
IOC_TEST:CAN03-Stat-AC_OK_2
IOC_TEST:CAN03-Stat-FAULT
IOC_TEST:CAN03-Stat-DC_OK_1
IOC_TEST:CAN03-Stat-DC_OK_2
IOC_TEST:CAN03-Stat-DC_FAIL
IOC_TEST:CAN03-Stat-SUPPLY_FAIL
IOC_TEST:CAN03-Stat-REGULATION_FAIL
IOC_TEST:CAN03-Stat-INT_FAIL
IOC_TEST:CAN03-Stat-INT_BUSY
IOC_TEST:CAN03-Stat-BIAS_FAIL_BIT0
IOC_TEST:CAN03-Stat-BIAS_FAIL_BIT1
IOC_TEST:CAN03-Stat-BIAS_ADJ
IOC_TEST:CAN03-Stat-SD_MON
```

### Analog

```
IOC_TEST:CAN03-Stat-PWR_A
IOC_TEST:CAN03-Stat-PWR_B
IOC_TEST:CAN03-Stat-PWR_OUT
IOC_TEST:CAN03-Stat-REFL_OUT
IOC_TEST:CAN03-Stat-V_REG
IOC_TEST:CAN03-Stat-V_TEMP
IOC_TEST:CAN03-Stat-I_DRV
IOC_TEST:CAN03-Stat-I_PRE
IOC_TEST:CAN03-Stat-I_1A
IOC_TEST:CAN03-Stat-I_2A
IOC_TEST:CAN03-Stat-V_REFL_SAVE
IOC_TEST:CAN03-Stat-V_PLUSMON
IOC_TEST:CAN03-Stat-V_I_DC
IOC_TEST:CAN03-Stat-I_1B
IOC_TEST:CAN03-Stat-I_2B
IOC_TEST:CAN03-Stat-V_12V_MON
IOC_TEST:CAN03-Stat-VREF_PWR_OPV
IOC_TEST:CAN03-Stat-V_AUX_IN
IOC_TEST:CAN03-Stat-V_5V_ACB
IOC_TEST:CAN03-Stat-V_3V5
IOC_TEST:CAN03-Stat-AIR_INLET
IOC_TEST:CAN03-Stat-AIR_OUTLET
```
Note: Scalings needs to be confirmed.

## Control PVs

```
IOC_TEST:CAN03-Ctrl-PowerOn
IOC_TEST:CAN03-Ctrl-VrefPwr
IOC_TEST:CAN03-Ctrl-VdcCtrl
IOC_TEST:CAN00-Ctrl-ResetAll
```

### Turn amplifier on

Turn amplifier on:
```
caput IOC_TEST:CAN03-Ctrl-PowerOn 1
```

Output if socketcan plugin is started in DBG_MODE:
```
w 0x700 [1] 0x05
STATE = WRITE_REQ_TRANSFER  basicConfig
STATE = WRITE_WAIT_FOR_CONF basicConfig
w 0x603 [8] 0x21 0x90 0x26 0x01 0x07 0x00 0x00 0x00
STATE = WRITE_DATA basicConfig
r 0x583 [8] 0x60 0x90 0x26 0x01 0x00 0x00 0x00 0x00
w 0x603 [8] 0x01 0x01 0x64 0x00 0x88 0x13 0x00 0x00
STATE = WRITE_IDLE  basicConfig
All data written to slave SDO.
```

### Set VrefPwr

Set VrefPwr to 100:
```
caput IOC_TEST:CAN03-Ctrl-VrefPwr 100
```

Output if socketcan plugin is started in DBG_MODE:
```
STATE = WRITE_REQ_TRANSFER  basicConfig
STATE = WRITE_WAIT_FOR_CONF basicConfig
w 0x603 [8] 0x21 0x90 0x26 0x01 0x07 0x00 0x00 0x00
STATE = WRITE_DATA basicConfig
r 0x583 [8] 0x60 0x90 0x26 0x01 0x00 0x00 0x00 0x00
w 0x603 [8] 0x01 0x00 0x64 0x00 0x00 0x00 0x00 0x00
STATE = WRITE_IDLE  basicConfig
All data written to slave SDO.
```

### Set VdcCtrl

Set VdcCtrl to 5000:
```
caput IOC_TEST:CAN03-Ctrl-VdcCtrl 5000
```

Output if socketcan plugin is started in DBG_MODE:
```
r 0x583 [8] 0x11 0x00 0xCA 0x00 0x00 0x00 0x88 0x13
r 0x703 [1] 0x05
STATE = WRITE_REQ_TRANSFER  basicConfig
STATE = WRITE_WAIT_FOR_CONF basicConfig
w 0x603 [8] 0x21 0x90 0x26 0x01 0x07 0x00 0x00 0x00
STATE = WRITE_DATA basicConfig
r 0x583 [8] 0x60 0x90 0x26 0x01 0x00 0x00 0x00 0x00
w 0x603 [8] 0x01 0x00 0x64 0x00 0x88 0x13 0x00 0x00
STATE = WRITE_IDLE  basicConfig
All data written to slave SDO.
```

### Reset all pmu905 devices

Reset all pmu905 devcies on can network:
```
caput IOC_TEST:CAN00-Ctrl-ResetAll 1
```

Output if socketcan plugin is started in DBG_MODE:
```
w 0x300 [8] 0x02 0x00 0x00 0x00 0x00 0x00 0x00 0x00
```

## Admin PVs (under the hood)

PVs only used for administration (all endinf with "*_").
Should not be accessed directly.

Admin PVs:
```
IOC_TEST:CAN03-StatB0_
IOC_TEST:CAN03-StatB3_
IOC_TEST:CAN03-StatB4_
IOC_TEST:CAN03-StatB5_
IOC_TEST:CAN03-StatB6_
IOC_TEST:CAN03-StatB7_
IOC_TEST:CAN03-Stat-V_TEMP_
IOC_TEST:CAN03-Stat-AIR_INLET_
IOC_TEST:CAN03-Stat-AIR_OUTLET_
IOC_TEST:CAN03-Ctrl-VrefPwrCalcB1_
IOC_TEST:CAN03-Ctrl-VrefPwrCalcB2_
IOC_TEST:CAN03-Ctrl-VdcCtrlCalcB3_
IOC_TEST:CAN03-Ctrl-VdcCtrlCalcB4_
IOC_TEST:CAN00-ResetPackArray_
IOC_TEST:CAN03-BasicConfigPackArray_
IOC_TEST:CAN00-ResetB0_
IOC_TEST:CAN03-BasicConfigB0_
IOC_TEST:CAN00-PDO01-Reset_
IOC_TEST:CAN03-PDO01-Status_
IOC_TEST:CAN03-SDO01-AnalogValues_
IOC_TEST:CAN03-SDO02-BasicConfig_
```

## CANOpen Raw data PVs (interface to ecmc_plugin_socketcan)

The arrays that are the link to the actual data (in ecmc_plugin_socketcan plugin);
Should not be accessed directly.

```
IOC_TEST:CAN00-PDO01-Reset_
IOC_TEST:CAN03-PDO01-Status_
IOC_TEST:CAN03-SDO01-AnalogValues_
IOC_TEST:CAN03-SDO02-BasicConfig_
```

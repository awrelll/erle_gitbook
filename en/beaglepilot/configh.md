# config.h


###APM_Config.h

Before analizing the `Config.h`, it would be useful to comment the [APM_Config.h](https://github.com/diydrones/ardupilot/blob/master/APMrover2/APM_Config.h) file, which also stores configuration matters.

The `APM_Config.h` file is just a placeholder for your configuration file.  If you wish to change any of the setup parameters from their default values, place the appropriate `#define` statements here.

So the `APM_Config.h` funtion is to deal with your parameters and configuration choice.

On the other hand, `Config.h` contains default and automatic configuration details. As you will see there are many warnings alont the file to avoid changes that can lead to desconfiguration.

###Config.h
It is important to do a `Config.h`analysis, because here you can find all the variables initialization value.From these init-values you can extract how they should advance/change.
The [Config.h](https://github.com/BeaglePilot/ardupilot/blob/master/APMrover2/config.h) defines default configuration so, as you can see, it starts with a warning:

```cpp
// -*- tab-width: 4; Mode: C++; c-basic-offset: 4; indent-tabs-mode: nil -*-
//
//////////////////////////////////////////////////////////////////////////////
//////////////////////////////////////////////////////////////////////////////
//
// WARNING WARNING WARNING WARNING WARNING WARNING WARNING WARNING WARNING
//
//  DO NOT EDIT this file to adjust your configuration.  Create your own
//  APM_Config.h and use APM_Config.h.example as a reference.
//
// WARNING WARNING WARNING WARNING WARNING WARNING WARNING WARNING WARNING
///
//////////////////////////////////////////////////////////////////////////////
//////////////////////////////////////////////////////////////////////////////
//
// Default and automatic configuration details.
//
// Notes for maintainers:
//
// - Try to keep this file organised in the same order as APM_Config.h.example
//
...
```
```cpp
#include "defines.h"

///
/// DO NOT EDIT THIS INCLUDE - if you want to make a local change, make that
/// change in your local copy of APM_Config.h.
///
#include "APM_Config.h"  // <== THIS INCLUDE, DO NOT EDIT IT. EVER.
///
/// DO NOT EDIT THIS INCLUDE - if you want to make a local change, make that
/// change in your local copy of APM_Config.h.
///
...
```
The [defines.h](https://github.com/BeaglePilot/ardupilot/blob/master/APMrover2/defines.h) contains internal definitions, that should be included in the configuration.

As commented above, the `APM_config.h ` file includes the local configuration details.

```cpp


#if defined( __AVR_ATmega1280__ )
 // default choices for a 1280. We can't fit everything in, so we
 // make some popular choices by default
 #define LOGGING_ENABLED DISABLED
 #ifndef MOUNT
 # define MOUNT DISABLED
 #endif
 #ifndef CAMERA
 # define CAMERA DISABLED
 #endif
#endif

...
```
This slice of code defines the default status of the Camera and the mount disabled. In order to enable it when really connecting them.
```cpp
// Just so that it's completely clear...
#define ENABLED			1
#define DISABLED		0
...
```
I would remark this slice of code. Enabled is defined with the binary value 1 and Disabled with the 0. It is important to note that 1 and 0 refer the **binary values** of these numbers. In electronics, generally, the binary value 1 refers something working (like a closed switch) and the value 0 the oposite (like a open switch).
This concep will appear frecuently with dealing with electronic devices.

```cpp
//////////////////////////////////////////////////////////////////////////////
// sensor types

#define CONFIG_INS_TYPE HAL_INS_DEFAULT
#define CONFIG_BARO     HAL_BARO_DEFAULT
#define CONFIG_COMPASS  HAL_COMPASS_DEFAULT

#ifdef HAL_SERIAL0_BAUD_DEFAULT
# define SERIAL0_BAUD HAL_SERIAL0_BAUD_DEFAULT
#endif
...
```

Here four types of sensors are configures: barometer, compass data sensor, serial sensor and ins type sensor.
```cpp
//////////////////////////////////////////////////////////////////////////////
// HIL_MODE                                 OPTIONAL

#ifndef HIL_MODE
 #define HIL_MODE        HIL_MODE_DISABLED
#endif

#if HIL_MODE != HIL_MODE_DISABLED       // we are in HIL mode
 #undef CONFIG_BARO
 #define CONFIG_BARO HAL_BARO_HIL
 #undef CONFIG_INS_TYPE
 #define CONFIG_INS_TYPE HAL_INS_HIL
 #undef  CONFIG_COMPASS
 #define CONFIG_COMPASS HAL_COMPASS_HIL
#endif

#if CONFIG_HAL_BOARD == HAL_BOARD_APM1
# define BATTERY_PIN_1	  0
# define CURRENT_PIN_1	  1
#elif CONFIG_HAL_BOARD == HAL_BOARD_APM2
# define BATTERY_PIN_1	  1
# define CURRENT_PIN_1	  2
#elif CONFIG_HAL_BOARD == HAL_BOARD_AVR_SITL
# define BATTERY_PIN_1	  1
# define CURRENT_PIN_1	  2
#elif CONFIG_HAL_BOARD == HAL_BOARD_PX4
# define BATTERY_PIN_1	  -1
# define CURRENT_PIN_1	  -1
#elif CONFIG_HAL_BOARD == HAL_BOARD_FLYMAPLE
# define BATTERY_PIN_1     20
# define CURRENT_PIN_1	   19
#elif CONFIG_HAL_BOARD == HAL_BOARD_LINUX
# define BATTERY_PIN_1     -1
# define CURRENT_PIN_1	   -1
#elif CONFIG_HAL_BOARD == HAL_BOARD_VRBRAIN
# define BATTERY_PIN_1	  -1
# define CURRENT_PIN_1	  -1
#endif
...
```
This slice of code stablish the parameters of the HIL mode (Hardware in the loop), for
development and testing of embedded real-time complex systems.First check if the HIL mode is enabled and after that defines and initialize the corresponding variables.

For example deactivate the `CONFIG_BARO` and enables the `CONFIG_BARO HAL_BARO_HIL`, which is used in the HIL mode.

Also defines the battery and input pins, depending on the type of board.

```cpp

//////////////////////////////////////////////////////////////////////////////
// HIL_MODE                                 OPTIONAL

#ifndef HIL_MODE
#define HIL_MODE	HIL_MODE_DISABLED
#endif

#ifndef MAV_SYSTEM_ID
# define MAV_SYSTEM_ID		1
#endif
...
```

This configuration opf the HIL mode is optional and stablish the `MAV_SYSTEM_ID`.
```cpp

//////////////////////////////////////////////////////////////////////////////
// Serial port speeds.
//
#ifndef SERIAL0_BAUD
# define SERIAL0_BAUD			115200
#endif
#ifndef SERIAL1_BAUD
# define SERIAL1_BAUD			 57600
#endif
#ifndef SERIAL2_BAUD
# define SERIAL2_BAUD			 57600
#endif

#ifndef CH7_OPTION
# define CH7_OPTION		          CH7_SAVE_WP
#endif

#ifndef TUNING_OPTION
# define TUNING_OPTION		          TUN_NONE
#endif

...
```
Here the serial ports speed is stablished through the number of bauds.Baud is the unit of symbol rate, also known as baud or modulation rate; the number of distinct symbol changes (signaling events) made to the transmission medium per second in a digitally modulated signal or a line code.
```cpp
//////////////////////////////////////////////////////////////////////////////
// INPUT_VOLTAGE
//
#ifndef INPUT_VOLTAGE
# define INPUT_VOLTAGE			4.68	//  4.68 is the average value for a sample set.  This is the value at the processor with 5.02 applied at the servo rail
#endif
...
```
This slice of code defines the `INPUT_VOLTAGE`for making it work.

```cpp
//////////////////////////////////////////////////////////////////////////////
//  MAGNETOMETER
#ifndef MAGNETOMETER
# define MAGNETOMETER			ENABLED
#endif

...
```
`MAGNETOMETER`is enabled here fot its later use in other code files.
```cpp

//////////////////////////////////////////////////////////////////////////////
// MODE
// MODE_CHANNEL
//
#ifndef MODE_CHANNEL
# define MODE_CHANNEL	8
#endif
#if (MODE_CHANNEL != 5) && (MODE_CHANNEL != 6) && (MODE_CHANNEL != 7) && (MODE_CHANNEL != 8)
# error XXX
# error XXX You must set MODE_CHANNEL to 5, 6, 7 or 8
# error XXX
#endif

#if !defined(MODE_1)
# define MODE_1			LEARNING
#endif
#if !defined(MODE_2)
# define MODE_2			LEARNING
#endif
#if !defined(MODE_3)
# define MODE_3			LEARNING
#endif
#if !defined(MODE_4)
# define MODE_4			LEARNING
#endif
#if !defined(MODE_5)
# define MODE_5			LEARNING
#endif
#if !defined(MODE_6)
# define MODE_6			MANUAL
#endif
...
```
This slice of code defines six modes for channel input/output.The first 5th are `LEARNING`modes, whilehe 6th one is `MANUAL`.

```cpp
//////////////////////////////////////////////////////////////////////////////
// failsafe defaults
#ifndef THROTTLE_FAILSAFE
# define THROTTLE_FAILSAFE		ENABLED
#endif
#ifndef THROTTLE_FS_VALUE
# define THROTTLE_FS_VALUE		950
#endif
#ifndef LONG_FAILSAFE_ACTION
# define LONG_FAILSAFE_ACTION		0
#endif
#ifndef GCS_HEARTBEAT_FAILSAFE
# define GCS_HEARTBEAT_FAILSAFE		DISABLED
#endif


///////////////////
...
```
The failsafe default events configuration is made here.For example defines and enables the `THROTTLE_FAILSAFE` in case the there is any problem with the throttle.
```cpp
//////////////////////////////////////////////////////////////////////////////
// THROTTLE_OUT
//
#ifndef THROTTLE_OUT
# define THROTTLE_OUT			ENABLED
#endif
...
```
Here the `THROTTLE_OUT`is defined and enabled.
```cpp


//////////////////////////////////////////////////////////////////////////////
//////////////////////////////////////////////////////////////////////////////
// STARTUP BEHAVIOUR
//////////////////////////////////////////////////////////////////////////////
//////////////////////////////////////////////////////////////////////////////

```
Now  comes the `STARTUP BEHAVIOUR` configuration.

```cpp

//////////////////////////////////////////////////////////////////////////////
// GROUND_START_DELAY
//
#ifndef GROUND_START_DELAY
# define GROUND_START_DELAY		0
#endif
...
```
This slice of code defines and initialize the delay in 0 ms.

```cpp
//////////////////////////////////////////////////////////////////////////////
// MOUNT (ANTENNA OR CAMERA)
//
#ifndef MOUNT
# define MOUNT		ENABLED
#endif

//////////////////////////////////////////////////////////////////////////////
// CAMERA control
//
#ifndef CAMERA
# define CAMERA ENABLED
#endif

...
```
Here the `MOUNT`and the `CAMERA` are enabled, in case they are defined (it means they exist/are connected).

```cpp

/////////////////////////////////////////////////////////////////////////////
// AIRSPEED_CRUISE
//
#ifndef SPEED_CRUISE
# define SPEED_CRUISE		3 // 3 m/s
#endif

#ifndef GSBOOST
# define GSBOOST		0
#endif
#ifndef GSBOOST
# define GSBOOST		0
#endif
#ifndef NUDGE_OFFSET
# define NUDGE_OFFSET		0
#endif

#ifndef E_GLIDER
# define E_GLIDER		ENABLED
#endif

#ifndef TURN_GAIN
# define TURN_GAIN		5
#endif
...
```
This slice of code stablish the value of the variables related to the measure of the air-speed.

```cpp
//////////////////////////////////////////////////////////////////////////////
// Servo Mapping
//
#ifndef THROTTLE_MIN
# define THROTTLE_MIN			0 // percent
#endif
#ifndef THROTTLE_CRUISE
# define THROTTLE_CRUISE		45
#endif
#ifndef THROTTLE_MAX
# define THROTTLE_MAX			100
#endif
...
```
This slice of code defines the min, max and cruise values for the throttle.

```cpp

//////////////////////////////////////////////////////////////////////////////
// Attitude control gains
//
#ifndef SERVO_STEER_P
# define SERVO_STEER_P         0.4
#endif
#ifndef SERVO_STEER_I
# define SERVO_STEER_I         0.0
#endif
#ifndef SERVO_STEER_D
# define SERVO_STEER_D         0.0
#endif
#ifndef SERVO_STEER_INT_MAX
# define SERVO_STEER_INT_MAX   5
#endif
#define SERVO_STEER_INT_MAX_CENTIDEGREE SERVO_STEER_INT_MAX*100

...
```
Here the gains when controlling the attitude are stablished.

```cpp

//////////////////////////////////////////////////////////////////////////////
// Crosstrack compensation
//
#ifndef XTRACK_GAIN
# define XTRACK_GAIN          1 // deg/m
#endif
#ifndef XTRACK_ENTRY_ANGLE
# define XTRACK_ENTRY_ANGLE   50 // deg
#endif
# define XTRACK_GAIN_SCALED XTRACK_GAIN*100
# define XTRACK_ENTRY_ANGLE_CENTIDEGREE XTRACK_ENTRY_ANGLE*100
...
```
This slice of code stablish the angular gain and the entry angle for cosstrack compensation.

```cpp

//////////////////////////////////////////////////////////////////////////////
// Dataflash logging control
//
#ifndef LOGGING_ENABLED
# define LOGGING_ENABLED		ENABLED
#endif

#if CONFIG_HAL_BOARD == HAL_BOARD_APM1 || CONFIG_HAL_BOARD == HAL_BOARD_APM2
#define DEFAULT_LOG_BITMASK     \
    MASK_LOG_ATTITUDE_MED | \
    MASK_LOG_GPS | \
    MASK_LOG_PM | \
    MASK_LOG_CTUN | \
    MASK_LOG_NTUN | \
    MASK_LOG_MODE | \
    MASK_LOG_CMD | \
    MASK_LOG_SONAR | \
    MASK_LOG_COMPASS | \
    MASK_LOG_CURRENT | \
    MASK_LOG_STEERING | \
    MASK_LOG_CAMERA
#else
// other systems have plenty of space for full logs
#define DEFAULT_LOG_BITMASK   0xffff
#endif
...
```

This slice of code enables the logging to the Dataflash and if the board is one of the specified, then defines default logs.
```cpp
//////////////////////////////////////////////////////////////////////////////
// Developer Items
//

#ifndef STANDARD_SPEED
# define STANDARD_SPEED		3.0
#define STANDARD_SPEED_SQUARED (STANDARD_SPEED * STANDARD_SPEED)
#endif
#define STANDARD_THROTTLE_SQUARED (THROTTLE_CRUISE * THROTTLE_CRUISE)

// use this to enable servos in HIL mode
#ifndef HIL_SERVOS
# define HIL_SERVOS DISABLED
#endif

// use this to completely disable the CLI
#ifndef CLI_ENABLED
# define CLI_ENABLED ENABLED
#endif

// if RESET_SWITCH_CH is not zero, then this is the PWM value on
// that channel where we reset the control mode to the current switch
// position (to for example return to switched mode after failsafe or
// fence breach)
#ifndef RESET_SWITCH_CHAN_PWM
# define RESET_SWITCH_CHAN_PWM 1750
#endif

#ifndef BOOSTER
# define BOOSTER              2    // booster factor x1 = 1 or x2 = 2
#endif

#ifndef SONAR_ENABLED
# define SONAR_ENABLED       DISABLED
#endif

/*
  build a firmware version string.
  GIT_VERSION comes from Makefile builds
*/
#ifndef GIT_VERSION
#define FIRMWARE_STRING THISFIRMWARE
#else
#define FIRMWARE_STRING THISFIRMWARE " (" GIT_VERSION ")"
#endif
```


This slice of code, first defines the standard speed values, then the HIL MODE servos,then enables the command line (CMI), enables the sonar and the filmeware.Also, implements a `RESET_SWITCH` function for reseting the control mode to the current switch position.


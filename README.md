# Q-SYS Reflect Connection Monitoring

A basic Q-SYS component to monitor for loss of connection with Reflect and disable network connection to prevent Core lockup.

Created to address rare cases of a Core locking up when connection to reflect.qsc.com is lost. If the server is able to be pinged but communication is not successful, the core will enter a cycle of losing connection and restoring connection to the server. In most cases, this is slow enough to not be a problem. But, in some cases, this cycle has been rapid enough to cause the core to lock up. Testing has shown that if the core cannot ping the server, it will not enter this cycle. This component will monitor for loss of communication with reflect.qsc.com. If connection drops exceed parameters set by the programmer, the output LED of the component will be set low for a given period before returning high. This output would typically be wired to the "Enable/Disable Port" pin of the network port the core uses to reach the internet, but may be used in other ways.

## Caveats

This was designed for cores that use different LANs for control of the A/V system and connection with the internet. For cores that use only one LAN connection, the output pin of the component could be wired as needed.

## Output

The "Reflect Connection Monitoring" container has one LED output pin. This pin will be high when internet connection should be enabled and low when it should be disabled.

## Monitoring Method

The component monitors the core's Event Log looking for "Connection to Reflect has been lost" and "Connection to Reflect has been restored" messages. If the configured maximum number of drops is reached within the set window, the output pin will be set low. After the set delay time, the output pin will be set high again.

## Controls

All controls are contained within the Reflect Drop Monitoring Text Controller, within the Reflect Connection Monitoring container.

| Name            | Description                                                                                   |
| --------------- | --------------------------------------------------------------------------------------------- |
| time_LossWindow | Time in which the max number of drops needs to be reached for the port to be disabled.        |
| time_RetryDelay | How long the port will remain disabled for.                                                   |
| int_MaxDrops    | Maximum number of connection drops needed within the loss window for the port to be disabled. |

## Netgear AV Line Setup

This was developed for use with the Netgear AV Line Switch plugin to control the network switch, but the output pin may be connected as desired for other control methods.

* The user credentials that the AV Line Switch plugin uses to connect to the switch must be configured to allow read/write access in the configuration on the switch. Make sure this is allowed by security policies and access to these credentials is restricted.
* In the AV Line Switch Plugin properties, "Write Access" must be set to "Yes", and "Port Information" must be set to "Extended"
* Expose the "Enable/Disable Port" pin for the port the core uses to connect to the internet.
* Wire the output pin of this component to the desired "Enable/Disable Port" pin.

## Testing

Within the Reflect Connection Monitoring container is a "Drop Test" text controller. This contains a "btn_ConnectionState" toggle button. This simulates a loss of communication by simulating "Connection to Reflect has been Lost/Restored" events when the button changes state. This allows for testing of programming when emulating, offline, or when there is no problem with communication with Reflect.

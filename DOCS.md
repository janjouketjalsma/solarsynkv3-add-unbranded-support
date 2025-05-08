![](https://github.com/martinville/solarsynkv3/blob/main/doc/logo.png?raw=true)

## How It Works
SolarSynk retrieves solar system data from the cloud, where it's uploaded via your Sunsynk dongle. It has no direct physical connection to your inverter. 

*Please note that this add-on only provides sensor data, it does not include pre-built home assistant  cards to display the information.

## Getting Started
This add-on publishes sensor values to Home Assistant entities using the local API, requiring a long-lived access token.

### Creating a Long-Lived Access Token
Click your profile picture in the bottom left of the HA interface. Scroll down to the bottom and generate a long-lived token. The name isn't important for SolarSynk, but the token key is—be sure to copy and save it for later use.

![](https://github.com/martinville/solarsynkv3/blob/main/doc/longlivetoken.png?raw=true)

### Add this respository to your Home Assistant add-on store
From the "Settings" menu item in Home Asstant's UI go to "Add-ons". In the bottom right-hand corner click "ADD-ON STORE". The in the right-hand top corner click the three dots and select "Repositories".
Paste the following repository link and click add then close https://github.com/martinville/solarsynkv3

![](https://github.com/martinville/solarsynkv3/blob/main/doc/addrepo.png?raw=true)

Refresh the browser. Right at the bottom you should now see the "SolarSynk" add-on. Click it to open the into page, then click the "Install" button.

![](https://github.com/martinville/solarsynkv3/blob/main/doc/addonavail.png?raw=true)

### Provide your Sunsynk.net credentials
After installing this add-on make sure you enter all the required information on the configuration page. Note if your intentions are to update a Home Assistant installtion with a different IP than the one where this addon is installed, you need to generate the long live token on the Home Assistant instance where entities will be updated. 
*NOTE: In some cases special characters in the password field may cause authentication issues. Also SolarSynk will not be able to connect to your sunsynk profile with MFA enabled.

*DO NOT USE localhost or 127.0.0.1 in the IP field, either use the actual IP or hostname. If you implemented a SSL certificate , its mandatory to use the hostname assigned to the certificate and not the IP. 
(This addon runs in a docker and cannot reach your home assistant network on the local loopback name or ip)

![](https://github.com/martinville/solarsynkv3/blob/main/doc/settings.png)

In case you are unsure what your Sunsynk inverter's serial number is. Log into the synsynk.net portal and copy the serial number from the "Inverter" menu item.
For multiple inverters seperate the inverter serial numbers with a semi colon ; Example 123456;7890123

![](https://github.com/martinville/solarsynkv3/blob/doc/main/sunserial.png)

Make sure you also populate the "HA_LongLiveToken" field with the long-lived token that you created earlier on.

### Start the script
After entering all of the required information you can go ahead and start the service script.

![](https://github.com/martinville/solarsynkv3/blob/main/doc/scriptstarted.png?raw=true)

After starting the add-on, verify everything is working by clicking the "Log" tab. Scroll through the log to ensure sensor data is populated correctly. The log will display success or error messages based on the status of the integration.



# Sending Settings back to inverter

### Intro
The Solarsynk Home Assistant integration enables you to send settings to your inverter, with support currently limited to updating battery settings.

### How it works
After each loop, when the integration retrieves the various values, it will check the entity named [solarsynkv3_YOUR_INVERTER_SERIAL_NUMBER_settings]. If valid settings are found, they will be posted back to the SunSynk Portal.

Once the settings are posted, the settings entity will be cleared to prevent the integration from repeatedly uploading the same settings in a continuous loop.

When running the integration for the first time, you will see an error message indicating that the entity does not exist, along with instructions on how to create it. However, the integration will also attempt to automatically create the entity. If the automatic creation fails, you can follow the instructions in the log to manually create it.

#### Below is a list of all settings that can be updated
```json
{
    "absorptionVolt": "57.6",
    "battMode": "-1",
    "batteryCap": "100",
    "batteryEfficiency": "99",
    "batteryEmptyV": "45",
    "batteryImpedance": "8",
    "batteryLowCap": "35",
    "batteryLowVolt": "47.5",
    "batteryMaxCurrentCharge": "40",
    "batteryMaxCurrentDischarge": "70",
    "batteryOn": "1",
    "batteryRestartCap": "50",
    "batteryRestartVolt": "52",
    "batteryShutdownCap": "20",
    "batteryShutdownVolt": "46",
    "bmsErrStop": "0",
    "disableFloatCharge": "0",
    "equChargeCycle": "90",
    "equChargeTime": "0",
    "equVoltCharge": "57.6",
    "floatVolt": "55.2",
    "genChargeOn": "0",
    "genSignal": "1",
    "generatorBatteryCurrent": "40",
    "generatorForcedStart": "0",
    "generatorStartCap": "30",
    "generatorStartVolt": "49",
    "gridSignal": "1",
    "lithiumMode": "0",
    "lowNoiseMode": "8000",
    "lowPowerMode": "0",
    "safetyType": "0",
    "sdBatteryCurrent": "40",
    "sdChargeOn": "1",
    "sdStartCap": "30",
    "sdStartVol": "undefined",
    "sdStartVolt": "49",
    "signalIslandModeEnable": "0",
    "sn": "123456",
    "tempco": "0"
}
```

## Setting the settings entity value with a properly formatted string
Each setting must be separated by a semicolon (;).

### Example of a Single Setting:
`"batteryCap": "100"`

### Example of Updating Multiple Settings Simultaneously:
`"batteryCap": "100";"batteryLowCap": "35"`




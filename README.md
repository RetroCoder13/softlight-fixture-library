# softlight-fixture-library
Fixture library for Softlight

- [Creating a new Fixture Profile](#creating-a-new-fixture-profile)
- [Structure](#structure)
- [Channels](#channels)
  - [Default Value](#default-value)
  - [Channel Types](#channel-types)
  - [Channel Names](#channel-names)
  - [Coarse and Fine Channels](#coarse-and-fine-channels)
  - [Pixel Zones](#pixel-zones)
- [Modes](#modes)
- [Examples](#examples)

## Creating a new Fixture Profile
When creating a new fixture profile, please save it to this file location:

```<manufacturer>/<fixture>.json```

If you have any queries then look at other fixture profiles to help.

## Structure
The base JSON data should follow this structure:

```json
{
	"name": "<name>",
	"manufacturer": "<manufacturer>",
	"productPage": "<product page url>",
	"pixels": <pixel zones>,
	"channels": {
        <channels>
	},
	"modes": {
        <modes>
	}
}
```

## Channels
The channels section should follow this structure:

```json
"channels": {
    "Dimmer": {
		"defaultValue": 0,
		"type": "dimmer"
	},
	"Red": {
		"defaultValue": 255,
		"type": "color"
	},
	"Green": {
		"defaultValue": 255,
		"type": "color"
	},
	"Blue": {
		"defaultValue": 255,
		"type": "color"
	},
    ...
}
```

### Default Value
The default value is what Softlight sets the channels as when a new fixture is created or Softlight is loaded. For example most fixtures have a colour already set but the dimmer is off. To achieve this set the colours default value to 255 but the leave the dimmer as 0.

### Channel Types
There are multiple channel types for fixtures. These correspond to the fixture parameter buttons in Softlight.

They can be either ```dimmer``` ```color``` ```position``` ```beam``` ```shape```

### Channel Names
The channel names should follow the same standard as the fixture documentation.

The only exception is if the documentation says ```Shutter``` or ```Strobe```. Either of these should always say ```Shutter Strobe``` in the fixture profile.

### Coarse and Fine Channels
If a fixture has a coarse and fine channel then the standard would be to have:
```Dimmer``` and ```Fine Dimmer``` as the channel names. Softlight will then detect this and merge them into one 16-bit controllable parameter.

The default value will still need to be split over both channels. For example a fixture that uses 16-bits for ```Pan``` and ```Tilt``` may want the default value to be halfway. To achieve this set the coarse channel to 128 and leave the fine as 0:
```json
"channels": {
	"Pan": {
		"defaultValue": 128,
		"type": "position"
	},
	"Fine Pan": {
		"defaultValue": 0,
		"type": "position"
	},
	"Tilt": {
		"defaultValue": 128,
		"type": "position"
	},
	"Fine Tilt": {
		"defaultValue": 0,
		"type": "position"
	},
}
```

### Pixel zones
If a fixture has multiple pixel zones then ensure the ```pixel``` attribute at the top of the fixture profile is set accordingly. The channel names should then follow this standard: ```<attribute> <zone>```

For example: ```Red 1``` or ```Fine Dimmer 2```

## Modes
Channel modes define what parameters are used in that mode. This can be found in the fixture documentation. The standard is:
```json
"modes": {
    "4 Channel": [
		"Dimmer",
        "Red",
        "Green",
        "Blue"
	],
    "1 Channel": [
        "Dimmer"
    ],
    ...
}
```

Any channel names must match exactly to what is written under the channels section. 

Each line corresponds to that channel for the fixture. For example looking at the above ```4 Channel``` mode, the ```Dimmer``` attribute is the 1st channel and the ```Blue``` attribute is the 4th channel.

## Examples
Here are some exemplar fixture profiles which are accurate to documentation and work correctly within Softlight:
- [Rogue R2X Wash](/Chauvet%20Professional/Rogue%20R2X%20Wash.json)
- [Ovation E-910FC](/Chauvet%20Professional/Ovation%20E-910FC.json)
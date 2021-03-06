## Overview
* [Prerequisities](#prerequisities)
* [Introduction](#introduction)
* [Basic Config](#basic-config)
  * [Required Parameters](#required-parameters)
  * [Example: Basic Alarm Configuration using Required Parameters](#example-basic-alarm-configuration-using-required-parameters)
* [Advanced Config](#advanced-config)
  * [Optional Parameters](#optional-parameters)
  * [Example: Alarm Configuration Using Optional Parameters](#example-alarm-configuration-using-optional-parameters)
  * [Mini Map Configuration](#mini-map-configuration)
* [How to Get a Slack API Key](#how-to-get-a-slack-api-key)

## Prerequisites
This guide assumes 

1. You are familiar with [JSON formatting](http://www.w3schools.com/json/default.asp)
2. You have read and understood the [Alarm Configuration](https://github.com/kvangent/PokeAlarm/wiki/Alarm-Configuration) Wiki
3. You are comfortable with the layout of `alarms.json`.

Please familiarize yourself with all of the above before proceeding.

## Introduction

![](images/slack_demo.PNG)

**Slack** is a cloud-based team collaboration tool that is available on Windows, Mac OS X, Linux, iOS, Android, and Windows Phone. Slack offers a lot of IRC-like features: persistent chat rooms (channels) organized by topic, as well as private groups and direct messaging. All content inside Slack is searchable, including files, conversations, and people.

PokeAlarm offers the following for Slack:

* Custom username for posting
* High resolution icons for pokemon, gym, or pokestop notifications
* Notifications to multiple Slack channels and/or teams
* Customizable Google Map image of the pokemon, gym, and/or pokestop location
* Personalized notifications via [Dynamic Text Substitution](Dynamic-Text-Subsitution)



## Basic Config

### Required Parameters
These `alarms.json` parameters - `active`, `type`, and `api_key` - are required to enable the Slack alarm service:

| Parameters     | Description                            |
| -------------- |----------------------------------------|
| type           | must be `slack`                        |
| active         | `True` for alarm to be active          |
| api_key        | Your API key                           |

### Example: Basic Alarm Configuration using Required Parameters
```json
{
	"active": "True",
	"type":"slack",
	"api_key":"YOUR_API_KEY"
}
```
**Note:** The above code is to be inserted into the alarms section of alarms.json. It does not represent the entire alarms.json file.

## Advanced Config

### Optional Parameters
In addition to the 3 required parameters, several optional parameters are available to personalize your Slack notifications.  Below is an example of these optional parameters and how they are incorporated into a functional alarm layout for Slack.

These optional parameters, `startup_message`, and `startup_list`, are entered at the same level as `"type":"slack"`.

| Parameters         | Description                                                | Default                      |
|--------------------|------------------------------------------------------------|------------------------------|
| `startup_message`  | confirmation post when PokeAlarm initialized               | `True`                       |
| `startup_list`     | First post will list all alarmed pokemon enabled in `alarms.json`    | `True`            |

These optional parameters below are applicable to the `pokemon`, `pokestop`, and `gym` sections of the JSON file.

| Parameters       | Description                                       | Default                                       |
| -----------------|---------------------------------------------------|-----------------------------------------------|
| `channel`        | Send messages to this channel.. `#<pkmn>` pushes to pokemon name* | `#general`                    |
| `username`       | Username the bot should post the message as       | `<pkmn>`                                      | 
| `icon_url`       | URL path to pokemon icon	   					   |												 |
| `title`          | Notification text to begin the message            | `A wild <pkmn> has appeared!`                 |
| `url`            | Link to be added to notification text             | `<gmaps>`                                     |
| `body`           | Additional text to be added to the message        | `Available until <24h_time> (<time_left>).`   | 
| `map`            | Specify a json object to describe the map         | See Mini Map Configuration for more details   |
*Note: Nidorans will be `nidoranf` or `nidoranm`, Farfetch'd will be `farfetchd`, and Mr. Mime will be `mrmime`. Channels that do not exist (channels cannot be created by bots) will default to general instead.

## Example: Alarm Configuration Using Optional Parameters
```json
{
	"active": "True",
	"type":"slack",
	"api_key":"YOUR_API_KEY",
	"channel":"general",
	"startup_message":"True",
	"startup_list":"True",
	"pokemon":{
		"channel":"general",
		"username":"<pkmn>",
		"icon_url" : "https://raw.githubusercontent.com/kvangent/PokeAlarm/master/icons/<id>.png",
		"title":"A wild <pkmn> has appeared!",
		"url":"<gmaps>",
		"body": "Available until <24h_time> (<time_left>).",
		"map": { 
			"enabled":"true",
			"width":"250",
			"height":"125",
			"maptype":"roadmap",
			"zoom": "15"
		}
	},
	"pokestop":{
		"channel":"general",
		"username":"Pokestop",
		"icon_url" : "https://raw.githubusercontent.com/kvangent/PokeAlarm/master/icons/pokestop.png",
		"title":"Someone has placed a lure on a Pokestop!",
		"url":"<gmaps>",
		"body":"Lure will expire at <24h_time> (<time_left>)."
	},
	"gym":{
		"channel":"general",
		"username":"Pokemon Gym",
		"icon_url" : "https://raw.githubusercontent.com/kvangent/PokeAlarm/master/icons/gym.png",
		"title":"A Team <old_team> gym has fallen!",
		"url":"<gmaps>",
		"body": "It is now controlled by <new_team>."
	}
}
```
**Note:** The above code is to be inserted into the alarms section of alarms.json. It does not represent the entire alarms.json file.


### Mini Map Configuration
![](images/minimap.png)

You can enable a small Google Static Maps image after your Slack post, showing the location of the alarmed pokemon, gym, or lure.  This is done by adding the `map` parameter to your `pokemon`, `gym`, or `lure` sections of your Slack alarm. 



Below is an example of enabling the mini map for pokemon.
```json
	"pokemon":{
		"channel":"general",
		"username":"<pkmn>",
		"icon_url" : "https://raw.githubusercontent.com/kvangent/PokeAlarm/master/icons/<id>.png",
		"title":"A wild <pkmn> has appeared!",
		"url":"<gmaps>",
		"body": "Available until <24h_time> (<time_left>).",
		"map": {             
			"enabled":"true", 
			"width":"250",    
			"height":"125",  
			"maptype":"roadmap",
			"zoom": "15"      
		}                      
	},
```

| Parameters     | Description                                       | Default                                       |
| -------------- |---------------------------------------------------|-----------------------------------------------|
| enabled        | Turns the map on or off                           | `True`                                        |
| width          | Width of the map                                  | `250` px                                      |
| height         | Height of the map                                 | `150` px                                      | 
| maptype        | Link to be added to notification text             | `roadmap`                                     |
| zoom           | Specifies the zoom of the map                     | `15`                                          | 

 
## How to get a Slack API Key

1. Visit [slack.com](https://www.slack.com). Enter your email address and click 'Create your team'. Follow the instructions to setup and activate your account. 

2. Go to the [create a bot page](https://my.slack.com/services/new/bot). Enter a username and click create.

3. Copy the API Token given. Fill out any more information you want, and click 'Save Integration'.
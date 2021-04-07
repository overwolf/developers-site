---
id: overwolf-settings
title: overwolf.settings API
sidebar_label: overwolf.settings
---

Use this API to view and modify one of the following Overwolf settings properties:

* Hotkeys – register a function for a given hotkey, or retrieve an existing hotkey key combination.
* Retrieve the current Overwolf user language.
* Video (i.e., folder location, capture settings, FPS settings, etc.).

## Special OW URL's

You can also use the following helpful URLs to open the Overwolf settings and hotkey windows:

* `overwolf://settings`  
  A clickable link that opens the Overwolf settings window from your app.
  
* `overwolf://settings/subscriptions`  
  A clickable link that opens the Overwolf Subscriptions settings window from your app.

* `overwolf://settings/games-overlay`  
   A clickable link that opens the Overwolf Overlay and Hotkeys settings window from your app.

* `overwolf://settings/games-overlay?hotkey=hotkey_name_in_manifest`  
  A clickable link that opens the Overwolf Overlay and Hotkeys settings window from your app, and then focuses on the stated hotkey.
  This should be the same hotkey name as written in the manifest.json.  
  
  Note that this means you can’t focus on Overwolf’s built-in hotkeys or hotkeys of other apps.

* You can even link into the hotkeys of a specific game settings:  
  `overwolf://settings/games-overlay?hotkey=hotkey_name_in_manifest&gameId=game_id`

Read more about how to use the [overwolf.settings.hotkeys API](overwolf-settings-hotkeys) in our [Hotkeys best practices guide](../topics/hotkeys-best-practices).

## Methods Reference

* [overwolf.settings.getHotKey()](#gethotkeyfeatureid-callback)
* [overwolf.settings.registerHotKey()](#registerhotkeyactionid-callback)
* [overwolf.settings.getCurrentOverwolfLanguage()](#getcurrentoverwolflanguagecallback)
* [overwolf.settings.getOverwolfVideosFolder()](#getoverwolfvideosfoldercallback)
* [overwolf.settings.setOverwolfVideosFolder()](#setoverwolfvideosfolderpath-callback)
* [overwolf.settings.getOverwolfScreenshotsFolder()](#getoverwolfscreenshotsfoldercallback)
* [overwolf.settings.setOverwolfScreenshotsFolder()](#setoverwolfscreenshotsfolderpath-callback)
* [overwolf.settings.getVideoCaptureSettings()](#getvideocapturesettingscallback)
* [overwolf.settings.setVideoCaptureSettings()](#setvideocapturesettingsresolutionsettings-fps-callback)
* [overwolf.settings.getAudioCaptureSettings()](#getaudiocapturesettingscallback)
* [overwolf.settings.setAudioCaptureSettings()](#setaudiocapturesettingsenablesound-enablemicrophone-callback)
* [overwolf.settings.getFpsSettings()](#getfpssettingscallback)
* [overwolf.settings.setFpsSettings()](#setfpssettingssettings-callback)
* [overwolf.settings.setExtensionSettings()](#setextensionsettingsextensionsettings-callback)
* [overwolf.settings.getExtensionSettings()](#getextensionsettingscallback)

## Events Reference

* [overwolf.settings.onFpsSettingsChanged](#onfpssettingschanged)
* [overwolf.settings.OnVideoCaptureSettingsChanged](#onvideocapturesettingschanged)
* [overwolf.settings.OnAudioCaptureSettingsChanged](#onaudiocapturesettingschanged)
* [overwolf.settings.OnHotKeyChanged](#onhotkeychanged)

## Types Reference

* [overwolf.settings.enums.ResolutionSettings](#resolutionsettings-enum) enum
* [overwolf.settings.enums.eIndicationPosition](#eindicationposition-enum) enum
* [FpsSettings](#fpssettings-object) Object
* [GetFpsSettingsResult](#getfpssettingsresult-object) Object
* [GeneralExtensionSettings](#generalextensionsettings-object) Object
* [GetExtensionSettingsResult](#getextensionsettingsresult-object) Object
* [FolderResult](#folderresult-object) Object
* [Path](#path-object) Object
* [GetVideoCaptureSettingsResult](#getvideocapturesettingsresult-object) Object
* [GetAudioCaptureSettingsResult](#getaudiocapturesettingsresult-object) Object
* [FpsSettingsChangedEvent](#fpssettingschangedevent-object) Object
* [VideoCaptureSettingsChangedEvent](#videocapturesettingschangedevent-object) Object
* [AudioCaptureSettingsChangedEvent](#audiocapturesettingschangedevent-object) Object

## getHotKey(featureId, callback)
#### Version added: 0.78
#### Permissions required: Hotkeys

> Returns the hotkey assigned to a given feature id by calling the callback.

:::warning OBSOLETE
This function is obsolete.  Instead, please use [overwolf.settings.hotkeys.get()](overwolf-settings-hotkeys#getcallback).
:::

Parameter | Type                  | Description                                                             |
--------- | ----------------------| ----------------------------------------------------------------------- |
featureId | string                | The feature id for which to get the set hotkey                           |
callback  | function              | A callback function which will be called with the status of the request |

#### Callback argument: Success

A callback function which will be called with the status of the request

```json
{ 
    "status": "success",
    "hotkey ": "Ctrl+F2"
}
```

## registerHotKey(actionId, callback)
#### Version added: 0.78
#### Permissions required: Hotkeys

> Registers a callback for a given hotkey action.

:::warning OBSOLETE
This function is obsolete. Instead, please register to the [overwolf.settings.hotkeys.onPressed](overwolf-settings-hotkeys#onpressed) event.
:::

Parameter | Type                  | Description                                                             |
--------- | ----------------------| ----------------------------------------------------------------------- |
actionId  | string                | The action id for which to register the callback                        |
callback  | function              | The function to run when the hotkey is pressed                           |

:::important
If you are using a transparnet background controller (window), you must register the hotkey in that window.
:::

#### Callback argument: Success

On successful registration, the callback function which will be called  when the hotkey is pressed.
A Note regarding hotkeys: Shift can only be combined with the F keys.

```json
{ 
    "status": "success",
    "hotkey ": "Ctrl+F2"
}
```

#### Callback argument: Failure

If the registration had failed, the callback function will be called immediately with the status "error" and another property,
“error”, indicating the reason for the failure.

```json
{ "status": "error", "error": "something went wrong..." }
```

#### Usage Example

If your manifest.json file defined these hotkeys:

```json
"data": {
    "hotkeys" : {
        "my_cool_action": {
            "title": "My Cool Action",
            "action-type": "custom",
            "default": "Ctrl+Alt+C"
        }
    }
}
```

A call to register this hotkey should be look like this:

```js
overwolf.settings.registerHotKey(
    "my_cool_action",
    function(arg) {
        if (arg.status == "success") {
            alert("This is my cool action!");
        }
    }
);
```

## getCurrentOverwolfLanguage(callback)
#### Version added: 0.85

> Returns the current language overwolf is set to in a two letter ISO name format.

:::warning OBSOLETE
This function is obsolete.  Instead, please use [overwolf.settings.language]() API.
:::

Parameter | Type                  | Description                                                             |
--------- | ----------------------| ----------------------------------------------------------------------- |
callback  | function              |  A callback function which will be called with the status of the request|

#### Callback argument: Success

 A callback function which will be called with the status of the request and the current language overwolf is set to:

```json
{ 
    "status": "success",
    "language ": "en"
}
```

## getOverwolfVideosFolder(callback)
#### Version added: 0.86

> Returns the current folder overwolf uses to store videos.

Parameter | Type                  | Description                                                             |
--------- | ----------------------| ----------------------------------------------------------------------- |
callback  | [(Result:FolderResult )](#folderresult-object) => void | called with the status of the request |


## setOverwolfVideosFolder(path, callback)
#### Version added: 0.119

> Sets the folder Overwolf uses to store videos.

Parameter | Type                  | Description                                                              |
--------- | ----------------------| -----------------------------------------------------------------------  |
path	  | string                | The folder to use                                                        |
callback  | [(Result:FolderResult )](#folderresult-object) => void | called with the status of the request |

```js
overwolf.settings.setOverwolfVideosFolder("C:\Users\Azamoth\Videos\Captures",console.log)
```

## getOverwolfScreenshotsFolder(callback)
#### Version added: 0.103

> Returns the current folder Overwolf uses to store screenshots and GIFs.

Parameter | Type                  | Description                                                                                              |
--------- | ----------------------| -------------------------------------------------------------------------------------------------------- |
callback  | [(Result:FolderResult )](#folderresult-object) => void | called with the result of the request which contains the current Overwolf screenshots folder |

## setOverwolfScreenshotsFolder(path, callback)
#### Version added: 0.119

> Sets the folder Overwolf uses to store screenshots.

Parameter | Type                  | Description                                                                                              |
--------- | ----------------------| -------------------------------------------------------------------------------------------------------- |
path	  | string                | The folder to use                                                                                        |
callback  | [(Result:FolderResult )](#folderresult-object) => void | called with the result of the request which contains the current Overwolf screenshots folder |

## getVideoCaptureSettings(callback)
#### Version added: 0.86

> Returns the current video capture settings.

Parameter | Type                  | Description                                                                                              |
--------- | ----------------------| -------------------------------------------------------------------------------------------------------- |
callback  | [(Result:GetVideoCaptureSettingsResult)](#getvideocapturesettingsresult-object) => void | called with the result of the request which contains the current Overwolf capture settings |

## setVideoCaptureSettings(resolutionSettings, fps, callback)
#### Version added: 0.117
#### Permissions required: VideoCaptureSettings

requires the |VideoCaptureSettings| permission.

> Sets new video capture settings.

Parameter          | Type                                                                  | Description                                              |
-------------------| ----------------------------------------------------------------------| -------------------------------------------------------- |
resolutionSettings | [overwolf.settings.ResolutionSettings](#resolutionsettings-enum) enum |                                                          |
fps                | int                                                                   |                                                          |
callback           | (Result) => void                                                      | called with the result of the request                    |

## getAudioCaptureSettings(callback)
#### Version added: 0.117

> Returns the current audio capture settings.

Parameter          | Type        | Description                                                              |
-------------------| ------------| ------------------------------------------------------------------------ |
callback  | [(Result:GetAudioCaptureSettingsResult)](#getaudiocapturesettingsresult-object) => void | called with the status of the request |

## setAudioCaptureSettings(enableSound, enableMicrophone, callback)
#### Version added: 0.117

> Sets new audio capture settings.

Parameter          | Type                                                                  | Description                                              |
-------------------| ----------------------------------------------------------------------| -------------------------------------------------------- |
enableSound	       | bool                                                                  | The folder to use                                        |
enableMicrophone   | bool                                                                  |                                                          |
callback           | (Result) => void                                                      | called with the result of the request                    |

## getFpsSettings(callback)
#### Version added: 0.89

> Gets the status of the FPS control (on/off), its position, its offset (in pixels) and its scale [0, 1].

Parameter          | Type               | Description                                                              |
-------------------| -------------------| ------------------------------------------------------------------------ |
callback           | ([Result: GetFpsSettingsResult](#getfpssettingsresult-object)) => void  |  A callback function which will be called with the status of the request |

## setFpsSettings(settings, callback)
#### Version added: 0.89

> Sets the state (on/off), position, offset (in pixels) and scale [0, 1] of the Fps control.

:::warning OBSOLETE
This function is obsolete.
:::

Parameter          | Type               | Description                                                              |
-------------------| -------------------| ------------------------------------------------------------------------ |
settings           | [FpsSettings](#fpssettings-object) Object |  Container for the FPS settings                   |
callback           | (Result) => void   | called with the result of the request                    |

## setExtensionSettings(extensionSettings, callback)
#### Version added: 0.149

> Sets the extension settings.  

Supports enabling/disabling app auto-launch with Overwolf client, exit with Overwolf client and more.

Parameter          | Type                                                                 | Description                                                              |
-------------------| ---------------------------------------------------------------------| ------------------------------------------------------------------------ |
extensionSettings  | [GeneralExtensionSettings](#generalextensionsettings-object) Object  |  Container for the extension settings                                   |
callback           | (Result) => void   | called with the result of the request                    |


## getExtensionSettings(callback)
#### Version added: 0.149

> Gets the extension settings.  

Parameter          | Type                                                                 | Description                                                              |
-------------------| ---------------------------------------------------------------------| ------------------------------------------------------------------------ |
callback           | ([Result: GetExtensionSettingsResult](#getextensionsettingsresult-object)) => void  |  A callback function which will be called with the status of the request |

## onFpsSettingsChanged

#### Version added: 0.89

> Fired when fps settings are changed, with the following structure: [FpsSettingsChangedEvent](#fpssettingschangedevent-object) Object.

## OnVideoCaptureSettingsChanged

#### Version added: 0.117

> Fired when video capture settings are changed, with the following structure: [VideoCaptureSettingsChangedEvent](#fpssettingschangedevent-object) Object.

## OnAudioCaptureSettingsChanged

#### Version added: 0.117

> Fired when audio capture settings are changed, with the following structure: [AudioCaptureSettingsChangedEvent](#audiocapturesettingschangedevent-object) Object.

## OnHotKeyChanged

#### Version added: 0.119

> Fired when a hotkey is modified. Apps will only be notified of hotkey changes that relate to them.

:::warning OBSOLETE
This event is obsolete.  Instead, please use the  [overwolf.settings.hotkeys.onChanged](overwolf-settings-hotkeys#onchanged) event.
:::

#### Event Data Example: Success

```json
{ "source": "replayhud_save", "description": "Replay HUD: Save Replay for later", "hotkey": "Ctrl+Shift+F7" }
```

## ResolutionSettings enum
#### Version added: 0.78

> Describes Resolution settings.

Option    | Description  |
--------- | -------------|
Original  |              |
R1080p    |              |
R720p     |              |
R480p     |              |

## eIndicationPosition enum
#### Version added: 0.78

> Describes position to use as anchor.

Option            | Description  |
----------------- | -------------|
None              | -1           |
TopLeftCorner     |  0           |
TopRightCorner    |  1           |
BottomLeftCorner  |  2           |
BottomRightCorner |  3           |

## FpsSettings Object
#### Version added: 0.78

> Container for the FPS settings.

Parameter | Type               | Description                          |
--------- | -------------------| ------------------------------------ |
offset    | Odkv2Point Object  | The offset from the edge (in pixels) |
scale     | double             | A scale (1.0 = original)             |
enabled   | bool               | Whether to enable or disable fps     |
position  | [eIndicationPosition](#eindicationposition-enum) enum      | The position (anchor) to use         |

#### Object Data Example

```json
{
    "settings": {
        "offset": {
            "x": 0,
            "y": 0
        },
        "scale": 1,
        "enabled": false,
        "position": 0
    }
}
```

## GeneralExtensionSettings Object
#### Version added: 0.149

> Container for the extension settings.

Parameter                   | Type               | Description                                                               |
--------------------------- | -------------------| ------------------------------------------------------------------------- |
auto_launch_with_overwolf   | bool               | Set your app to auto-launch when the OW client starts. See [notes](#auto_launch_with_overwolf-notes).     |
exit_overwolf_on_exit       | bool               | Set the OW client to auto-shutdown when your OW app closes. See [notes](#exit_overwolf_on_exit-notes).|

:::warning
The exit_overwolf_on_exit option shouldn’t be used without Overwolf’s permission
:::

```json
{
    "settings": {
        "auto_launch_with_overwolf": true,
        "exit_overwolf_on_exit": false
    }
}
```

#### `auto_launch_with_overwolf` notes

* After setting the "auto_launch_with_overwolf", your app should use auto-launch after you start the client (takes ~15 seconds).
* If you would like to set app auto-launch with OW client, you should add the "Tray" permission to your app's [manifest permissions list](manifest-json#permissions-array).
* You can set the same app auto-launch with OW client using the manifest. [Read more about it](manifest-json#enable-app-auto-launch-with-overwolf).
* Apps launched this way will have [origin](overwolf-extensions#the-origin-string) "overwolfstartlaunchevent".

#### `exit_overwolf_on_exit` notes

* If you would like to set app auto-launch with OW client, you should add the "Shutdown" permission to your app's [manifest permissions list](manifest-json#permissions-array).
* Currently you can NOT set the same auto-exit with OW client using the manifest. Maybe we will add this feature in the future.
* Overwolf client no not closes when an app that was using that setting has crashed, however, it will still close Overwolf if the user has dismissed the crash notification or didn’t click on the "Relaunch" button in that same notification.

## GetFpsSettingsResult Object

Parameter          | Type     | Description                                 |
-------------------| ---------| ------------------------------------------- |
*success*          | boolean  | inherited from the "Result" Object          |
*error*            | string   | inherited from the "Result" Object          |
settings           | [FpsSettings](#fpssettings-object) Object   | container for the FPS object |

#### Example data: Success

```json
{
    "status": "success",
    "settings": {
        "offset": {
            "x": 0,
            "y": 0
        },
        "scale": 1,
        "enabled": false,
        "position": 0
    }
}
```

## GetExtensionSettingsResult Object

Parameter          | Type     | Description                                 |
-------------------| ---------| ------------------------------------------- |
*success*          | boolean  | inherited from the "Result" Object          |
*error*            | string   | inherited from the "Result" Object          |
settings           | [GeneralExtensionSettings](#generalextensionsettings-object) Object   | Container for the extension settings |

#### Example data: Success

```json
{
    "success": true,
    "settings": {
        "auto_launch_with_overwolf": true
    }
}
```

## FolderResult Object

Parameter          | Type     | Description                                 |
-------------------| ---------| ------------------------------------------- |
*success*          | boolean  | inherited from the "Result" Object          |
*error*            | string   | inherited from the "Result" Object          |
path               | string   | Folder path |

#### Example data: Success

```json
{
    "status": "success",
    "path": "E:\Video\Overwolf"
}
```

## Path Object

> Container for the path entity

Parameter          | Type     | Description                                 |
-------------------| ---------| ------------------------------------------- |
Value              | string   |                                             |
Type               | string   |                                             |
Name               | string   |                                             |

#### Example data

```json
{
    "Value": "E:\Video\Overwolf",
    "Type": "System.String, mscorlib, Version=2.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089",
    "Name": "Folders_VideoCapturesFolder"
}
```

## GetVideoCaptureSettingsResult Object

Parameter          | Type     | Description                                 |
-------------------| ---------| ------------------------------------------- |
*success*          | boolean  | inherited from the "Result" Object          |
*error*            | string   | inherited from the "Result" Object          |
encoder            | string   |                                             |
preset             | string   |                                             |
fps                | number   |                                             |
resolution         | number   |                                             |

#### Example data: Success

```json
{
    "encoder": "NVIDIA_NVENC",
    "preset": "DEFAULT",
    "fps": 30,
    "resolution": 2
}
```

## GetAudioCaptureSettingsResult Object

Parameter          | Type     | Description                                 |
-------------------| ---------| ------------------------------------------- |
*success*          | boolean  | inherited from the "Result" Object          |
*error*            | string   | inherited from the "Result" Object          |
sound_enabled      | boolean  |                                             |
microphone_enabled | boolean  |                                             |

#### Example data: Success

```json
{
    "sound_enabled": true,
    "microphone_enabled": false
}
```

## FpsSettingsChangedEvent Object

Parameter     | Type                                                              | Description     |
--------------| ------------------------------------------------------------------|---------------- |
setting       |  string   ("OnScreenLocation" | "Enabled" | "Scale" | "Offset")           |                 | 

#### Event data example

```json
{ 
   "setting":"OnScreenLocation"
}
```

## VideoCaptureSettingsChangedEvent Object

Parameter     | Type                                                              | Description     |
--------------| ------------------------------------------------------------------|---------------- |
setting       |  string   ("resolution" | "fps" | "unknown" | "Offset")           |                 | 

#### Event data example

```json
{ 
   "setting":"resolution"
}
```

## AudioCaptureSettingsChangedEvent Object

Parameter     | Type                                                              | Description     |
--------------| ------------------------------------------------------------------|---------------- |
setting       |  string   ("speakers" | "microphone" | "unknown" | "Offset")           |                 | 

#### Event data example

```json
{ 
   "setting":"speakers"
}
```


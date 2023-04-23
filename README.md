# **WAIS-EHUD - ELECTRUS SERIES**


## How can i use new ESX?
* To use the new esx version, you must open the ***Config*** file and set the ***Config.NewESX*** variable in line 2 to `true @boolean`

## What is ***Config.DevMode***?
* If you enable this variable, you will ensure that it runs when you restart the script on the server. 

## How do I activate the map only in the vehicle?
* Open the config file and you will see a variable called ***Config.ShowMapOnlyInTheCar***. If you change this variable to `true @boolean`, the map will only work when you are in the car.

## How to activate Stress?
* If you change the variable named ***Config.StressSystem*** in the config file to `true @boolean`, you will see that the necessary settings for stress and stress are now active in hud. Note that for ESX this system will only reflect the data, there are no lines of code to increase or decrease stress. There is such a file for QB, you can download it from here. [QB STRESS](https://cdn.discordapp.com/attachments/1035485961217384488/1099749164302205088/qb-stress.rar)

## What is ***Config.RefreshTimes***?
* This regeneration time affects resmon. If you increase the number to 200 or 500, health, armor, food, thirst, stamina and stress information will be updated a little later. Increasing the number will cause the resmon to decrease and decreasing the number will cause it to increase. 


## How do I adapt Hud to a voice system?

* You can run hud with ***SALTYCHAT*** or ***PMA-VOICE*** thanks to this table structure ***Config.VoiceSettings*** in the config file.

## What should I do if I get such a crash
![alt text](https://forum.cfx.re/uploads/default/original/4X/e/8/6/e86d3cc7d96d4deb74f6b50601d43ebe2239af20.png)
* You are getting this error because you are using *saltychat* and have not set the distances. If you want to know how to set them, follow these steps:

* Open **Saltychat/config.json** and find this veriable **"VoiceRanges"**. Enter the numeric values you see in this variable into hud just like in this example:

```
Config.VoiceSettings = {
    ["saltychat"] = {
        ["use"] = true,
        ["ranges"] = {
            ["Range value. This value must be float (String)"] = {name = "Range Name (string)", meter = Range Value (Number))}

            ["3.0"] = {name = "Whisper", meter = 10},
            ["6.5"] = { name = "Normal", meter = 30},
            ["15.0"] = { name = "Shouting", meter = 100},
        }
    }
}
```

## Do not work in Carhud or Belt x Vehicle!
* If you want the belt or carhud to work in some vehicles, you must do the following:
* - Open the ***Config*** file and find the ***Config.BlackListVehicle*** variable.
* - Add a table without breaking the table structure as in the example:
```
Config.BlackListVehicle = {
    ['Vehicle Model Name'] = {
        ["carhud"] = true,
        ["belt"] = true
    }
}
```

## How Can I Hide Front-End Elements?
* If there is an element you want to hide in the front-end, you need the div name or id of this element.
* After finding the name of the element you want to hide, if the element is div, you need to use **".element-name"** like this, if the element is id, you need to use it like this **"#element-id"**.
* Then use this event to hide or unhide the element.

```
    Client side event. Event Name: "wais:customHide"
    TriggerEvent("wais:customHide", ".element-name", true or false) @boolean 
    TriggerClientEvent("wais:customHide", source, ".element-name", true or false) @boolean
```

## How Can I Hide Hud, Carhud or Map?
* You can hide or unhide using these events.

```
    Client side events:
    "wais:hideHud",
    "wais:hideCarHud",
    "wais:hideRadar"

    TriggerEvent("wais:hideHud", true or false) @boolean
    TriggerEvent("wais:hideCarHud", true or false) @boolean
    TriggerEvent("wais:hideRadar", true or false) @boolean
```

## How Can I Use Notification?
* If you want to use this notification system in general, you can follow these ways.

* For ESX:
>- Open es_extended folder
>- Open client folder
>- Open functions.lua
>- Find the function called ***ESX.ShowNotification***

```
function ESX.ShowNotification(message, type, length)
    if Config.NativeNotify then
        BeginTextCommandThefeedPost('STRING')
        AddTextComponentSubstringPlayerName(message)
        EndTextCommandThefeedPostTicker(0, 1)
    else
        exports["esx_notify"]:Notify(type, length, message)
    end
end

Change to this:

function ESX.ShowNotification(message, type, length)
    if Config.NativeNotify then
        BeginTextCommandThefeedPost('STRING')
        AddTextComponentSubstringPlayerName(message)
        EndTextCommandThefeedPostTicker(0, 1)
    else
        TriggerEvent("wais:addNotification", message, type, length)
    end
end
```

* For QB:
>- Open qb-core folder
>- Open client folder
>- Open functions.lua
>- Find the function called ***QBCore.Functions.Notify***

```
function QBCore.Functions.Notify(text, texttype, length)
    if type(text) == "table" then
        local ttext = text.text or 'Placeholder'
        local caption = text.caption or 'Placeholder'
        texttype = texttype or 'primary'
        length = length or 5000
        SendNUIMessage({
            action = 'notify',
            type = texttype,
            length = length,
            text = ttext,
            caption = caption
        })
    else
        texttype = texttype or 'primary'
        length = length or 5000
        SendNUIMessage({
            action = 'notify',
            type = texttype,
            length = length,
            text = text
        })
    end
end

Change to this:

function QBCore.Functions.Notify(text, texttype, length)
    if type(text) == "table" then
        local ttext = text.text or 'Placeholder'
        local caption = text.caption or 'Placeholder'
        texttype = texttype or 'primary'
        length = length or 5000
        TriggerEvent("wais:addNotification", ttext, texttype, length)
    else
        texttype = texttype or 'primary'
        length = length or 5000
        TriggerEvent("wais:addNotification", text, texttype, length)
    end
end

```

Or normally using:

```
TriggerEvent("wais:addNotification", "Title", "Message", "Type", 5000)
TriggerClientEvent("wais:addNotification", source, "Title", "Message", "Type", 5000)

--@title: string,
--@message: string,
--@type: string, [success, error, warning, information]
--@length: number
```

## How Can I Use Interactıve Notficiation?
* This notification type is only available in the client example:

```
TriggerEvent('wais:interactiveNotification', title, message, time, function(result)
    print(result)
end)

--@title: string,
--@message: string,
--@length: number

```

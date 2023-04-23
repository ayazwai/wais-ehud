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

## FOR vRP USERS 

ONLY FOR THE LATEST vRP WITH FACTIONS, NOT ONLY WITH GROUPS! YOU HAVE IN THE FOLDER A README.MD WITH THE NECESARY INSTRUCTIONS FOR INSTALLING THE SCRIPT RIGHT FOR YOUR SERVER !!!!!!!! ONLY DUNKO FUNCTIONS !!!!!!!!!!

For Money Hud, you need to use the next triggers at the vRP.setMoney and vRP.setBankMoney

```
TriggerClientEvent("wais:setData", source, {money = money + amount}) 


function vRP.setMoney(user_id,value)
  local tmp = vRP.getUserTmpTable(user_id)
  if tmp then
    tmp.wallet = value
  end

  local source = vRP.getUserSource(user_id)
  if source ~= nil then
    local accounts = {
      [1] = {name = "money", money = value},
      [2] = {name = "bank", money = vRP.getBankMoney(user_id)},
    }
    TriggerClientEvent("wais:setData", source, {type = "UPDATE_MONEY", object = accounts, cashItem = false, prefix = "." , symbol = "$"}) 
  end
end

function vRP.setBankMoney(user_id,value)
  local tmp = vRP.getUserTmpTable(user_id)
  if tmp then
    tmp.bank = value
  end
  local source = vRP.getUserSource(user_id)
  if source ~= nil then
    local accounts = {
      [1] = {name = "money", money = vRP.getMoney(user_id)},
      [2] = {name = "bank", money = value},
    }
    TriggerClientEvent("wais:setData", source, {type = "UPDATE_MONEY", object = accounts, cashItem = false, prefix = "." , symbol = "$"}) 
  end
end

function vRP.addUserFaction(user_id,theGroup)
	local player = vRP.getUserSource(user_id)
	if(player)then
		local ngroup = factions[theGroup]
		if ngroup then
			local factionRank = ngroup.fRanks[1].rank
			local tmp = vRP.getUserDataTable(user_id)
			if tmp then
				Player(player).state.faction = theGroup
				Player(player).state.factionGrade = 1
				Player(player).state.factionName = vRP.getFactionRankName(user_id,1)
				Player(player).state.salary = vRP.getFactionRankSalary(theGroup, factionRank)
				tmp.fName = theGroup
				tmp.fRank = factionRank
				tmp.fLeader = 0
				tmp.fCoLeader = 0
        TriggerClientEvent("wais:setData", source, {type = "UPDATE_JOB", name = theGroup, label = factionRank}) 
				exports.oxmysql:query("UPDATE vrp_users SET faction = @group, factionRank = @rank WHERE id = @user_id", {user_id = user_id, group = theGroup, rank = factionRank}, function()end)
				exports.oxmysql:query("SELECT * FROM vrp_users WHERE id = @user_id", {user_id = user_id}, function (rows)
					thePlayer = rows[1]
					table.insert(factionMembers[theGroup], thePlayer)
				end)
			end
		end
	end
end


function vRP.removeUserFaction(user_id,theGroup)
	local player = vRP.getUserSource(user_id)
	if(player)then
		local tmp = vRP.getUserDataTable(user_id)
		if tmp then
			for i, v in pairs(factionMembers[theGroup])do
				if(v.id == user_id)then
					Player(player).state.faction = "user"
					Player(player).state.factionGrade = 0 
					Player(player).state.factionName = "none"
					Player(player).state.salary = 0
					tmp.fName = "user"
					tmp.fRank = 'none'
					tmp.fLeader = 0
					tmp.fCoLeader = 0
          			TriggerClientEvent("wais:setData", source, {type = "UPDATE_JOB", name = "Somer", label = "Somer"}) 
					exports.oxmysql:query("UPDATE vrp_users SET faction = @group, factionRank = @rank, isFactionLeader = 0, isFactionCoLeader = 0 WHERE id = @user_id", {user_id = user_id, group = "user", rank = "none"})
					table.remove(factionMembers[theGroup], i)
				end
			end
		end
	else
		for i, v in pairs(factionMembers[theGroup])do
			if(v.id == user_id)then
				table.remove(factionMembers[theGroup], i)
				exports.oxmysql:query("UPDATE vrp_users SET faction = @group, factionRank = @rank, isFactionLeader = 0, isFactionCoLeader = 0 WHERE id = @user_id", {user_id = user_id, group = "user", rank = "none"}, function()end)
			end
		end
	end
end
```


For The PlayerId Display you need to add this trigger in vrp/base.lua at "vRPcli:playerSpawned" event, and should look like this 

```
TriggerClientEvent("wais:setData", source, {type = "PLAYER_LOADED", playerId = user_id}) 

RegisterServerEvent("vRPcli:playerSpawned")
AddEventHandler("vRPcli:playerSpawned", function()
    Debug.pbegin("playerSpawned")
    -- register user sources and then set first spawn to false
    local user_id = vRP.getUserId(source)
    local player = source
    if user_id ~= nil then
        vRP.user_sources[user_id] = source
        local tmp = vRP.getUserTmpTable(user_id)
        tmp.spawns = tmp.spawns+1
        local first_spawn = (tmp.spawns == 1)
        if first_spawn then
            for k,v in pairs(vRP.user_sources) do
                vRPclient.addPlayer(source,{v})
            end
            vRPclient.addPlayer(-1,{source})
        end
		TriggerClientEvent("wais:setData", source, {type = "PLAYER_LOADED", playerId = user_id}) 
        TriggerEvent("vRP:playerSpawn",user_id,player,first_spawn)
    end
    Debug.pend()
end)
```

For the Hunger/Thirst System you need to add this trigger in vrp/modules/survival.lua  at vRP.varyThirst and vRP.varyHunger functions, and should look like this

```
TriggerClientEvent("waisHud:onTick", vRP.getUserSource(user_id), {{name = "hunger", percent = data.hunger},{name = "thirst", percent = data.thirst}})

function vRP.varyThirst(user_id, variation)
    if vRPConfig.EnableFoodAndWater then 
        local data = vRP.getUserDataTable(user_id)
        if data then
            local was_thirsty = data.thirst >= 100
            data.thirst = data.thirst + variation
            local is_thirsty = data.thirst >= 100

            -- apply overflow as damage
            local overflow = data.thirst - 100
            if overflow > 0 then
                vRPclient.varyHealth(vRP.getUserSource(user_id), {-overflow * cfg.overflow_damage_factor})
            end

            if data.thirst < 0 then
                data.thirst = 0
            elseif data.thirst > 100 then
                data.thirst = 100
            end

            -- set progress bar data
            local source = vRP.getUserSource(user_id)
			TriggerClientEvent("waisHud:onTick", source, {{name = "hunger", percent = data.hunger},{name = "thirst", percent = data.thirst}})
        end
    end
end

function vRP.varyHunger(user_id, variation)
    if vRPConfig.EnableFoodAndWater then 
        local data = vRP.getUserDataTable(user_id)
        if data then
            local was_starving = data.hunger >= 100
            data.hunger = data.hunger + variation
            local is_starving = data.hunger >= 100

            -- apply overflow as damage
            local overflow = data.hunger - 100
            if overflow > 0 then
                vRPclient.varyHealth(vRP.getUserSource(user_id), {-overflow * cfg.overflow_damage_factor})
            end

            if data.hunger < 0 then
                data.hunger = 0
            elseif data.hunger > 100 then
                data.hunger = 100
            end

            -- set progress bar data
            local source = vRP.getUserSource(user_id)
			TriggerClientEvent("waisHud:onTick", source, {{name = "hunger", percent = data.hunger},{name = "thirst", percent = data.thirst}})
        end
    end
end
```

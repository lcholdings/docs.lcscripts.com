# Bad code examples

## Very bad code

{% code lineNumbers="true" fullWidth="true" %}
```lua
local iwantodie = true

Citizen.CreateThread(function()
    exports.ox_target:addGlobalVehicle({
        label = 'Bilmodifikationer',
        name = 'vehicle_mods_check',
        icon = 'fas fa-car',
        groups = 'police',
        event = 'LC-Utils:client:checkVehicleModsMenu'
    })
    while iwantodie do
        TriggerEvent("LC-Utils:client:checkVehicleModsMenu")
        Citizen.Wait(100000)
    end
end)

RegisterNetEvent('LC-Utils:client:checkVehicleModsMenu', function()
    local vehicle = lib.getClosestVehicle(GetEntityCoords(GetPlayerPed(-1), false))
    local vehiclePlate = GetVehicleNumberPlateText(vehicle)

    if vehiclePlate then
        local vehicleMods = lib.callback.await('LC-Utils:server:getVehicleMods', source, vehiclePlate)

        if vehicleMods then
            local Options = {}
            function isInstalled(bool) if bool then return 'Ja' else return 'Nej' end end
            function turboInstalled(modTurbo) if modTurbo ~= false then return 'Ja' else return 'Nej' end end
            Options[#Options + 1] = {
                title = "Belysnings Kontroll",
                description = "Installerat: " .. isInstalled(vehicleMods.lcInstalled),
                icon = 'palette',
                arrow = false,
            }
            Options[#Options + 1] = {
                title = "RGB Xenon",
                description = "Installerat: " .. isInstalled(vehicleMods.lcXenons),
                icon = 'lightbulb',
                arrow = false,
            }
            Options[#Options + 1] = {
                title = "RGB Underglow",
                description = "Installerat: " .. isInstalled(vehicleMods.lcUnderglow),
                icon = 'lightbulb',
                arrow = false,
            }
            Options[#Options + 1] = {
                title = "Stansningskit",
                description = "Installerat: " .. isInstalled(vehicleMods.enableStance),
                icon = 'wrench',
                arrow = false,
            }
            Options[#Options + 1] = {
                title = "Turbocharger",
                description = "Installerat: " .. turboInstalled(vehicleMods.modTurbo),
                icon = 'fan',
                arrow = false,
            }

            lib.registerContext({
                id = 'vehicleModsMenu',
                title = 'Bilmodifikationer',
                options = Options
            })
            lib.showContext('vehicleModsMenu')
        else
            QBCore.Functions.Notify('Inga bilmodifikationer hittades', 'error')
        end
    else
        QBCore.Functions.Notify('Ingen bil finns in närheten', 'error')
    end
end)
```
{% endcode %}

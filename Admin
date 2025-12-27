-- AdminCommands.lua
-- Sistema simple de comandos de administrador (educativo)

local AdminCommands = {}

-- Lista de admins (IDs o nombres)
AdminCommands.Admins = {
    ["Player1"] = true,
    ["DevUser"] = true
}

--------------------------------------------------
-- Utilidades
--------------------------------------------------
local function isAdmin(player)
    return AdminCommands.Admins[player.name] == true
end

local function split(str)
    local t = {}
    for word in string.gmatch(str, "%S+") do
        table.insert(t, word)
    end
    return t
end

--------------------------------------------------
-- Comandos disponibles
--------------------------------------------------
AdminCommands.Commands = {}

-- /help
AdminCommands.Commands.help = function(player)
    player:sendMessage(
        "/help, /tp x y z, /speed n, /god, /ungod"
    )
end

-- /tp x y z
AdminCommands.Commands.tp = function(player, args)
    local x = tonumber(args[2])
    local y = tonumber(args[3])
    local z = tonumber(args[4])

    if x and y and z then
        player:setPosition({ x = x, y = y, z = z })
    else
        player:sendMessage("Uso: /tp x y z")
    end
end

-- /speed n
AdminCommands.Commands.speed = function(player, args)
    local speed = tonumber(args[2])
    if speed then
        player:setWalkSpeed(speed)
    else
        player:sendMessage("Uso: /speed número")
    end
end

-- /god
AdminCommands.Commands.god = function(player)
    player:setInvincible(true)
    player:sendMessage("Modo dios activado")
end

-- /ungod
AdminCommands.Commands.ungod = function(player)
    player:setInvincible(false)
    player:sendMessage("Modo dios desactivado")
end

-- /kick nombre (simulado)
AdminCommands.Commands.kick = function(player, args)
    local targetName = args[2]
    if not targetName then
        player:sendMessage("Uso: /kick nombre")
        return
    end

    for _, p in pairs(Server:getPlayers()) do
        if p.name == targetName then
            p:sendMessage("Has sido expulsado por un admin")
            p:disconnect()
            return
        end
    end

    player:sendMessage("Jugador no encontrado")
end

--------------------------------------------------
-- Listener del chat
--------------------------------------------------
function AdminCommands:onChat(player, message)
    if string.sub(message, 1, 1) ~= "/" then return end
    if not isAdmin(player) then
        player:sendMessage("No tienes permisos")
        return
    end

    local args = split(message)
    local cmdName = string.sub(args[1], 2)

    local command = self.Commands[cmdName]
    if command then
        command(player, args)
    else
        player:sendMessage("Comando desconocido. Usa /help")
    end
end

--------------------------------------------------
-- Inicialización
--------------------------------------------------
Server:on("playerChat", function(player, message)
    AdminCommands:onChat(player, message)
end)

return AdminCommands

# Ex-paineladminroblox
-- Services
local Players = game:GetService("Players")

-- Admins
local Admins = {"GHOST001"} -- Seu username exato

-- Função para verificar se o jogador é admin
local function isAdmin(player)
    for _, name in ipairs(Admins) do
        if player.Name == name then
            return true
        end
    end
    return false
end

-- Kick all players (menos admins)
local function kickAll()
    for _, player in pairs(Players:GetPlayers()) do
        if not isAdmin(player) then
            player:Kick("Você foi expulso pelo admin")
        end
    end
end

-- Kill all players (menos admins)
local function killAll()
    for _, player in pairs(Players:GetPlayers()) do
        if player.Character and not isAdmin(player) then
            local humanoid = player.Character:FindFirstChildOfClass("Humanoid")
            if humanoid then
                humanoid.Health = 0
            end
        end
    end
end

-- Freeze player
local function freezePlayer(player)
    if player.Character then
        for _, part in pairs(player.Character:GetChildren()) do
            if part:IsA("BasePart") then
                part.Anchored = true
            end
        end
    end
end

-- Jail player
local function jailPlayer(player)
    if player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
        player.Character.HumanoidRootPart.CFrame = CFrame.new(0, 10, 0) -- coordenadas da prisão
    end
end

-- Comandos via chat
local function onChatted(player, message)
    if not isAdmin(player) then return end

    local msg = message:lower()

    if msg == "!kickall" then
        kickAll()
    elseif msg == "!killall" then
        killAll()
    elseif msg:match("!freeze (%w+)") then
        local targetName = msg:match("!freeze (%w+)")
        local target = Players:FindFirstChild(targetName)
        if target then freezePlayer(target) end
    elseif msg:match("!jail (%w+)") then
        local targetName = msg:match("!jail (%w+)")
        local target = Players:FindFirstChild(targetName)
        if target then jailPlayer(target) end
    end
end

-- Conectar o PlayerAdded
Players.PlayerAdded:Connect(function(player)
    player.Chatted:Connect(function(msg)
        onChatted(player, msg)
    end)
end)

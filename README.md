--// Stack Hub - Kick Player Menu
--// Script único (LocalScript)

local Players = game:GetService("Players")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local LocalPlayer = Players.LocalPlayer

-- Criar evento remoto (só uma vez)
local remote = ReplicatedStorage:FindFirstChild("StackHubKick")
if not remote then
    remote = Instance.new("RemoteEvent")
    remote.Name = "StackHubKick"
    remote.Parent = ReplicatedStorage

    -- Parte servidor (roda no servidor)
    remote.OnServerEvent:Connect(function(player, targetName)
        if player.UserId == game.CreatorId then -- só o dono pode usar
            local target = Players:FindFirstChild(targetName)
            if target then
                target:Kick("Você foi expulso pelo dono do jogo via Stack Hub!")
            end
        end
    end)
end

-- Interface
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "StackHub"
screenGui.ResetOnSpawn = false
screenGui.Parent = LocalPlayer:WaitForChild("PlayerGui")

local frame = Instance.new("Frame", screenGui)
frame.Position = UDim2.new(0.35, 0, 0.3, 0)
frame.Size = UDim2.new(0, 300, 0, 150)
frame.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
frame.BorderSizePixel = 2

local title = Instance.new("TextLabel", frame)
title.Text = "STACK HUB"
title.Font = Enum.Font.Code
title.TextSize = 22
title.TextColor3 = Color3.fromRGB(255, 0, 0)
title.Size = UDim2.new(1, 0, 0, 30)
title.BackgroundTransparency = 1

local textBox = Instance.new("TextBox", frame)
textBox.PlaceholderText = "Nome do player"
textBox.Font = Enum.Font.Code
textBox.TextSize = 18
textBox.Size = UDim2.new(0.8, 0, 0, 40)
textBox.Position = UDim2.new(0.1, 0, 0.4, 0)
textBox.BackgroundColor3 = Color3.fromRGB(50, 0, 0)
textBox.TextColor3 = Color3.fromRGB(255, 255, 255)

local kickButton = Instance.new("TextButton", frame)
kickButton.Text = "Kick Player"
kickButton.Font = Enum.Font.Code
kickButton.TextSize = 18
kickButton.Size = UDim2.new(0.8, 0, 0, 40)
kickButton.Position = UDim2.new(0.1, 0, 0.75, 0)
kickButton.BackgroundColor3 = Color3.fromRGB(60, 0, 0)
kickButton.TextColor3 = Color3.fromRGB(255, 120, 120)

kickButton.MouseButton1Click:Connect(function()
    local targetName = textBox.Text
    if targetName ~= "" then
        remote:FireServer(targetName)
    end
end)

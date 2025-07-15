-- Script de Teletransporte para Brookhaven
-- Pressione F para se teletransportar para onde o cursor está apontando

local Players = game:GetService("Players")
local UserInputService = game:GetService("UserInputService")
local RunService = game:GetService("RunService")

local player = Players.LocalPlayer
local mouse = player:GetMouse()

-- Função para teletransportar o jogador
local function teleportToMouse()
    local character = player.Character
    if not character then return end
    
    local humanoidRootPart = character:FindFirstChild("HumanoidRootPart")
    if not humanoidRootPart then return end
    
    -- Pega a posição do cursor no mundo 3D
    local targetPosition = mouse.Hit.Position
    
    -- Adiciona um pequeno offset para evitar ficar dentro do chão
    targetPosition = targetPosition + Vector3.new(0, 3, 0)
    
    -- Teletransporta o jogador
    humanoidRootPart.CFrame = CFrame.new(targetPosition)
end

-- Função para detectar quando a tecla F é pressionada
local function onKeyPress(key)
    if key.KeyCode == Enum.KeyCode.F then
        teleportToMouse()
    end
end

-- Conecta o evento de input
UserInputService.InputBegan:Connect(onKeyPress)

-- Feedback visual (opcional) - mostra um indicador onde você vai se teletransportar
local indicator = Instance.new("Part")
indicator.Name = "TeleportIndicator"
indicator.Size = Vector3.new(2, 0.1, 2)
indicator.Material = Enum.Material.Neon
indicator.BrickColor = BrickColor.new("Bright blue")
indicator.Anchored = true
indicator.CanCollide = false
indicator.Transparency = 0.5
indicator.Parent = workspace

-- Atualiza a posição do indicador constantemente
RunService.Heartbeat:Connect(function()
    if mouse.Hit then
        indicator.Position = mouse.Hit.Position + Vector3.new(0, 0.1, 0)
    end
end)

-- Função para limpar o script quando necessário
local function cleanup()
    if indicator then
        indicator:Destroy()
    end
end

-- Limpa quando o jogador sai
player.CharacterRemoving:Connect(cleanup)

print("Script de teletransporte carregado! Pressione F para se teletransportar para onde o cursor está apontando.")

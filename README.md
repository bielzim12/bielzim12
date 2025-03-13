local players = game:GetService("Players")
local localPlayer = players.LocalPlayer
local espEnabled = false

-- Criando GUI do menu
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Parent = game.CoreGui

local Frame = Instance.new("Frame")
Frame.Size = UDim2.new(0, 200, 0, 100)
Frame.Position = UDim2.new(0, 10, 0, 10)
Frame.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
Frame.Parent = ScreenGui

local Button = Instance.new("TextButton")
Button.Size = UDim2.new(0, 180, 0, 50)
Button.Position = UDim2.new(0, 10, 0, 25)
Button.Text = "Ativar ESP"
Button.BackgroundColor3 = Color3.fromRGB(0, 200, 0)
Button.Parent = Frame

-- Função para ativar/desativar ESP
local function toggleESP()
    espEnabled = not espEnabled
    Button.Text = espEnabled and "Desativar ESP" or "Ativar ESP"
    Button.BackgroundColor3 = espEnabled and Color3.fromRGB(200, 0, 0) or Color3.fromRGB(0, 200, 0)

    for _, player in pairs(players:GetPlayers()) do
        if player ~= localPlayer then
            if espEnabled then
                local highlight = Instance.new("Highlight")
                highlight.Parent = player.Character or player.CharacterAdded:Wait()
                highlight.FillColor = Color3.fromRGB(255, 0, 0) -- Vermelho
                highlight.FillTransparency = 0.5
                highlight.OutlineTransparency = 0
                highlight.Name = "ESPHighlight"
            else
                if player.Character then
                    local esp = player.Character:FindFirstChild("ESPHighlight")
                    if esp then
                        esp:Destroy()
                    end
                end
            end
        end
    end
end

-- Conectar botão ao ESP
Button.MouseButton1Click:Connect(toggleESP)

-- Atualiza ESP para novos jogadores
players.PlayerAdded:Connect(function(player)
    if espEnabled then
        local highlight = Instance.new("Highlight")
        highlight.Parent = player.Character or player.CharacterAdded:Wait()
        highlight.FillColor = Color3.fromRGB(255, 0, 0)
        highlight.FillTransparency = 0.5
        highlight.OutlineTransparency = 0
        highlight.Name = "ESPHighlight"
    end
end)

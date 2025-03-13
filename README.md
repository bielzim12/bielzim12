local Fluent = loadstring(game:HttpGet("https://github.com/dawid-scripts/Fluent/releases/latest/download/main.lua"))()
local Window = Fluent:CreateWindow({
    Title = "ESP Menu",
    SubTitle = "By Fluent",
    TabWidth = 160,
    Size = UDim2.fromOffset(580, 460),
    Acrylic = true, 
    Theme = "Dark",
    MinimizeKey = Enum.KeyCode.LeftControl
})

local Tabs = {
    Main = Window:AddTab({ Title = "ESP", Icon = "" })
}

local Options = Fluent.Options

-- Função para criar ESP
local function createESP(player)
    if player == game.Players.LocalPlayer then return end
    local highlight = Instance.new("Highlight")
    highlight.Parent = player.Character or player.CharacterAdded:Wait()
    highlight.Adornee = player.Character
    highlight.FillColor = Color3.fromRGB(255, 0, 0) -- Vermelho
    highlight.FillTransparency = 0.5
    highlight.OutlineColor = Color3.fromRGB(255, 255, 255)
    highlight.OutlineTransparency = 0
end

-- Função para ativar ESP em todos os jogadores
local function applyESP()
    for _, player in pairs(game.Players:GetPlayers()) do
        createESP(player)
    end
end

-- Ativa ESP em jogadores que entrarem depois
game.Players.PlayerAdded:Connect(function(player)
    player.CharacterAdded:Connect(function()
        createESP(player)
    end)
end)

-- Botão para ativar ESP
Tabs.Main:AddButton({
    Title = "Ativar ESP",
    Description = "Destaca jogadores com um contorno",
    Callback = function()
        applyESP()
        Fluent:Notify({
            Title = "ESP Ativado",
            Content = "Os jogadores agora estão destacados!",
            Duration = 5
        })
    end
})

Window:SelectTab(1)

Fluent:Notify({
    Title = "ESP Script",
    Content = "O ESP foi carregado com sucesso!",
    Duration = 8
})

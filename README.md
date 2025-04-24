local Fluent = loadstring(game:HttpGet("https://github.com/dawid-scripts/Fluent/releases/latest/download/main.lua"))()

-- Garantir que o Fluent foi carregado corretamente
if not Fluent then
    warn("Falha ao carregar o Fluent.")
    return
end

-- Criar a janela principal
local Window = Fluent:CreateWindow({
    Title = "Fluent " .. Fluent.Version,
    TabWidth = 120,
    Size = UDim2.fromOffset(480, 360),
    Theme = "Dark"
})

-- Criar as Tabs
local Tabs = {
    Classes = Window:AddTab({ Title = "Classes" }),
    ScriptsDeTPs = Window:AddTab({ Title = "Scripts" }),
    Creditos = Window:AddTab({ Title = "Créditos (feito por Cezar)" })
}

-- CLASSES
local classes = {
    "Horse", "Musician", "Doctor", "Miner", "Arsonist", "Packmaster",
    "Necromancer", "Werewolf", "High Roller", "Conductor", "Cowboy",
    "The Alamo", "Zombie", "Vampire", "Priest", "Survivalist", "Ironclad"
}

for _, class in ipairs(classes) do
    Tabs.Classes:AddButton({
        Title = "Comprar " .. class,
        Callback = function()
            game:GetService("ReplicatedStorage")
                :WaitForChild("Shared")
                :WaitForChild("RemotePromise")
                :WaitForChild("Remotes")
                :WaitForChild("C_BuyClass"):FireServer(class)
        end
    })
end

-- SCRIPT PARA ZERAR
Tabs.ScriptsDeTPs:AddButton({
    Title = "Executar DeadRails",
    Callback = function()
        loadstring(game:HttpGet("https://raw.githubusercontent.com/gumanba/Scripts/main/DeadRails"))()
    end
})

Tabs.ScriptsDeTPs:AddButton({
    Title = "null-fire",
    Callback = function()
        loadstring(game:HttpGet("https://raw.githubusercontent.com/InfernusScripts/Null-Fire/main/Loader"))()
    end
})

-- ESP com distância
local espEnabled = false
local espGuiTable = {}
local runService = game:GetService("RunService")
local localPlayer = game.Players.LocalPlayer

function createESP(player)
    if not player.Character or not player.Character:FindFirstChild("Head") then return end

    local billboard = Instance.new("BillboardGui")
    billboard.Name = "ESPGui"
    billboard.Adornee = player.Character.Head
    billboard.Size = UDim2.new(0, 100, 0, 40)
    billboard.AlwaysOnTop = true

    local text = Instance.new("TextLabel")
    text.Size = UDim2.new(1, 0, 1, 0)
    text.BackgroundTransparency = 1
    text.TextColor3 = Color3.fromRGB(255, 0, 0)
    text.TextStrokeTransparency = 0.5
    text.TextScaled = true
    text.Font = Enum.Font.SourceSansBold
    text.Text = player.Name

    text.Parent = billboard
    billboard.Parent = player.Character.Head
    espGuiTable[player] = { gui = billboard, label = text }
end

function removeESP(player)
    if espGuiTable[player] then
        espGuiTable

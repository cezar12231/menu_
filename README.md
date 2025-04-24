local Fluent = loadstring(game:HttpGet("https://github.com/dawid-scripts/Fluent/releases/latest/download/main.lua"))()

local Window = Fluent:CreateWindow({
    Title = "Fluent " .. Fluent.Version,
    TabWidth = 120,
    Size = UDim2.fromOffset(480, 360),
    Theme = "Dark"
})

-- Tabs
local Tabs = {
    Script = Window:AddTab({ Title = "Script" }),
    Classes = Window:AddTab({ Title = "Classes" }),
    Creditos = Window:AddTab({ Title = "Créditos" })
})

-- Botão: Skull Hub
Tabs.Script:AddButton({
    Title = "1 - Skull hub:",
    Callback = function()
        pcall(function() -- Usando pcall para evitar erros de execução inesperados
            loadstring(game:HttpGet('https://skullhub.xyz/loader.lua'))()
        end)
    end
})

-- Lista de classes
local classList = {
    "Horse", "Musician", "Doctor", "Miner", "Arsonist", "Packmaster",
    "Necromancer", "Werewolf", "High Roller", "Conductor", "Cowboy",
    "The Alamo", "Zombie", "Vampire", "Priest", "Survivalist", "Ironclad"
}

for _, class in ipairs(classList) do
    Tabs.Classes:AddButton({
        Title = "Comprar " .. class,
        Callback = function()
            pcall(function() -- Usando pcall para prevenir erros ao tentar comprar classe
                local args = { [1] = class }
                game:GetService("ReplicatedStorage")
                    :WaitForChild("Shared")
                    :WaitForChild("RemotePromise")
                    :WaitForChild("Remotes")
                    :WaitForChild("C_BuyClass"):FireServer(unpack(args))
            end)
        end
    })
end

-- Créditos
Tabs.Creditos:AddParagraph({
    Title = "Criado por",
    Content = "Cezar"
})

Tabs.Creditos:AddParagraph({
    Title = "Suporte",
    Content = "cezarr0294"
})

-- Botão de minimizar
local isMinimized = false

local function toggleWindow()
    isMinimized = not isMinimized
    Window.Visible = not isMinimized
end

local minimizeButton = Instance.new("TextButton")
minimizeButton.Size = UDim2.new(0, 100, 0, 50)
minimizeButton.Position = UDim2.new(0, 10, 1, -10)
minimizeButton.AnchorPoint = Vector2.new(0, 1)
minimizeButton.Text = "Minimizar"
minimizeButton.Parent = game.Players.LocalPlayer:WaitForChild("PlayerGui")

minimizeButton.MouseButton1Click:Connect(function()
    toggleWindow()
    minimizeButton.Text = isMinimized and "Abrir Menu" or "Minimizar"
end)

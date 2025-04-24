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
    Script = Window:AddTab({ Title = "Script" }),
    Classes = Window:AddTab({ Title = "Classes" }),
    Creditos = Window:AddTab({ Title = "Créditos" })
}

-- Adicionar o botão para o "Skull Hub"
Tabs.Script:AddButton({
    Title = "1 - Skull Hub:",
    Callback = function()
        local success, err = pcall(function()
            loadstring(game:HttpGet('https://skullhub.xyz/loader.lua'))()
        end)
        if not success then
            Fluent:Notify({
                Title = "Erro",
                Content = "Erro ao carregar o Skull Hub: " .. err,
                Duration = 5
            })
        end
    end
})

-- Lista de classes para compra
local classList = {
    "Horse", "Musician", "Doctor", "Miner", "Arsonist", "Packmaster",
    "Necromancer", "Werewolf", "High Roller", "Conductor", "Cowboy",
    "The Alamo", "Zombie", "Vampire", "Priest", "Survivalist", "Ironclad"
}

for _, class in ipairs(classList) do
    Tabs.Classes:AddButton({
        Title = "Comprar " .. class,
        Callback = function()
            local success, err = pcall(function()
                local args = { [1] = class }
                game:GetService("ReplicatedStorage")
                    :WaitForChild("Shared")
                    :WaitForChild("RemotePromise")
                    :WaitForChild("Remotes")
                    :WaitForChild("C_BuyClass"):FireServer(unpack(args))
            end)
            if not success then
                Fluent:Notify({
                    Title = "Erro",
                    Content = "Erro ao comprar a classe " .. class .. ": " .. err,
                    Duration = 5
                })
            end
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

-- Garantir que a GUI fique visível e funcional
Window:Show()

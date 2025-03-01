-- Carregar a biblioteca de UI
local GUI = loadstring(game:HttpGet("https://raw.githubusercontent.com/Patskorn/GUI/main/Daino.lua"))()
local idk = GUI:new({Color = Color3.fromRGB(255, 255, 255)}) -- Define a cor do menu como branco

-- Criar abas
local AbaJogadores = idk:Tap("Jogadores")
local AbaArmas = idk:Tap("Armas")
local AbaBypass = idk:Tap("Bypass")
local AbaDupes = idk:Tap("Dupes Armas")
local AbaTrolls = idk:Tap("Trolls") -- Nova aba Trolls

-------------------------------
--        FUNÇÃO ESP         --
-------------------------------
local CorESP = "Red"

AbaJogadores:Dropdown("Cor do ESP", {"Red", "Blue"}, function(selecionado)
    CorESP = selecionado
end)

AbaJogadores:Button("Ativar ESP", function()
    for _, jogador in ipairs(game:GetService("Players"):GetPlayers()) do
        if jogador ~= game.Players.LocalPlayer and jogador.Character then
            local destaque = Instance.new("Highlight", jogador.Character)
            destaque.FillColor = (CorESP == "Red") and Color3.fromRGB(255, 0, 0) or Color3.fromRGB(0, 0, 255)
            destaque.OutlineTransparency = 1
        end
    end
end)

-------------------------------
--       PUXAR ARMAS         --
-------------------------------
local ArmaSelecionada = nil

function ObterTodasAsArmas()
    local armas = {}
    for _, ferramenta in ipairs(game:GetDescendants()) do
        if ferramenta:IsA("Tool") then
            table.insert(armas, ferramenta.Name)
        end
    end
    return armas
end

AbaArmas:Dropdown("Selecionar Arma", ObterTodasAsArmas(), function(selecionado)
    ArmaSelecionada = selecionado
end)

AbaArmas:Button("Puxar Arma", function()
    if ArmaSelecionada then
        for _, ferramenta in ipairs(game:GetDescendants()) do
            if ferramenta:IsA("Tool") and ferramenta.Name == ArmaSelecionada then
                ferramenta.Parent = game.Players.LocalPlayer.Backpack
            end
        end
    end
end)

-------------------------------
--       BYPASS DEX          --
-------------------------------
AbaBypass:Button("Dex Explorer", function()
    loadstring(game:HttpGet("https://cdn.wearedevs.net/scripts/Dex%20Explorer.txt"))()
end)

AbaBypass:Button("Bypass Dex", function()
    local function DesativarTodosOsScripts(obj)
        if obj:IsA("Script") or obj:IsA("LocalScript") then
            obj.Disabled = true
        end
        for _, filho in pairs(obj:GetChildren()) do
            DesativarTodosOsScripts(filho)
        end
    end
    DesativarTodosOsScripts(game)
end)

-------------------------------
--       DUPLICAR ARMAS      --
-------------------------------
local ArmaParaDuplicar = nil

function ObterArmasDoJogador()
    local armas = {}
    for _, ferramenta in ipairs(game.Players.LocalPlayer.Backpack:GetChildren()) do
        if ferramenta:IsA("Tool") then
            table.insert(armas, ferramenta.Name)
        end
    end
    return armas
end

AbaDupes:Dropdown("Selecionar Arma para Dupe", ObterArmasDoJogador(), function(selecionado)
    ArmaParaDuplicar = selecionado
end)

AbaDupes:Button("Duplicar", function()
    if ArmaParaDuplicar then
        for _, ferramenta in ipairs(game.Players.LocalPlayer.Backpack:GetChildren()) do
            if ferramenta:IsA("Tool") and ferramenta.Name == ArmaParaDuplicar then
                for i = 1, 5 do
                    local clone = ferramenta:Clone()
                    clone.Parent = game.Players.LocalPlayer.Backpack
                end
                break
            end
        end
    end
end)

-------------------------------
--       FLY (ABA TROLLS)     --
-------------------------------
AbaTrolls:Button("FLY", function()
    loadstring("\108\111\97\100\115\116\114\105\110\103\40\103\97\109\101\58\72\116\116\112\71\101\116\40\40\39\104\116\116\112\115\58\47\47\103\105\115\116\46\103\105\116\104\117\98\117\115\101\114\99\111\110\116\101\110\116\46\99\111\109\47\109\101\111\122\111\110\101\89\84\47\98\102\48\51\55\100\102\102\57\102\48\97\55\48\48\49\55\51\48\52\100\100\100\54\55\102\100\99\100\51\55\48\47\114\97\119\47\101\49\52\101\55\52\102\52\50\53\98\48\54\48\100\102\53\50\51\51\52\51\99\102\51\48\98\55\56\55\48\55\52\101\98\51\99\53\100\50\47\97\114\99\101\117\115\37\50\53\50\48\120\37\50\53\50\48\102\108\121\37\50\53\50\48\50\37\50\53\50\48\111\98\102\108\117\99\97\116\111\114\39\41\44\116\114\117\101\41\41\40\41\10\10")()
end)

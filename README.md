local Lib = loadstring(game:HttpGet("https://raw.githubusercontent.com/7yhx/kwargs_Ui_Library/main/source.lua"))()

-- Criação da UI
local UI = Lib:Create{
    Theme = "Dark", -- Tema da interface
    Size = UDim2.new(0, 555, 0, 400) -- Tamanho da interface
}

local Main = UI:Tab{
    Name = "Inicia"
}

local Divider = Main:Divider{
    Name = "Funções"
}

local QuitDivider = Main:Divider{
    Name = "Sair"
}

-- Função para teletransportar o jogador para a posição de outro jogador
local function teleportPlayerToTarget(player, targetPlayer)
    -- Verifica se o jogador tem um personagem e se o alvo também tem
    if player.Character and targetPlayer.Character then
        local targetPosition = targetPlayer.Character:WaitForChild("HumanoidRootPart").Position
        -- Teletransporta o jogador para a posição do alvo
        player.Character.HumanoidRootPart.CFrame = CFrame.new(targetPosition)
        print(player.Name .. " foi teletransportado para " .. targetPlayer.Name)
    end
end

-- Função para preencher o Dropdown com os nomes dos jogadores
local function getPlayersList()
    local playerNames = {}
    for _, player in pairs(game.Players:GetPlayers()) do
        table.insert(playerNames, player.Name)  -- Adiciona os nomes dos jogadores
    end
    return playerNames
end

-- Dropdown para escolher um jogador para teletransportar
local PlayersList = getPlayersList()

local TeleportDropdown = Divider:Dropdown{
    Name = "Escolha um jogador para teletransportar",
    Options = PlayersList, -- Lista de jogadores no jogo
    Callback = function(playerName)
        local player = game.Players:FindFirstChild(playerName)
        if player then
            local localPlayer = game.Players.LocalPlayer
            teleportPlayerToTarget(localPlayer, player)  -- Teletransporta o jogador local para o alvo
        end
    end
}

-- Botão para fechar a UI
local Quit = QuitDivider:Button{
    Name = "Fechar a biblioteca UI.",
    Callback = function()
        UI:Quit{
            Message = "Saindo...", -- Mensagem ao fechar a interface
            Length = 1 -- Tempo em segundos que a mensagem fica visível
        }
    end
}

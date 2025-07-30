-- Pegando o jogador local
local player = game.Players.LocalPlayer

-- Aguardando o Bastão de Faísca de Arco-íris na mochila do jogador
local tool = player.Backpack:WaitForChild("Bastão de Faísca de Arco-íris")

-- Função de spam simples
local function spam()
    while true do
        -- Verifica se o jogador ainda tem o bastão
        if player.Backpack:FindFirstChild(tool.Name) then
            -- Ativa o bastão
            tool:Activate()
            wait(0.1)  -- Aguarda 0.1 segundos entre cada ativação
        else
            break  -- Para o spam se o bastão não estiver mais na mochila
        end
    end
end

-- Inicia o spam
spam()

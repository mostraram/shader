-- Script único para ligar/desligar bastões com faíscas
-- Coloque este script no "ServerScriptService"

local ReplicatedStorage = game:GetService("ReplicatedStorage")
local StarterGui = game:GetService("StarterGui")
local player = game.Players.LocalPlayer
local ScreenGui = Instance.new("ScreenGui")
local TextButton = Instance.new("TextButton")
local ligado = false  -- Estado inicial do efeito (desligado)

-- Criar o RemoteEvent para comunicação entre cliente e servidor
local EfeitoAtivado = Instance.new("RemoteEvent")
EfeitoAtivado.Name = "EfeitoAtivado"
EfeitoAtivado.Parent = ReplicatedStorage

-- Criar o TextButton
ScreenGui.Parent = StarterGui
TextButton.Parent = ScreenGui
TextButton.Text = "Ligar Bastões"
TextButton.Size = UDim2.new(0, 200, 0, 50)
TextButton.Position = UDim2.new(0.5, -100, 0.5, -25)

-- Script de Criação dos Bastões com Efeito de Faíscas
local bastaoOriginal = game.Workspace:WaitForChild("Bastao")  -- Bastão original no Workspace

local efeitoDeFaixa = Instance.new("ParticleEmitter")  -- Efeito de partículas
efeitoDeFaixa.Lifetime = NumberRange.new(0.5, 1)  -- Tempo de vida da partícula
efeitoDeFaixa.Rate = 50  -- Quantidade de partículas por segundo
efeitoDeFaixa.Enabled = false  -- Começa com o efeito desativado

-- Função para criar bastões com efeito de faíscas
local function criarBastao()
    local novoBastao = bastaoOriginal:Clone()  -- Clona o bastão original
    novoBastao.Parent = game.Workspace  -- Adiciona o novo bastão no jogo
    novoBastao.Position = bastaoOriginal.Position + Vector3.new(0, 5, 0)  -- Coloca o novo bastão ligeiramente acima
    
    -- Adiciona o efeito de faíscas ao novo bastão
    local novoEfeito = efeitoDeFaixa:Clone()
    novoEfeito.Parent = novoBastao
    novoEfeito.Enabled = true  -- Ativa o efeito de faíscas
end

-- Função que alterna o estado do botão e cria os bastões
local function alternarEfeito()
    ligado = not ligado  -- Alterna entre ligado e desligado
    
    -- Muda o texto do botão para refletir o estado
    if ligado then
        TextButton.Text = "Desligar Bastões"
    else
        TextButton.Text = "Ligar Bastões"
    end
    
    -- Envia o comando para o servidor
    EfeitoAtivado:FireServer(ligado)
    
    -- Se ligar, cria bastões
    if ligado then
        for i = 1, 5 do  -- Cria 5 bastões (ajuste conforme necessário)
            criarBastao()
        end
    end
end

-- Quando o jogador clicar no botão
TextButton.MouseButton1Click:Connect(alternarEfeito)

-- Script do servidor (Server)
EfeitoAtivado.OnServerEvent:Connect(function(player, ativar)
    ligado = ativar  -- Atualiza o estado do efeito

    -- Se ligar, cria bastões
    if ligado then
        for i = 1, 5 do  -- Cria 5 bastões (ajuste conforme necessário)
            criarBastao()
        end
    end
end)

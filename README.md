-- Coloque este LocalScript em StarterPlayerScripts

local Players = game:GetService("Players")
local player = Players.LocalPlayer
local PlayerGui = player:WaitForChild("PlayerGui")

-- Função para esconder o botão de pulo
local function esconderBotaoPulo()
    local touchGui = PlayerGui:FindFirstChild("TouchGui")
    if not touchGui then return end

    local controlFrame = touchGui:FindFirstChild("TouchControlFrame")
    if not controlFrame then return end

    local jumpButton = controlFrame:FindFirstChild("JumpButton")
    if jumpButton then
        jumpButton.Visible = false
    end
end

-- Tenta esconder quando o jogo carregar
esconderBotaoPulo()

-- Se o PlayerGui mudar (ex: respawn), tenta de novo
PlayerGui.ChildAdded:Connect(function()
    esconderBotaoPulo()
end)

-- LocalScript em StarterPlayerScripts

local Players = game:GetService("Players")
local player = Players.LocalPlayer
local playerGui = player:WaitForChild("PlayerGui")

-- Cria GUI principal
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "HUDToggleGui"
ScreenGui.ResetOnSpawn = false
ScreenGui.Parent = playerGui

-- Cria botão de toggle
local ToggleButton = Instance.new("TextButton")
ToggleButton.Size = UDim2.new(0, 120, 0, 50)
ToggleButton.Position = UDim2.new(0.05, 0, 0.8, 0)
ToggleButton.Text = "Esconder HUD"
ToggleButton.Font = Enum.Font.GothamBold
ToggleButton.TextSize = 16
ToggleButton.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
ToggleButton.TextColor3 = Color3.fromRGB(255, 255, 255)
ToggleButton.Parent = ScreenGui

-- Bordas arredondadas
local UICorner = Instance.new("UICorner")
UICorner.CornerRadius = UDim.new(0, 15)
UICorner.Parent = ToggleButton

-- Sombra
local UIStroke = Instance.new("UIStroke")
UIStroke.Color = Color3.fromRGB(200, 200, 200)
UIStroke.Thickness = 2
UIStroke.Parent = ToggleButton

-- Variáveis
local hudVisivel = true
local touchGui
local dragging = false
local dragOffset
local moved = false -- <-- Novo: detecta se foi arrastado

-- Função para atualizar HUD
local function atualizarHUD()
	if touchGui then
		local touchControlFrame = touchGui:FindFirstChild("TouchControlFrame")
		local jumpButton = touchGui:FindFirstChild("JumpButton", true)

		if touchControlFrame then
			touchControlFrame.Visible = hudVisivel
		end
		if jumpButton then
			jumpButton.Visible = hudVisivel
		end
	end
end

-- Detecta quando TouchGui aparece
playerGui.ChildAdded:Connect(function(child)
	if child.Name == "TouchGui" then
		touchGui = child
		atualizarHUD()
	end
end)

-- Arrastar botão
ToggleButton.InputBegan:Connect(function(input)
	if input.UserInputType == Enum.UserInputType.Touch or input.UserInputType == Enum.UserInputType.MouseButton1 then
		dragging = true
		moved = false
		dragOffset = input.Position - ToggleButton.AbsolutePosition
	end
end)

ToggleButton.InputChanged:Connect(function(input)
	if dragging and (input.UserInputType == Enum.UserInputType.Touch or input.UserInputType == Enum.UserInputType.MouseMovement) then
		moved = true
		local newPos = UDim2.fromOffset(input.Position.X - dragOffset.X, input.Position.Y - dragOffset.Y)
		ToggleButton.Position = newPos
	end
end)

ToggleButton.InputEnded:Connect(function(input)
	if input.UserInputType == Enum.UserInputType.Touch or input.UserInputType == Enum.UserInputType.MouseButton1 then
		dragging = false
		if not moved then
			-- Só alterna se não arrastou
			hudVisivel = not hudVisivel
			ToggleButton.Text = hudVisivel and "Esconder HUD" or "Mostrar HUD"
			atualizarHUD()
		end
	end
end)

-- LocalScript - Pré-carrega o mapa num raio de 300 metros ao redor do jogador

local Players = game:GetService("Players")
local ContentProvider = game:GetService("ContentProvider")
local RunService = game:GetService("RunService")
local Workspace = game:GetService("Workspace")

local player = Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local radius = 300 -- raio de carregamento em studs
local scanInterval = 2 -- segundos entre cada varredura

-- Função para verificar distância
local function isWithinRadius(objPos, centerPos, radius)
	return (objPos - centerPos).Magnitude <= radius
end

-- Função principal de preload
local function preloadNearbyParts()
	if not character or not character:FindFirstChild("HumanoidRootPart") then return end
	local hrp = character.HumanoidRootPart
	local center = hrp.Position

	local objectsToPreload = {}

	for _, obj in ipairs(Workspace:GetDescendants()) do
		if obj:IsA("BasePart") and isWithinRadius(obj.Position, center, radius) then
			table.insert(objectsToPreload, obj)
		end
	end

	if #objectsToPreload > 0 then
		ContentProvider:PreloadAsync(objectsToPreload)
		print("✅ Precarregado", #objectsToPreload, "partes ao redor do jogador")
	end
end

-- Loop para atualizar periodicamente
while true do
	preloadNearbyParts()
	task.wait(scanInterval)
end

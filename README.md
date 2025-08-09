-- LocalScript - Pré-carrega o mapa num raio de 300 metros ao redor do jogador

local Players = game:GetService("Players")
local ContentProvider = game:GetService("ContentProvider")
local RunService = game:GetService("RunService")
local Workspace = game:GetService("Workspace")

local player = Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local radius = 300 -- raio de carregamento em studs
local scanInterval = 5 -- segundos entre cada varredura (aumentado para evitar travamento)
local batchSize = 50 -- número de partes por lote para pré-carregar

-- Função para verificar distância
local function isWithinRadius(objPos, centerPos, radius)
	return (objPos - centerPos).Magnitude <= radius
end

-- Função principal de preload em lotes
local function preloadNearbyParts()
	if not character or not character:FindFirstChild("HumanoidRootPart") then return end
	local hrp = character.HumanoidRootPart
	local center = hrp.Position

	local partsToPreload = {}

	for _, obj in ipairs(Workspace:GetDescendants()) do
		if obj:IsA("BasePart") and isWithinRadius(obj.Position, center, radius) then
			table.insert(partsToPreload, obj)
		end
	end

	-- Pré-carrega em lotes menores para evitar travamentos
	coroutine.wrap(function()
		for i = 1, #partsToPreload, batchSize do
			local batch = {}
			for j = i, math.min(i + batchSize - 1, #partsToPreload) do
				table.insert(batch, partsToPreload[j])
			end
			ContentProvider:PreloadAsync(batch)
			task.wait(0.1) -- pequena pausa para suavizar o impacto
		end
		print("✅ Pré-carregamento concluído:", #partsToPreload, "partes")
	end)()
end

-- Loop de verificação com segurança
task.spawn(function()
	while true do
		pcall(preloadNearbyParts)
		task.wait(scanInterval)
	end
end)

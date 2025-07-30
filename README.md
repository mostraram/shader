-- LocalScript em StarterPlayerScripts
local Lighting = game:GetService("Lighting")
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local player = Players.LocalPlayer

-- IDs das texturas (substitua pelos seus reais)
local TEXTURE_ID = {
	ColorMap = "rbxassetid://1234567890",
	MetalnessMap = "rbxassetid://1234567891",
	RoughnessMap = "rbxassetid://1234567892",
	NormalMap = "rbxassetid://1234567893",
}

-- Nome do objeto alvo que vai ter shader
local SHADER_OBJECT_NAME = "ShaderObjeto"

-- 🧱 Aplica SurfaceAppearance + Highlight
local function applyShaderToPart(part)
	if not part:FindFirstChild("SurfaceAppearance") then
		local surface = Instance.new("SurfaceAppearance")
		surface.Name = "SurfaceAppearance"
		surface.ColorMap = TEXTURE_ID.ColorMap
		surface.MetalnessMap = TEXTURE_ID.MetalnessMap
		surface.RoughnessMap = TEXTURE_ID.RoughnessMap
		surface.NormalMap = TEXTURE_ID.NormalMap
		surface.Parent = part
	end

	if not part:FindFirstChild("ShaderHighlight") then
		local hl = Instance.new("Highlight")
		hl.Name = "ShaderHighlight"
		hl.FillTransparency = 1
		hl.OutlineTransparency = 0.3
		hl.OutlineColor = Color3.fromRGB(255, 222, 100)
		hl.Adornee = part
		hl.Parent = part
	end
end

-- 🎨 Reaplica efeitos visuais no Lighting
local function ensureLightingEffects()
	local function safeInsert(className, props)
		if not Lighting:FindFirstChild(className) then
			local effect = Instance.new(className)
			for prop, value in pairs(props) do
				effect[prop] = value
			end
			effect.Name = className
			effect.Parent = Lighting
		end
	end

	safeInsert("BloomEffect", {
		Intensity = 1.2,
		Size = 56,
		Threshold = 0.8
	})

	safeInsert("ColorCorrectionEffect", {
		Brightness = 0.1,
		Contrast = 0.15,
		Saturation = 0.25,
		TintColor = Color3.fromRGB(255, 240, 210)
	})

	safeInsert("DepthOfFieldEffect", {
		FarIntensity = 0.3,
		FocusDistance = 25,
		InFocusRadius = 25,
		NearIntensity = 0.3
	})

	safeInsert("SunRaysEffect", {
		Intensity = 0.2,
		Spread = 0.75
	})
end

-- 🔁 Loop que observa se o objeto shader reaparece
local function monitorShaderTarget()
	local lastTarget = nil

	RunService.RenderStepped:Connect(function()
		ensureLightingEffects()

		local currentTarget = workspace:FindFirstChild(SHADER_OBJECT_NAME)
		if currentTarget and currentTarget ~= lastTarget then
			applyShaderToPart(currentTarget)
			lastTarget = currentTarget
		end
	end)
end

-- 🧬 Inicializa no load do personagem e cenário
local function initialize()
	ensureLightingEffects()
	monitorShaderTarget()
end

-- 👤 Espera o personagem do jogador carregar
if player.Character then
	initialize()
else
	player.CharacterAdded:Connect(function()
		initialize()
	end)
end

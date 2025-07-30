local Vignette = true -- Ative/desative a vinheta
local Lighting = game:GetService("Lighting")
local Players = game:GetService("Players")
local player = Players.LocalPlayer

-- Função para aplicar os efeitos personalizados
local function applyEffects()
	-- Remover apenas os efeitos criados por este script
	for _, v in pairs(Lighting:GetChildren()) do
		if v.Name:match("^Custom") then
			v:Destroy()
		end
	end

	-- Bloom
	local Bloom = Instance.new("BloomEffect")
	Bloom.Intensity = 0.3
	Bloom.Size = 10
	Bloom.Threshold = 0.8
	Bloom.Name = "CustomBloom"
	Bloom.Parent = Lighting

	-- Blur
	local Blur = Instance.new("BlurEffect")
	Blur.Size = 5
	Blur.Name = "CustomBlur"
	Blur.Parent = Lighting

	-- Correção de Cor
	local ColorCor = Instance.new("ColorCorrectionEffect")
	ColorCor.Brightness = 0.1
	ColorCor.Contrast = 0.5
	ColorCor.Saturation = -0.3
	ColorCor.TintColor = Color3.fromRGB(255, 235, 203)
	ColorCor.Name = "CustomColorCorrection"
	ColorCor.Parent = Lighting

	-- Sun Rays
	local SunRays = Instance.new("SunRaysEffect")
	SunRays.Intensity = 0.075
	SunRays.Spread = 0.727
	SunRays.Name = "CustomSunRays"
	SunRays.Parent = Lighting

	-- Céu
	local Sky = Instance.new("Sky")
	Sky.Name = "CustomSky"
	Sky.SkyboxBk = "rbxassetid://151165214"
	Sky.SkyboxDn = "rbxassetid://151165197"
	Sky.SkyboxFt = "rbxassetid://151165224"
	Sky.SkyboxLf = "rbxassetid://151165191"
	Sky.SkyboxRt = "rbxassetid://151165206"
	Sky.SkyboxUp = "rbxassetid://151165227"
	Sky.SunAngularSize = 10
	Sky.Parent = Lighting

	-- Atmosfera
	local Atm = Instance.new("Atmosphere")
	Atm.Name = "CustomAtmosphere"
	Atm.Density = 0.364
	Atm.Offset = 0.556
	Atm.Color = Color3.fromRGB(199, 175, 166)
	Atm.Decay = Color3.fromRGB(44, 39, 33)
	Atm.Glare = 0.36
	Atm.Haze = 1.72
	Atm.Parent = Lighting

	-- Configurações do Lighting
	Lighting.Ambient = Color3.fromRGB(2, 2, 2)
	Lighting.Brightness = 2.25
	Lighting.ColorShift_Bottom = Color3.fromRGB(0, 0, 0)
	Lighting.ColorShift_Top = Color3.fromRGB(0, 0, 0)
	Lighting.EnvironmentDiffuseScale = 0.2
	Lighting.EnvironmentSpecularScale = 0.2
	Lighting.GlobalShadows = true
	Lighting.OutdoorAmbient = Color3.fromRGB(0, 0, 0)
	Lighting.ShadowSoftness = 0.2
	Lighting.ClockTime = 17
	Lighting.GeographicLatitude = 45
	Lighting.ExposureCompensation = 0.5
	Lighting.Technology = Enum.Technology.ShadowMap
end

-- Função para criar vinheta
local function createVignette()
	if player:FindFirstChild("PlayerGui") and not player.PlayerGui:FindFirstChild("ShaderVignette") then
		local Gui = Instance.new("ScreenGui")
		Gui.Name = "ShaderVignette"
		Gui.ResetOnSpawn = false
		Gui.IgnoreGuiInset = true
		Gui.Parent = player.PlayerGui

		local ShadowFrame = Instance.new("ImageLabel")
		ShadowFrame.Name = "ShadowVignette"
		ShadowFrame.AnchorPoint = Vector2.new(0.5, 1)
		ShadowFrame.Position = UDim2.new(0.5, 0, 1, 0)
		ShadowFrame.Size = UDim2.new(1, 0, 1.05, 0)
		ShadowFrame.BackgroundTransparency = 1
		ShadowFrame.Image = "rbxassetid://4576475446"
		ShadowFrame.ImageTransparency = 0.3
		ShadowFrame.ZIndex = 10
		ShadowFrame.Parent = Gui
	end
end

-- Aplicar efeitos inicialmente
applyEffects()
if Vignette then
	createVignette()
end

-- Monitorar a cada 10 segundos e reaplicar se necessário
task.spawn(function()
	while true do
		task.wait(10)
		if not Lighting:FindFirstChild("CustomBloom") then
			applyEffects()
		end
		if Vignette and player and player:FindFirstChild("PlayerGui") and not player.PlayerGui:FindFirstChild("ShaderVignette") then
			createVignette()
		end
	end
end)

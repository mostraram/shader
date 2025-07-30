local Vignette = true -- Altere para false se não quiser a moldura escura

local Players = game:GetService("Players")
local Lighting = game:GetService("Lighting")
local StarterGui = game:GetService("StarterGui")
local player = Players.LocalPlayer

local function applyShader()
    -- Limpa apenas os efeitos criados por você, se necessário
    for _, v in pairs(Lighting:GetChildren()) do
        if v:IsA("PostEffect") or v:IsA("Sky") or v:IsA("Atmosphere") then
            v:Destroy()
        end
    end

    local Bloom = Instance.new("BloomEffect")
    Bloom.Intensity = 0.3
    Bloom.Size = 10
    Bloom.Threshold = 0.8
    Bloom.Parent = Lighting

    local Blur = Instance.new("BlurEffect")
    Blur.Size = 5
    Blur.Parent = Lighting

    local ColorCor = Instance.new("ColorCorrectionEffect")
    ColorCor.Brightness = 0.1
    ColorCor.Contrast = 0.5
    ColorCor.Saturation = -0.3
    ColorCor.TintColor = Color3.fromRGB(255, 235, 203)
    ColorCor.Parent = Lighting

    local SunRays = Instance.new("SunRaysEffect")
    SunRays.Intensity = 0.075
    SunRays.Spread = 0.727
    SunRays.Parent = Lighting

    local Sky = Instance.new("Sky")
    Sky.SkyboxBk = "http://www.roblox.com/asset/?id=151165214"
    Sky.SkyboxDn = "http://www.roblox.com/asset/?id=151165197"
    Sky.SkyboxFt = "http://www.roblox.com/asset/?id=151165224"
    Sky.SkyboxLf = "http://www.roblox.com/asset/?id=151165191"
    Sky.SkyboxRt = "http://www.roblox.com/asset/?id=151165206"
    Sky.SkyboxUp = "http://www.roblox.com/asset/?id=151165227"
    Sky.SunAngularSize = 10
    Sky.Parent = Lighting

    local Atm = Instance.new("Atmosphere")
    Atm.Density = 0.364
    Atm.Offset = 0.556
    Atm.Color = Color3.fromRGB(199, 175, 166)
    Atm.Decay = Color3.fromRGB(44, 39, 33)
    Atm.Glare = 0.36
    Atm.Haze = 1.72
    Atm.Parent = Lighting

    Lighting.Ambient = Color3.fromRGB(2,2,2)
    Lighting.Brightness = 2.25
    Lighting.ColorShift_Bottom = Color3.fromRGB(0,0,0)
    Lighting.ColorShift_Top = Color3.fromRGB(0,0,0)
    Lighting.EnvironmentDiffuseScale = 0.2
    Lighting.EnvironmentSpecularScale = 0.2
    Lighting.GlobalShadows = true
    Lighting.OutdoorAmbient = Color3.fromRGB(0,0,0)
    Lighting.ShadowSoftness = 0.2
    Lighting.ClockTime = 17
    Lighting.GeographicLatitude = 45
    Lighting.ExposureCompensation = 0.5

    -- Aplica a moldura (vignette) corretamente
    if Vignette and player and player:FindFirstChild("PlayerGui") then
        local existingGui = player.PlayerGui:FindFirstChild("ShaderVignette")
        if existingGui then existingGui:Destroy() end

        local Gui = Instance.new("ScreenGui")
        Gui.Name = "ShaderVignette"
        Gui.IgnoreGuiInset = true
        Gui.ResetOnSpawn = false
        Gui.Parent = player.PlayerGui

        local ShadowFrame = Instance.new("ImageLabel")
        ShadowFrame.Parent = Gui
        ShadowFrame.AnchorPoint = Vector2.new(0.5,1)
        ShadowFrame.Position = UDim2.new(0.5,0,1,0)
        ShadowFrame.Size = UDim2.new(1,0,1.05,0)
        ShadowFrame.BackgroundTransparency = 1
        ShadowFrame.Image = "rbxassetid://4576475446"
        ShadowFrame.ImageTransparency = 0.3
        ShadowFrame.ZIndex = 10
    end
end

-- Aplica ao entrar no jogo
applyShader()

-- Garante que será reaplicado ao mudar de ambiente/casa
player.CharacterAdded:Connect(function()
    task.wait(1.5) -- pequeno delay para garantir que a casa carregou
    applyShader()
end)

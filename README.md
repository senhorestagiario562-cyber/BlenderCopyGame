-- Carregando a Biblioteca Visual (Orion Lib)
local OrionLib = loadstring(game:HttpGet(('https://raw.githubusercontent.com/shlexware/Orion/main/source')))()

local Window = OrionLib:MakeWindow({
    Name = "HUB Completo - Delta", 
    HidePremium = true, 
    SaveConfig = false, 
    ConfigFolder = "DeltaHubConfig",
    IntroText = "Carregando HUB..."
})

-- VARIÁVEIS DE CONFIGURAÇÃO
local Config = {
    PlayerESP = false,
    ESPColor = Color3.fromRGB(0, 255, 128),
    
    NametagESP = false,
    NametagColor = Color3.fromRGB(255, 255, 255),
    
    LineESP = false,
    
    HitboxAtiva = false,
    HitboxTamanho = 2,
    MostrarHitbox = false,
    HitboxCor = Color3.fromRGB(255, 0, 0)
}

local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local Camera = workspace.CurrentCamera

--------------------------------------------------------------------------------
-- 1. CATEGORIA: ESP
--------------------------------------------------------------------------------
local TabESP = Window:MakeTab({
    Name = "ESP",
    Icon = "rbxassetid://4483345998",
    PremiumOnly = false
})

TabESP:AddToggle({
    Name = "Player ESP (Highlight)",
    Default = false,
    Callback = function(Value)
        Config.PlayerESP = Value
    end    
})

TabESP:AddColorpicker({
    Name = "Mudar Cor do Player ESP",
    Default = Color3.fromRGB(0, 255, 128),
    Callback = function(Value)
        Config.ESPColor = Value
    end
})

TabESP:AddToggle({
    Name = "Nametag ESP",
    Default = false,
    Callback = function(Value)
        Config.NametagESP = Value
    end    
})

TabESP:AddColorpicker({
    Name = "Mudar Cor da Nametag",
    Default = Color3.fromRGB(255, 255, 255),
    Callback = function(Value)
        Config.NametagColor = Value
    end
})

TabESP:AddToggle({
    Name = "Line ESP (Linhas na Tela)",
    Default = false,
    Callback = function(Value)
        Config.LineESP = Value
    end    
})

--------------------------------------------------------------------------------
-- 2. CATEGORIA: HITBOX
--------------------------------------------------------------------------------
local TabHitbox = Window:MakeTab({
    Name = "Hitbox",
    Icon = "rbxassetid://4483345998",
    PremiumOnly = false
})

TabHitbox:AddToggle({
    Name = "Ativar Hitbox",
    Default = false,
    Callback = function(Value)
        Config.HitboxAtiva = Value
    end    
})

TabHitbox:AddSlider({
    Name = "Aumentar Tamanho da Hitbox",
    Min = 1,
    Max = 5,
    Default = 2,
    Color = Color3.fromRGB(255, 255, 255),
    Increment = 1,
    ValueName = "x",
    Callback = function(Value)
        Config.HitboxTamanho = Value
    end    
})

TabHitbox:AddToggle({
    Name = "Mostrar Hitbox (Visível)",
    Default = false,
    Callback = function(Value)
        Config.MostrarHitbox = Value
    end    
})

TabHitbox:AddColorpicker({
    Name = "Mudar Cor do Hitbox",
    Default = Color3.fromRGB(255, 0, 0),
    Callback = function(Value)
        Config.HitboxCor = Value
    end
})

--------------------------------------------------------------------------------
-- 3. CATEGORIA: PAINEL HD-ADMIN
--------------------------------------------------------------------------------
local TabAdmin = Window:MakeTab({
    Name = "HD Admin",
    Icon = "rbxassetid://4483345998",
    PremiumOnly = false
})

TabAdmin:AddButton({
    Name = "Criar Painel Separado HD-Admin",
    Callback = function()
        -- Criação da Janela Flutuante Independente
        local CoreGui = game:GetService("CoreGui")
        local ScreenGui = Instance.new("ScreenGui")
        ScreenGui.Name = "HDAdminFloatingPanel"
        ScreenGui.Parent = CoreGui

        local Frame = Instance.new("Frame")
        Frame.Size = UDim2.new(0, 220, 0, 260)
        Frame.Position = UDim2.new(0.5, -110, 0.3, 0)
        Frame.BackgroundColor3 = Color3.fromRGB(20, 20, 25)
        Frame.Active = true
        Frame.Draggable = true
        Frame.Parent = ScreenGui

        local Title = Instance.new("TextLabel")
        Title.Size = UDim2.new(1, 0, 0, 30)
        Title.BackgroundColor3 = Color3.fromRGB(180, 40, 40)
        Title.Text = "PAINEL HD-ADMIN"
        Title.TextColor3 = Color3.fromRGB(255, 255, 255)
        Title.Font = Enum.Font.SourceSansBold
        Title.TextSize = 14
        Title.Parent = Frame

        local CloseBtn = Instance.new("TextButton")
        CloseBtn.Size = UDim2.new(0, 25, 0, 25)
        CloseBtn.Position = UDim2.new(1, -27, 0, 2)
        CloseBtn.Text = "X"
        CloseBtn.BackgroundColor3 = Color3.fromRGB(255, 50, 50)
        CloseBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
        CloseBtn.Parent = Title
        CloseBtn.MouseButton1Click:Connect(function() ScreenGui:Destroy() end)

        local Scroll = Instance.new("ScrollingFrame")
        Scroll.Size = UDim2.new(1, -10, 1, -40)
        Scroll.Position = UDim2.new(0, 5, 0, 35)
        Scroll.BackgroundTransparency = 1
        Scroll.Parent = Frame

        local ComandosHD = {
            ";fly", ";unfly", ";god", ";ungod", ";noclip", ";clip",
            ";invisible", ";visible", ";speed 50", ";jump 100",
            ";kill", ";respawn", ";bring", ";goto", ";btools", ";ff", ";unff"
        }

        Scroll.CanvasSize = UDim2.new(0, 0, 0, #ComandosHD * 28)

        for i, cmd in ipairs(ComandosHD) do
            local BtnCmd = Instance.new("TextButton")
            BtnCmd.Size = UDim2.new(1, -10, 0, 25)
            BtnCmd.Position = UDim2.new(0, 0, 0, (i - 1) * 28)
            BtnCmd.BackgroundColor3 = Color3.fromRGB(40, 40, 50)
            BtnCmd.Text = cmd
            BtnCmd.TextColor3 = Color3.fromRGB(255, 255, 255)
            BtnCmd.Font = Enum.Font.SourceSans
            BtnCmd.TextSize = 13
            BtnCmd.Parent = Scroll

            BtnCmd.MouseButton1Click:Connect(function()
                local TextChatService = game:GetService("TextChatService")
                if TextChatService.ChatVersion == Enum.ChatVersion.TextChatService then
                    local channel = TextChatService.TextChannels.RBXGeneral
                    if channel then channel:SendAsync(cmd) end
                else
                    game:GetService("ReplicatedStorage").DefaultChatSystemChatEvents.SayMessageRequest:FireServer(cmd, "All")
                end
            end)
        end
    end
})

--------------------------------------------------------------------------------
-- 4. LOOP DAS FUNCIONALIDADES (ESP & HITBOX)
--------------------------------------------------------------------------------
local LinesTable = {}

game:GetService("RunService").RenderStepped:Connect(function()
    for _, p in ipairs(Players:GetPlayers()) do
        if p ~= LocalPlayer and p.Character and p.Character:FindFirstChild("HumanoidRootPart") then
            local char = p.Character
            local hrp = char.HumanoidRootPart

            -- 1. Player ESP (Highlight)
            local hl = char:FindFirstChild("Delta_Highlight")
            if Config.PlayerESP then
                if not hl then
                    hl = Instance.new("Highlight")
                    hl.Name = "Delta_Highlight"
                    hl.DepthMode = Enum.HighlightDepthMode.AlwaysOnTop
                    hl.Parent = char
                end
                hl.FillColor = Config.ESPColor
            else
                if hl then hl:Destroy() end
            end

            -- 2. Nametag ESP
            local head = char:FindFirstChild("Head")
            if head then
                local tag = head:FindFirstChild("Delta_Nametag")
                if Config.NametagESP then
                    if not tag then
                        tag = Instance.new("BillboardGui")
                        tag.Name = "Delta_Nametag"
                        tag.Size = UDim2.new(0, 100, 0, 30)
                        tag.StudsOffset = Vector3.new(0, 2, 0)
                        tag.AlwaysOnTop = true
                        
                        local txt = Instance.new("TextLabel", tag)
                        txt.Name = "Texto"
                        txt.Size = UDim2.new(1, 0, 1, 0)
                        txt.BackgroundTransparency = 1
                        txt.Font = Enum.Font.SourceSansBold
                        txt.TextSize = 14
                        tag.Parent = head
                    end
                    tag.Texto.Text = p.Name
                    tag.Texto.TextColor3 = Config.NametagColor
                else
                    if tag then tag:Destroy() end
                end
            end

            -- 3. Hitbox
            if Config.HitboxAtiva then
                local mult = Config.HitboxTamanho
                hrp.Size = Vector3.new(2 * mult, 2 * mult, 1 * mult)
                hrp.Transparency = Config.MostrarHitbox and 0.5 or 1
                hrp.Color = Config.HitboxCor
                hrp.Material = Enum.Material.ForceField
                hrp.CanCollide = false
            else
                hrp.Size = Vector3.new(2, 2, 1)
                hrp.Transparency = 1
            end
        end
    end
end)

OrionLib:Init()

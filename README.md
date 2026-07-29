-- ========================================================
-- ADVANCED MULTI-TOOL SCRIPT HUB
-- ========================================================

local HttpService = game:GetService("HttpService")
local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer

-- Instância da ScreenGui principal
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "AdvancedHubGUI"
ScreenGui.Parent = game:GetService("CoreGui") or LocalPlayer:WaitForChild("PlayerGui")
ScreenGui.ResetOnSpawn = false

-- Frame Principal
local MainFrame = Instance.new("Frame")
MainFrame.Name = "MainFrame"
MainFrame.Parent = ScreenGui
MainFrame.BackgroundColor3 = Color3.fromRGB(25, 25, 30)
MainFrame.Position = UDim2.new(0.25, 0, 0.2, 0)
MainFrame.Size = UDim2.new(0, 580, 0, 360)
MainFrame.Active = true
MainFrame.Draggable = true

local MainCorner = Instance.new("UICorner")
MainCorner.CornerRadius = UDim.new(0, 10)
MainCorner.Parent = MainFrame

-- Barra Superior (Título e Botões)
local TopBar = Instance.new("Frame")
TopBar.Name = "TopBar"
TopBar.Parent = MainFrame
TopBar.BackgroundColor3 = Color3.fromRGB(35, 35, 42)
TopBar.Size = UDim2.new(1, 0, 0, 35)

local TopBarCorner = Instance.new("UICorner")
TopBarCorner.CornerRadius = UDim.new(0, 10)
TopBarCorner.Parent = TopBar

local Title = Instance.new("TextLabel")
Title.Parent = TopBar
Title.BackgroundTransparency = 1
Title.Position = UDim2.new(0, 12, 0, 0)
Title.Size = UDim2.new(0, 200, 1, 0)
Title.Font = Enum.Font.SourceSansBold
Title.Text = "ADVANCED HUB | AI & UTILITIES"
Title.TextColor3 = Color3.fromRGB(255, 255, 255)
Title.TextSize = 15
Title.TextXAlignment = Enum.TextXAlignment.Left

-- Botão Fechar
CloseBtn = Instance.new("TextButton")
CloseBtn.Parent = TopBar
CloseBtn.BackgroundColor3 = Color3.fromRGB(230, 60, 60)
CloseBtn.Position = UDim2.new(1, -28, 0.15, 0)
CloseBtn.Size = UDim2.new(0, 22, 0, 22)
CloseBtn.Font = Enum.Font.SourceSansBold
CloseBtn.Text = "X"
CloseBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
CloseBtn.TextSize = 13
local CloseCorner = Instance.new("UICorner")
CloseCorner.CornerRadius = UDim.new(0, 4)
CloseCorner.Parent = CloseBtn

-- Botão Minimizar
MinimizeBtn = Instance.new("TextButton")
MinimizeBtn.Parent = TopBar
MinimizeBtn.BackgroundColor3 = Color3.fromRGB(60, 60, 70)
MinimizeBtn.Position = UDim2.new(1, -55, 0.15, 0)
MinimizeBtn.Size = UDim2.new(0, 22, 0, 22)
MinimizeBtn.Font = Enum.Font.SourceSansBold
MinimizeBtn.Text = "-"
MinimizeBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
MinimizeBtn.TextSize = 16
local MinCorner = Instance.new("UICorner")
MinCorner.CornerRadius = UDim.new(0, 4)
MinCorner.Parent = MinimizeBtn

-- Botão Flutuante (Reabrir)
local OpenBtn = Instance.new("TextButton")
OpenBtn.Name = "OpenHubBtn"
OpenBtn.Parent = ScreenGui
OpenBtn.BackgroundColor3 = Color3.fromRGB(40, 40, 50)
OpenBtn.Position = UDim2.new(0, 15, 0.5, 0)
OpenBtn.Size = UDim2.new(0, 85, 0, 32)
OpenBtn.Font = Enum.Font.SourceSansBold
OpenBtn.Text = "Open Hub"
OpenBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
OpenBtn.TextSize = 14
OpenBtn.Visible = false
OpenBtn.Active = true
OpenBtn.Draggable = true
local OpenCorner = Instance.new("UICorner")
OpenCorner.CornerRadius = UDim.new(0, 6)
OpenCorner.Parent = OpenBtn

-- Sidebar (Categorias)
local Sidebar = Instance.new("Frame")
Sidebar.Parent = MainFrame
Sidebar.BackgroundColor3 = Color3.fromRGB(18, 18, 22)
Sidebar.Position = UDim2.new(0, 0, 0, 35)
Sidebar.Size = UDim2.new(0, 140, 1, -35)

local SidebarList = Instance.new("UIListLayout")
SidebarList.Parent = Sidebar
SidebarList.SortOrder = Enum.SortOrder.LayoutOrder
SidebarList.Padding = UDim.new(0, 4)

-- Container principal para os conteúdos
local Container = Instance.new("Frame")
Container.Parent = MainFrame
Container.BackgroundTransparency = 1
Container.Position = UDim2.new(0, 145, 0, 40)
Container.Size = UDim2.new(1, -150, 1, -45)

----------------------------------------------------
-- SISTEMA DE ABAS E CRIAÇÃO DE UI
----------------------------------------------------
local Pages = {}

local function CreateTab(name)
    local TabBtn = Instance.new("TextButton")
    TabBtn.Parent = Sidebar
    TabBtn.Size = UDim2.new(1, 0, 0, 35)
    TabBtn.BackgroundColor3 = Color3.fromRGB(28, 28, 35)
    TabBtn.Font = Enum.Font.SourceSans
    TabBtn.Text = name
    TabBtn.TextColor3 = Color3.fromRGB(180, 180, 190)
    TabBtn.TextSize = 14

    local PageFrame = Instance.new("ScrollingFrame")
    PageFrame.Parent = Container
    PageFrame.Size = UDim2.new(1, 0, 1, 0)
    PageFrame.BackgroundTransparency = 1
    PageFrame.Visible = false
    PageFrame.ScrollBarThickness = 4
    PageFrame.CanvasSize = UDim2.new(0, 0, 0, 0)
    PageFrame.AutomaticCanvasSize = Enum.AutomaticSize.Y

    local PageList = Instance.new("UIListLayout")
    PageList.Parent = PageFrame
    PageList.SortOrder = Enum.SortOrder.LayoutOrder
    PageList.Padding = UDim.new(0, 8)

    Pages[name] = {Frame = PageFrame, Button = TabBtn}

    TabBtn.MouseButton1Click:Connect(function()
        for _, p in pairs(Pages) do
            p.Frame.Visible = false
            p.Button.BackgroundColor3 = Color3.fromRGB(28, 28, 35)
            p.Button.TextColor3 = Color3.fromRGB(180, 180, 190)
        end
        PageFrame.Visible = true
        TabBtn.BackgroundColor3 = Color3.fromRGB(50, 90, 180)
        TabBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
    end)

    return PageFrame
end

local function CreateButton(parent, text, color, callback)
    local Btn = Instance.new("TextButton")
    Btn.Parent = parent
    Btn.Size = UDim2.new(1, -10, 0, 32)
    Btn.BackgroundColor3 = color or Color3.fromRGB(45, 45, 55)
    Btn.Font = Enum.Font.SourceSansBold
    Btn.Text = text
    Btn.TextColor3 = Color3.fromRGB(255, 255, 255)
    Btn.TextSize = 14
    
    local Corner = Instance.new("UICorner")
    Corner.CornerRadius = UDim.new(0, 5)
    Corner.Parent = Btn

    Btn.MouseButton1Click:Connect(callback)
    return Btn
end

local function CreateTextBox(parent, placeholder)
    local Box = Instance.new("TextBox")
    Box.Parent = parent
    Box.Size = UDim2.new(1, -10, 0, 32)
    Box.BackgroundColor3 = Color3.fromRGB(20, 20, 25)
    Box.Font = Enum.Font.SourceSans
    Box.PlaceholderText = placeholder
    Box.Text = ""
    Box.TextColor3 = Color3.fromRGB(255, 255, 255)
    Box.TextSize = 14

    local Corner = Instance.new("UICorner")
    Corner.CornerRadius = UDim.new(0, 5)
    Corner.Parent = Box

    return Box
end

----------------------------------------------------
-- 1. SISTEMA: COPY WORKSPACE
----------------------------------------------------
local CopyPage = CreateTab("Copy Workspace")

CreateButton(CopyPage, "Salvar Mapa Completo (saveinstance)", Color3.fromRGB(40, 120, 60), function()
    if saveinstance then
        print("[Hub] Copiando e salvando Workspace...")
        saveinstance()
    else
        warn("[Hub] Seu executor não possui suporte para 'saveinstance'")
    end
end)

CreateButton(CopyPage, "Copiar Estrutura p/ Clipboard", Color3.fromRGB(50, 90, 180), function()
    if setclipboard then
        local data = "Workspace Children:\n"
        for _, v in pairs(game.Workspace:GetChildren()) do
            data = data .. "- " .. v.Name .. " [" .. v.ClassName .. "]\n"
        end
        setclipboard(data)
        print("[Hub] Nomes salvos na área de transferência!")
    end
end)

CreateButton(CopyPage, "Clonar Objetos Selecionados para ReplicatedStorage", Color3.fromRGB(70, 70, 90), function()
    for _, v in pairs(game.Workspace:GetChildren()) do
        if v:IsA("Model") or v:IsA("Part") then
            pcall(function()
                local clone = v:Clone()
                if clone then clone.Parent = game:GetService("ReplicatedStorage") end
            end)
        end
    end
    print("[Hub] Clones enviados para ReplicatedStorage!")
end)

----------------------------------------------------
-- 2. SISTEMA: ASSISTENTE COM IA
----------------------------------------------------
local AIPage = CreateTab("Sistema com IA")

local AIInput = CreateTextBox(AIPage, "Digite sua pergunta ou pedido de script...")
local AIResponse = Instance.new("TextLabel")
AIResponse.Parent = AIPage
AIResponse.Size = UDim2.new(1, -10, 0, 120)
AIResponse.BackgroundColor3 = Color3.fromRGB(18, 18, 22)
AIResponse.Font = Enum.Font.SourceSans
AIResponse.Text = "Aguardando pergunta..."
AIResponse.TextColor3 = Color3.fromRGB(200, 200, 200)
AIResponse.TextSize = 13
AIResponse.TextWrapped = true
AIResponse.TextYAlignment = Enum.TextYAlignment.Top

local AICorner = Instance.new("UICorner")
AICorner.CornerRadius = UDim.new(0, 5)
AICorner.Parent = AIResponse

CreateButton(AIPage, "Perguntar à IA", Color3.fromRGB(100, 60, 180), function()
    local prompt = AIInput.Text
    if prompt == "" then return end
    AIResponse.Text = "Pensando..."

    task.spawn(function()
        local success, res = pcall(function()
            local request = (syn and syn.request) or (http and http.request) or http_request or request
            if request then
                local response = request({
                    Url = "https://text.pollinations.ai/" .. HttpService:UrlEncode("Responda em portugues e de forma direta para Roblox Lua: " .. prompt),
                    Method = "GET"
                })
                return response.Body
            end
        end)

        if success and res then
            AIResponse.Text = res
        else
            AIResponse.Text = "Erro ao conectar com a API de IA. Verifique se seu executor suporta requisições HTTP."
        end
    end)
end)

CreateButton(AIPage, "Copiar Resposta da IA", Color3.fromRGB(60, 60, 70), function()
    if setclipboard and AIResponse.Text ~= "" then
        setclipboard(AIResponse.Text)
        print("[Hub] Resposta copiada!")
    end
end)

----------------------------------------------------
-- 3. SISTEMA: GERENCIADOR DE ANIMAÇÕES
----------------------------------------------------
local AnimPage = CreateTab("Sistema de Animação")

local AnimInput = CreateTextBox(AnimPage, "Insira o Asset ID da Animação (ex: 1234567)")
local SpeedInput = CreateTextBox(AnimPage, "Velocidade da Animação (Padrão: 1)")

local currentTrack = nil

CreateButton(AnimPage, "Tocar Animação (Play)", Color3.fromRGB(40, 140, 70), function()
    local animId = AnimInput.Text
    local speed = tonumber(SpeedInput.Text) or 1
    
    if animId == "" then return end
    
    local char = LocalPlayer.Character
    if char and char:FindFirstChild("Humanoid") then
        local humanoid = char.Humanoid
        local animation = Instance.new("Animation")
        animation.AnimationId = "rbxassetid://" .. animId
        
        if currentTrack then currentTrack:Stop() end
        
        currentTrack = humanoid:LoadAnimation(animation)
        currentTrack:Play()
        currentTrack:AdjustSpeed(speed)
        print("[Hub] Reproduzindo animação: " .. animId)
    end
end)

CreateButton(AnimPage, "Pausar / Retomar", Color3.fromRGB(180, 120, 40), function()
    if currentTrack then
        if currentTrack.IsPlaying then
            currentTrack:AdjustSpeed(0)
        else
            local speed = tonumber(SpeedInput.Text) or 1
            currentTrack:AdjustSpeed(speed)
        end
    end
end)

CreateButton(AnimPage, "Parar Animação (Stop)", Color3.fromRGB(180, 50, 50), function()
    if currentTrack then
        currentTrack:Stop()
        print("[Hub] Animação interrompida.")
    end
end)

----------------------------------------------------
-- 4. SISTEMA: COPY GUI
----------------------------------------------------
local CopyGuiPage = CreateTab("Copy GUI")

local TargetPlayerInput = CreateTextBox(CopyGuiPage, "Nome exato do Jogador (Alvo)")

CreateButton(CopyGuiPage, "Copiar PlayerGui do Jogador Alvo", Color3.fromRGB(160, 80, 40), function()
    local targetName = TargetPlayerInput.Text
    local targetPlayer = Players:FindFirstChild(targetName)

    if targetPlayer and targetPlayer:FindFirstChild("PlayerGui") then
        for _, gui in pairs(targetPlayer.PlayerGui:GetChildren()) do
            pcall(function()
                local clonedGui = gui:Clone()
                clonedGui.Parent = LocalPlayer:WaitForChild("PlayerGui")
            end)
        end
        print("[Hub] Interface do jogador " .. targetName .. " clonada com sucesso!")
    else
        warn("[Hub] Jogador não encontrado ou PlayerGui inacessível.")
    end
end)

CreateButton(CopyGuiPage, "Exportar Código de todas Guis Ativas", Color3.fromRGB(60, 90, 150), function()
    if setclipboard then
        local guiNames = "Guis Encontradas na PlayerGui:\n"
        for _, v in pairs(LocalPlayer.PlayerGui:GetChildren()) do
            guiNames = guiNames .. "- " .. v.Name .. " [" .. v.ClassName .. "]\n"
        end
        setclipboard(guiNames)
        print("[Hub] Nomes das GUIs salvos no clipboard!")
    end
end)

----------------------------------------------------
-- GERENCIAMENTO DE JANELA (Minimizar / Fechar)
----------------------------------------------------
Pages["Copy Workspace"].Frame.Visible = true
Pages["Copy Workspace"].Button.BackgroundColor3 = Color3.fromRGB(50, 90, 180)
Pages["Copy Workspace"].Button.TextColor3 = Color3.fromRGB(255, 255, 255)

MinimizeBtn.MouseButton1Click:Connect(function()
    MainFrame.Visible = false
    OpenBtn.Visible = true
end)

OpenBtn.MouseButton1Click:Connect(function()
    MainFrame.Visible = true
    OpenBtn.Visible = false
end)

CloseBtn.MouseButton1Click:Connect(function()
    ScreenGui:Destroy()
end)

-- Criaaando a Interface Principal (ScreenGui)
local CoreGui = game:GetService("CoreGui")
local UserInputService = game:GetService("UserInputService")
local SoundService = game:GetService("SoundService")

-- Criando o som de clique global para reutilizar nos botões
local ClickSound = Instance.new("Sound")
ClickSound.Name = "ClickSound"
ClickSound.SoundId = "rbxassetid://140207837688369"
ClickSound.Volume = 1
ClickSound.Parent = SoundService

local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "BlenderStudioHub"
ScreenGui.Parent = CoreGui
ScreenGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling

-- Fundo do Painel (Main Frame - Tamanho Aumentado)
local MainFrame = Instance.new("Frame")
MainFrame.Name = "MainFrame"
MainFrame.Size = UDim2.new(0, 620, 0, 380)
MainFrame.Position = UDim2.new(0.5, -310, 0.5, -190)
MainFrame.BackgroundColor3 = Color3.fromRGB(20, 20, 20) -- Preto Escuro
MainFrame.BorderSizePixel = 0
MainFrame.ClipsDescendants = true
MainFrame.Parent = ScreenGui

local MainCorner = Instance.new("UICorner")
MainCorner.CornerRadius = UDim.new(0, 10)
MainCorner.Parent = MainFrame

local MainStroke = Instance.new("UIStroke")
MainStroke.Color = Color3.fromRGB(255, 120, 0) -- Borda Laranja
MainStroke.Thickness = 1.5
MainStroke.Parent = MainFrame

-- Imagem de Fundo Transparente
local BackgroundImage = Instance.new("ImageLabel")
BackgroundImage.Name = "BackgroundImage"
BackgroundImage.Size = UDim2.new(1, 0, 1, 0)
BackgroundImage.Position = UDim2.new(0, 0, 0, 0)
BackgroundImage.BackgroundTransparency = 1
BackgroundImage.Image = "rbxassetid://111837850012187"
BackgroundImage.ImageTransparency = 0.5
BackgroundImage.ScaleType = Enum.ScaleType.Crop
BackgroundImage.ZIndex = 1
BackgroundImage.Parent = MainFrame

-- Barra Superior (Header - Área onde clica para arrastar)
local Header = Instance.new("Frame")
Header.Name = "Header"
Header.Size = UDim2.new(1, 0, 0, 40)
Header.BackgroundColor3 = Color3.fromRGB(15, 15, 15)
Header.BackgroundTransparency = 0.2
Header.BorderSizePixel = 0
Header.ZIndex = 2
Header.Parent = MainFrame

local HeaderCorner = Instance.new("UICorner")
HeaderCorner.CornerRadius = UDim.new(0, 10)
HeaderCorner.Parent = Header

-- Nome no Canto Direito do Cabeçalho
local Title = Instance.new("TextLabel")
Title.Name = "Title"
Title.Size = UDim2.new(0, 200, 1, 0)
Title.Position = UDim2.new(1, -260, 0, 0)
Title.BackgroundTransparency = 1
Title.Text = "BlenderStudio's Lite"
Title.TextColor3 = Color3.fromRGB(255, 120, 0) -- Laranja
Title.TextSize = 16
Title.Font = Enum.Font.GothamBold
Title.TextXAlignment = Enum.TextXAlignment.Right
Title.ZIndex = 3
Title.Parent = Header

-- Botão Fechar (X)
local CloseBtn = Instance.new("TextButton")
CloseBtn.Name = "CloseBtn"
CloseBtn.Size = UDim2.new(0, 30, 0, 30)
CloseBtn.Position = UDim2.new(1, -35, 0, 5)
CloseBtn.BackgroundTransparency = 1
CloseBtn.Text = "✕"
CloseBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
CloseBtn.TextSize = 16
CloseBtn.Font = Enum.Font.GothamBold
CloseBtn.ZIndex = 3
CloseBtn.Parent = Header

CloseBtn.MouseButton1Click:Connect(function()
    ClickSound:Play()
    ScreenGui:Destroy()
end)

-- Botão Minimizar (-)
local MinimizeBtn = Instance.new("TextButton")
MinimizeBtn.Name = "MinimizeBtn"
MinimizeBtn.Size = UDim2.new(0, 30, 0, 30)
MinimizeBtn.Position = UDim2.new(1, -65, 0, 5)
MinimizeBtn.BackgroundTransparency = 1
MinimizeBtn.Text = "—"
MinimizeBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
MinimizeBtn.TextSize = 14
MinimizeBtn.Font = Enum.Font.GothamBold
MinimizeBtn.ZIndex = 3
MinimizeBtn.Parent = Header

local Container = Instance.new("Frame")
Container.Name = "Container"
Container.Size = UDim2.new(1, 0, 1, -40)
Container.Position = UDim2.new(0, 0, 0, 40)
Container.BackgroundTransparency = 1
Container.ZIndex = 2
Container.Parent = MainFrame

local isMinimized = false
MinimizeBtn.MouseButton1Click:Connect(function()
    ClickSound:Play()
    isMinimized = not isMinimized
    if isMinimized then
        MainFrame:TweenSize(UDim2.new(0, 620, 0, 40), Enum.EasingDirection.Out, Enum.EasingStyle.Quart, 0.3, true)
        Container.Visible = false
    else
        MainFrame:TweenSize(UDim2.new(0, 620, 0, 380), Enum.EasingDirection.Out, Enum.EasingStyle.Quart, 0.3, true)
        task.wait(0.15)
        Container.Visible = true
    end
end)

-- Painel Lateral Esquerdo
local Sidebar = Instance.new("Frame")
Sidebar.Name = "Sidebar"
Sidebar.Size = UDim2.new(0, 150, 1, -10)
Sidebar.Position = UDim2.new(0, 5, 0, 0)
Sidebar.BackgroundColor3 = Color3.fromRGB(12, 12, 12)
Sidebar.BackgroundTransparency = 0.3
Sidebar.BorderSizePixel = 0
Sidebar.ZIndex = 2
Sidebar.Parent = Container

local SideCorner = Instance.new("UICorner")
SideCorner.CornerRadius = UDim.new(0, 8)
SideCorner.Parent = Sidebar

local SideLabel = Instance.new("TextLabel")
SideLabel.Size = UDim2.new(1, 0, 0, 30)
SideLabel.Position = UDim2.new(0, 10, 0, 10)
SideLabel.BackgroundTransparency = 1
SideLabel.Text = "⚙ Ferramentas"
SideLabel.TextColor3 = Color3.fromRGB(200, 200, 200)
SideLabel.TextSize = 13
SideLabel.Font = Enum.Font.GothamMedium
SideLabel.TextXAlignment = Enum.TextXAlignment.Left
SideLabel.ZIndex = 3
SideLabel.Parent = Sidebar

-- Área de Conteúdo Direita (Onde ficam os 5 Botões)
local ContentArea = Instance.new("ScrollingFrame")
ContentArea.Name = "ContentArea"
ContentArea.Size = UDim2.new(1, -170, 1, -10)
ContentArea.Position = UDim2.new(0, 160, 0, 0)
ContentArea.BackgroundTransparency = 1
ContentArea.BorderSizePixel = 0
ContentArea.ScrollBarThickness = 3
ContentArea.ScrollBarImageColor3 = Color3.fromRGB(255, 120, 0)
ContentArea.ZIndex = 2
ContentArea.Parent = Container

local UIList = Instance.new("UIListLayout")
UIList.Parent = ContentArea
UIList.SortOrder = Enum.SortOrder.LayoutOrder
UIList.Padding = UDim.new(0, 8)

-- Função para criar os botões com som de clique embutido
local function CreateButton(text, callback)
    local Btn = Instance.new("TextButton")
    Btn.Name = text
    Btn.Size = UDim2.new(1, -10, 0, 48)
    Btn.BackgroundColor3 = Color3.fromRGB(28, 28, 28)
    Btn.BackgroundTransparency = 0.2
    Btn.Text = "  " .. text
    Btn.TextColor3 = Color3.fromRGB(240, 240, 240)
    Btn.Font = Enum.Font.GothamMedium
    Btn.TextSize = 14
    Btn.TextXAlignment = Enum.TextXAlignment.Left
    Btn.AutoButtonColor = false
    Btn.ZIndex = 3
    Btn.Parent = ContentArea

    local BtnCorner = Instance.new("UICorner")
    BtnCorner.CornerRadius = UDim.new(0, 6)
    BtnCorner.Parent = Btn

    local BtnStroke = Instance.new("UIStroke")
    BtnStroke.Color = Color3.fromRGB(45, 45, 45)
    BtnStroke.Thickness = 1
    BtnStroke.Parent = Btn

    -- Efeito Hover
    Btn.MouseEnter:Connect(function()
        Btn.BackgroundColor3 = Color3.fromRGB(38, 38, 38)
        BtnStroke.Color = Color3.fromRGB(255, 120, 0)
    end)

    Btn.MouseLeave:Connect(function()
        Btn.BackgroundColor3 = Color3.fromRGB(28, 28, 28)
        BtnStroke.Color = Color3.fromRGB(45, 45, 45)
    end)

    -- Toca o som e executa a função ao clicar
    Btn.MouseButton1Click:Connect(function()
        ClickSound:Play()
        callback()
    end)
end

-- Lista dos 5 Botões Solicitados
local buttons = {
    "BlenderCopyLite",
    "BlenderAnimatorLite",
    "BlenderIA",
    "BlenderRonlox'Studio",
    "BlenderModeladorLite"
}

for _, name in ipairs(buttons) do
    CreateButton(name, function()
        print("[BlenderStudio] Botão clicado: " .. name)
    end)
end

-- Lógica para Arrastar (Drag System)
local dragging, dragInput, dragStart, startPos

Header.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
        dragging = true
        dragStart = input.Position
        startPos = MainFrame.Position

        input.Changed:Connect(function()
            if input.UserInputState == Enum.UserInputState.End then
                dragging = false
            end
        end)
    end
end)

Header.InputChanged:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch then
        dragInput = input
    end
end)

UserInputService.InputChanged:Connect(function(input)
    if input == dragInput and dragging then
        local delta = input.Position - dragStart
        MainFrame.Position = UDim2.new(
            startPos.X.Scale, 
            startPos.X.Offset + delta.X, 
            startPos.Y.Scale, 
            startPos.Y.Offset + delta.Y
        )
    end
end)

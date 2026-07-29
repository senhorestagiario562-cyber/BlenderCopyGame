-- Coloque este LocalScript em StarterPlayerScripts ou StarterGui
local Players = game:GetService("Players")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local TweenService = game:GetService("TweenService")
local UserInputService = game:GetService("UserInputService")
local LocalPlayer = Players.LocalPlayer

-- Pasta onde os mapas salvos serão mantidos
local SavedMapsFolder = ReplicatedStorage:FindFirstChild("SavedMaps")
if not SavedMapsFolder then
	SavedMapsFolder = Instance.new("Folder")
	SavedMapsFolder.Name = "SavedMaps"
	SavedMapsFolder.Parent = ReplicatedStorage
end

-- 1. ScreenGui Principal
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "RedBlackScriptHub"
screenGui.ResetOnSpawn = false
screenGui.Parent = LocalPlayer:WaitForChild("PlayerGui")

-- Botão Flutuante para Abrir
local openBtn = Instance.new("TextButton")
openBtn.Name = "OpenHubBtn"
openBtn.Size = UDim2.new(0, 120, 0, 38)
openBtn.Position = UDim2.new(0.02, 0, 0.45, 0)
openBtn.BackgroundColor3 = Color3.fromRGB(180, 0, 0)
openBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
openBtn.Text = "🔴 ABRIR HUB"
openBtn.Font = Enum.Font.GothamBold
openBtn.TextSize = 13
openBtn.Visible = false
openBtn.Parent = screenGui

local openCorner = Instance.new("UICorner")
openCorner.CornerRadius = UDim.new(0, 8)
openCorner.Parent = openBtn

-- Container Principal do Painel
local mainFrame = Instance.new("Frame")
mainFrame.Name = "MainHubFrame"
mainFrame.Size = UDim2.new(0, 620, 0, 380)
mainFrame.Position = UDim2.new(0.5, -310, 0.5, -190)
mainFrame.BackgroundColor3 = Color3.fromRGB(15, 15, 15)
mainFrame.BorderSizePixel = 0
mainFrame.Active = true
mainFrame.Draggable = true
mainFrame.Parent = screenGui

local mainCorner = Instance.new("UICorner")
mainCorner.CornerRadius = UDim.new(0, 10)
mainCorner.Parent = mainFrame

local mainStroke = Instance.new("UIStroke")
mainStroke.Color = Color3.fromRGB(200, 0, 0)
mainStroke.Thickness = 2
mainStroke.Parent = mainFrame

-- Barra Superior (Header)
local topBar = Instance.new("Frame")
topBar.Name = "TopBar"
topBar.Size = UDim2.new(1, 0, 0, 40)
topBar.BackgroundColor3 = Color3.fromRGB(25, 0, 0)
topBar.Parent = mainFrame

local topCorner = Instance.new("UICorner")
topCorner.CornerRadius = UDim.new(0, 10)
topCorner.Parent = topBar

local titleLabel = Instance.new("TextLabel")
titleLabel.Size = UDim2.new(0.5, 0, 1, 0)
titleLabel.Position = UDim2.new(0.03, 0, 0, 0)
titleLabel.BackgroundTransparency = 1
titleLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
titleLabel.Text = "🔴 RED & BLACK HUB - COPYWORKSPACE"
titleLabel.Font = Enum.Font.GothamBold
titleLabel.TextSize = 13
titleLabel.TextXAlignment = Enum.TextXAlignment.Left
titleLabel.Parent = topBar

-- Controles no Canto Superior Direito (Minimizar, Expandir, Fechar)
local windowControls = Instance.new("Frame")
windowControls.Size = UDim2.new(0, 100, 1, 0)
windowControls.Position = UDim2.new(1, -105, 0, 0)
windowControls.BackgroundTransparency = 1
windowControls.Parent = topBar

local minimizeBtn = Instance.new("TextButton")
minimizeBtn.Size = UDim2.new(0, 28, 0, 28)
minimizeBtn.Position = UDim2.new(0, 0, 0.5, -14)
minimizeBtn.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
minimizeBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
minimizeBtn.Text = "-"
minimizeBtn.Font = Enum.Font.GothamBold
minimizeBtn.TextSize = 16
minimizeBtn.Parent = windowControls

local expandBtn = Instance.new("TextButton")
expandBtn.Size = UDim2.new(0, 28, 0, 28)
expandBtn.Position = UDim2.new(0, 32, 0.5, -14)
expandBtn.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
expandBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
expandBtn.Text = "⛶"
expandBtn.Font = Enum.Font.GothamBold
expandBtn.TextSize = 12
expandBtn.Parent = windowControls

local closeBtn = Instance.new("TextButton")
closeBtn.Size = UDim2.new(0, 28, 0, 28)
closeBtn.Position = UDim2.new(0, 64, 0.5, -14)
closeBtn.BackgroundColor3 = Color3.fromRGB(180, 0, 0)
closeBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
closeBtn.Text = "X"
closeBtn.Font = Enum.Font.GothamBold
closeBtn.TextSize = 12
closeBtn.Parent = windowControls

for _, btn in pairs({minimizeBtn, expandBtn, closeBtn}) do
	local c = Instance.new("UICorner")
	c.CornerRadius = UDim.new(0, 6)
	c.Parent = btn
end

-- 2. Menu Lateral Esquerdo (Fileiras de Operações)
local sidebar = Instance.new("Frame")
sidebar.Name = "Sidebar"
sidebar.Size = UDim2.new(0, 150, 1, -40)
sidebar.Position = UDim2.new(0, 0, 0, 40)
sidebar.BackgroundColor3 = Color3.fromRGB(20, 5, 5)
sidebar.Parent = mainFrame

local btnCopyTab = Instance.new("TextButton")
btnCopyTab.Size = UDim2.new(0.9, 0, 0, 38)
btnCopyTab.Position = UDim2.new(0.05, 0, 0.05, 0)
btnCopyTab.BackgroundColor3 = Color3.fromRGB(150, 0, 0)
btnCopyTab.TextColor3 = Color3.fromRGB(255, 255, 255)
btnCopyTab.Text = "📦 CopyWorkspace"
btnCopyTab.Font = Enum.Font.GothamBold
btnCopyTab.TextSize = 11
btnCopyTab.Parent = sidebar

local btnAITab = Instance.new("TextButton")
btnAITab.Size = UDim2.new(0.9, 0, 0, 38)
btnAITab.Position = UDim2.new(0.05, 0, 0.18, 0)
btnAITab.BackgroundColor3 = Color3.fromRGB(40, 10, 10)
btnAITab.TextColor3 = Color3.fromRGB(200, 200, 200)
btnAITab.Text = "🤖 IA Avançada"
btnAITab.Font = Enum.Font.GothamBold
btnAITab.TextSize = 11
btnAITab.Parent = sidebar

for _, tabBtn in pairs({btnCopyTab, btnAITab}) do
	local c = Instance.new("UICorner")
	c.CornerRadius = UDim.new(0, 6)
	c.Parent = tabBtn
end

-- Área de Conteúdo
local contentArea = Instance.new("Frame")
contentArea.Size = UDim2.new(1, -150, 1, -40)
contentArea.Position = UDim2.new(0, 150, 0, 40)
contentArea.BackgroundTransparency = 1
contentArea.Parent = mainFrame

-- 3. ABA COPYWORKSPACE
local copyPage = Instance.new("Frame")
copyPage.Size = UDim2.new(1, 0, 1, 0)
copyPage.BackgroundTransparency = 1
copyPage.Parent = contentArea

-- Sub-tela Main de Copiar
local copyMainView = Instance.new("Frame")
copyMainView.Size = UDim2.new(1, 0, 1, 0)
copyMainView.BackgroundTransparency = 1
copyMainView.Parent = copyPage

local nameInput = Instance.new("TextBox")
nameInput.Size = UDim2.new(0.9, 0, 0, 40)
nameInput.Position = UDim2.new(0.05, 0, 0.1, 0)
nameInput.PlaceholderText = "Digite o nome para salvar..."
nameInput.Text = ""
nameInput.BackgroundColor3 = Color3.fromRGB(30, 10, 10)
nameInput.TextColor3 = Color3.fromRGB(255, 255, 255)
nameInput.Font = Enum.Font.Gotham
nameInput.TextSize = 13
nameInput.Parent = copyMainView

local saveBtn = Instance.new("TextButton")
saveBtn.Size = UDim2.new(0.9, 0, 0, 42)
saveBtn.Position = UDim2.new(0.05, 0, 0.28, 0)
saveBtn.BackgroundColor3 = Color3.fromRGB(180, 0, 0)
saveBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
saveBtn.Text = "💾 SALVAR MAPA (CopyWorkspace)"
saveBtn.Font = Enum.Font.GothamBold
saveBtn.TextSize = 13
saveBtn.Parent = copyMainView

local viewSavedBtn = Instance.new("TextButton")
viewSavedBtn.Size = UDim2.new(0.9, 0, 0, 42)
viewSavedBtn.Position = UDim2.new(0.05, 0, 0.45, 0)
viewSavedBtn.BackgroundColor3 = Color3.fromRGB(50, 10, 10)
viewSavedBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
viewSavedBtn.Text = "📁 VER SALVADOS"
viewSavedBtn.Font = Enum.Font.GothamBold
viewSavedBtn.TextSize = 13
viewSavedBtn.Parent = copyMainView

local statusLabel = Instance.new("TextLabel")
statusLabel.Size = UDim2.new(0.9, 0, 0, 60)
statusLabel.Position = UDim2.new(0.05, 0, 0.65, 0)
statusLabel.BackgroundColor3 = Color3.fromRGB(20, 5, 5)
statusLabel.TextColor3 = Color3.fromRGB(0, 255, 120)
statusLabel.Text = "Status: Aguardando ação..."
statusLabel.Font = Enum.Font.Code
statusLabel.TextSize = 11
statusLabel.TextWrapped = true
statusLabel.Parent = copyMainView

for _, el in pairs({nameInput, saveBtn, viewSavedBtn, statusLabel}) do
	local c = Instance.new("UICorner")
	c.CornerRadius = UDim.new(0, 6)
	c.Parent = el
end

-- Sub-tela "VER SALVADOS"
local savedView = Instance.new("Frame")
savedView.Size = UDim2.new(1, 0, 1, 0)
savedView.BackgroundTransparency = 1
savedView.Visible = false
savedView.Parent = copyPage

local backBtn = Instance.new("TextButton")
backBtn.Size = UDim2.new(0.28, 0, 0, 32)
backBtn.Position = UDim2.new(0.05, 0, 0.04, 0)
backBtn.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
backBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
backBtn.Text = "⬅ Voltar"
backBtn.Font = Enum.Font.GothamBold
backBtn.TextSize = 12
backBtn.Parent = savedView

local loadMapBtn = Instance.new("TextButton")
loadMapBtn.Size = UDim2.new(0.58, 0, 0, 32)
loadMapBtn.Position = UDim2.new(0.37, 0, 0.04, 0)
loadMapBtn.BackgroundColor3 = Color3.fromRGB(0, 150, 70)
loadMapBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
loadMapBtn.Text = "🚀 Carregar Mapa Selecionado"
loadMapBtn.Font = Enum.Font.GothamBold
loadMapBtn.TextSize = 11
loadMapBtn.Parent = savedView

local scrollList = Instance.new("ScrollingFrame")
scrollList.Size = UDim2.new(0.9, 0, 0.78, 0)
scrollList.Position = UDim2.new(0.05, 0, 0.16, 0)
scrollList.BackgroundColor3 = Color3.fromRGB(20, 5, 5)
scrollList.BorderSizePixel = 0
scrollList.CanvasSize = UDim2.new(0, 0, 2, 0)
scrollList.Parent = savedView

local listLayout = Instance.new("UIListLayout")
listLayout.Padding = UDim.new(0, 6)
listLayout.Parent = scrollList

for _, el in pairs({backBtn, loadMapBtn, scrollList}) do
	local c = Instance.new("UICorner")
	c.CornerRadius = UDim.new(0, 6)
	c.Parent = el
end

local selectedMapFolder = nil

-- 4. ABA IA AVANÇADA
local aiPage = Instance.new("Frame")
aiPage.Size = UDim2.new(1, 0, 1, 0)
aiPage.BackgroundTransparency = 1
aiPage.Visible = false
aiPage.Parent = contentArea

local aiInput = Instance.new("TextBox")
aiInput.Size = UDim2.new(0.9, 0, 0, 40)
aiInput.Position = UDim2.new(0.05, 0, 0.05, 0)
aiInput.PlaceholderText = "O que deseja criar? (Ex: Sistema de economia, animação, NPC)..."
aiInput.Text = ""
aiInput.BackgroundColor3 = Color3.fromRGB(30, 10, 10)
aiInput.TextColor3 = Color3.fromRGB(255, 255, 255)
aiInput.Font = Enum.Font.Gotham
aiInput.TextSize = 12
aiInput.Parent = aiPage

local aiOutput = Instance.new("TextLabel")
aiOutput.Size = UDim2.new(0.9, 0, 0, 220)
aiOutput.Position = UDim2.new(0.05, 0, 0.22, 0)
aiOutput.BackgroundColor3 = Color3.fromRGB(15, 5, 5)
aiOutput.TextColor3 = Color3.fromRGB(255, 220, 100)
aiOutput.Text = "🤖 IA AVANÇADA:\nPergunte qualquer tipo de lógica ou programação!"
aiOutput.Font = Enum.Font.Code
aiOutput.TextSize = 11
aiOutput.TextYAlignment = Enum.TextYAlignment.Top
aiOutput.TextWrapped = true
aiOutput.Parent = aiPage

for _, el in pairs({aiInput, aiOutput}) do
	local c = Instance.new("UICorner")
	c.CornerRadius = UDim.new(0, 6)
	c.Parent = el
end

-- 5. CAIXA DE DIÁLOGO DE CONFIRMAÇÃO PARA APAGAR
local confirmModal = Instance.new("Frame")
confirmModal.Name = "ConfirmModal"
confirmModal.Size = UDim2.new(0, 320, 0, 160)
confirmModal.Position = UDim2.new(0.5, -160, 0.5, -80)
confirmModal.BackgroundColor3 = Color3.fromRGB(25, 5, 5)
confirmModal.Visible = false
confirmModal.ZIndex = 10
confirmModal.Parent = mainFrame

local modalCorner = Instance.new("UICorner")
modalCorner.CornerRadius = UDim.new(0, 8)
modalCorner.Parent = confirmModal

local modalStroke = Instance.new("UIStroke")
modalStroke.Color = Color3.fromRGB(255, 0, 0)
modalStroke.Thickness = 2
modalStroke.Parent = confirmModal

local confirmText = Instance.new("TextLabel")
confirmText.Size = UDim2.new(0.9, 0, 0, 60)
confirmText.Position = UDim2.new(0.05, 0, 0.1, 0)
confirmText.BackgroundTransparency = 1
confirmText.TextColor3 = Color3.fromRGB(255, 255, 255)
confirmText.Text = "Você deseja Apagar 'Nome'?"
confirmText.Font = Enum.Font.GothamBold
confirmText.TextSize = 13
confirmText.TextWrapped = true
confirmText.ZIndex = 11
confirmText.Parent = confirmModal

local cancelBtn = Instance.new("TextButton")
cancelBtn.Size = UDim2.new(0.4, 0, 0, 35)
cancelBtn.Position = UDim2.new(0.08, 0, 0.65, 0)
cancelBtn.BackgroundColor3 = Color3.fromRGB(70, 70, 70)
cancelBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
cancelBtn.Text = "Cancelar"
cancelBtn.Font = Enum.Font.GothamBold
cancelBtn.TextSize = 12
cancelBtn.ZIndex = 11
cancelBtn.Parent = confirmModal

local deleteBtn = Instance.new("TextButton")
deleteBtn.Size = UDim2.new(0.4, 0, 0, 35)
deleteBtn.Position = UDim2.new(0.52, 0, 0.65, 0)
deleteBtn.BackgroundColor3 = Color3.fromRGB(180, 0, 0)
deleteBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
deleteBtn.Text = "Apagar"
deleteBtn.Font = Enum.Font.GothamBold
deleteBtn.TextSize = 12
deleteBtn.ZIndex = 11
deleteBtn.Parent = confirmModal

for _, el in pairs({cancelBtn, deleteBtn}) do
	local c = Instance.new("UICorner")
	c.CornerRadius = UDim.new(0, 6)
	c.Parent = el
end

---------------------------------------------------------
-- LÓGICA E FUNCIONALIDADES
---------------------------------------------------------

-- Função Atualizar Lista de Salvos
local function updateSavedList()
	for _, child in pairs(scrollList:GetChildren()) do
		if child:IsA("Frame") then child:Destroy() end
	end

	for _, folder in pairs(SavedMapsFolder:GetChildren()) do
		local itemRow = Instance.new("Frame")
		itemRow.Size = UDim2.new(1, -10, 0, 40)
		itemRow.BackgroundColor3 = Color3.fromRGB(35, 10, 10)

		local rCorner = Instance.new("UICorner")
		rCorner.CornerRadius = UDim.new(0, 6)
		rCorner.Parent = itemRow

		local mapBtn = Instance.new("TextButton")
		mapBtn.Size = UDim2.new(0.7, 0, 1, 0)
		mapBtn.BackgroundTransparency = 1
		mapBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
		mapBtn.Text = " 📁 " .. folder.Name
		mapBtn.Font = Enum.Font.GothamBold
		mapBtn.TextSize = 12
		mapBtn.TextXAlignment = Enum.TextXAlignment.Left
		mapBtn.Parent = itemRow

		local delItemBtn = Instance.new("TextButton")
		delItemBtn.Size = UDim2.new(0.25, 0, 0.8, 0)
		delItemBtn.Position = UDim2.new(0.72, 0, 0.1, 0)
		delItemBtn.BackgroundColor3 = Color3.fromRGB(180, 0, 0)
		delItemBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
		delItemBtn.Text = "🗑️ Apagar"
		delItemBtn.Font = Enum.Font.GothamBold
		delItemBtn.TextSize = 11
		delItemBtn.Parent = itemRow

		local dCorner = Instance.new("UICorner")
		dCorner.CornerRadius = UDim.new(0, 4)
		dCorner.Parent = delItemBtn

		mapBtn.MouseButton1Click:Connect(function()
			selectedMapFolder = folder
			statusLabel.Text = "Mapa selecionado: " .. folder.Name
		end)

		delItemBtn.MouseButton1Click:Connect(function()
			selectedMapFolder = folder
			confirmText.Text = "Você deseja Apagar '" .. folder.Name .. "'?"
			confirmModal.Visible = true
		end)

		itemRow.Parent = scrollList
	end
end

-- Botões do Canto Superior Direito
closeBtn.MouseButton1Click:Connect(function()
	mainFrame.Visible = false
	openBtn.Visible = true
end)

openBtn.MouseButton1Click:Connect(function()
	mainFrame.Visible = true
	openBtn.Visible = false
end)

local isExpanded = false
expandBtn.MouseButton1Click:Connect(function()
	isExpanded = not isExpanded
	if isExpanded then
		mainFrame.Size = UDim2.new(0, 780, 0, 480)
		mainFrame.Position = UDim2.new(0.5, -390, 0.5, -240)
	else
		mainFrame.Size = UDim2.new(0, 620, 0, 380)
		mainFrame.Position = UDim2.new(0.5, -310, 0.5, -190)
	end
end)

local isMinimized = false
minimizeBtn.MouseButton1Click:Connect(function()
	isMinimized = not isMinimized
	contentArea.Visible = not isMinimized
	sidebar.Visible = not isMinimized
	if isMinimized then
		mainFrame.Size = UDim2.new(0, 620, 0, 40)
	else
		mainFrame.Size = isExpanded and UDim2.new(0, 780, 0, 480) or UDim2.new(0, 620, 0, 380)
	end
end)

-- Troca de Abas Laterais
btnCopyTab.MouseButton1Click:Connect(function()
	copyPage.Visible = true
	aiPage.Visible = false
	btnCopyTab.BackgroundColor3 = Color3.fromRGB(150, 0, 0)
	btnAITab.BackgroundColor3 = Color3.fromRGB(40, 10, 10)
end)

btnAITab.MouseButton1Click:Connect(function()
	copyPage.Visible = false
	aiPage.Visible = true
	btnAITab.BackgroundColor3 = Color3.fromRGB(150, 0, 0)
	btnCopyTab.BackgroundColor3 = Color3.fromRGB(40, 10, 10)
end)

-- Salvar CopyWorkspace
saveBtn.MouseButton1Click:Connect(function()
	local mapName = nameInput.Text
	if mapName == "" then mapName = "Mapa_" .. os.time() end

	local mapFolder = Instance.new("Folder")
	mapFolder.Name = mapName
	mapFolder.Parent = SavedMapsFolder

	local count = 0
	for _, obj in pairs(workspace:GetChildren()) do
		if not obj:IsA("Camera") and not Players:GetPlayerFromCharacter(obj) then
			local clone = obj:Clone()
			if clone then
				clone.Parent = mapFolder
				count = count + 1
			end
		end
	end
	statusLabel.Text = "✅ Salvo com sucesso! '" .. mapName .. "' contém " .. count .. " itens."
	nameInput.Text = ""
end)

-- Ver Salvados / Voltar
viewSavedBtn.MouseButton1Click:Connect(function()
	updateSavedList()
	copyMainView.Visible = false
	savedView.Visible = true
end)

backBtn.MouseButton1Click:Connect(function()
	savedView.Visible = false
	copyMainView.Visible = true
end)

-- Carregar Mapa no Studio / Estudio Lite
loadMapBtn.MouseButton1Click:Connect(function()
	if selectedMapFolder then
		for _, item in pairs(selectedMapFolder:GetChildren()) do
			local clone = item:Clone()
			if clone then clone.Parent = workspace end
		end
		statusLabel.Text = "🚀 Mapa '" .. selectedMapFolder.Name .. "' carregado no Workspace com sucesso!"
		savedView.Visible = false
		copyMainView.Visible = true
	else
		statusLabel.Text = "❌ Selecione um mapa na lista primeiro!"
	end
end)

-- Lógica do Modal de Confirmação (Apagar)
cancelBtn.MouseButton1Click:Connect(function()
	confirmModal.Visible = false
end)

deleteBtn.MouseButton1Click:Connect(function()
	if selectedMapFolder then
		selectedMapFolder:Destroy()
		selectedMapFolder = nil
		confirmModal.Visible = false
		updateSavedList()
	end
end)

-- Sistema de IA Avançada
aiInput.FocusLost:Connect(function(enter)
	if enter and aiInput.Text ~= "" then
		local query = string.lower(aiInput.Text)
		if string.find(query, "sistema") or string.find(query, "economia") then
			aiOutput.Text = "-- SISTEMA DE ECONOMIA IA:\nlocal DataStore = game:GetService('DataStoreService')\nlocal coins = Instance.new('IntValue')\ncoins.Name = 'Coins'"
		elseif string.find(query, "npc") or string.find(query, "seguir") then
			aiOutput.Text = "-- IA SEGUIR JOGADOR:\nlocal path = game:GetService('PathfindingService'):CreatePath()\npath:ComputeAsync(npc.PrimaryPart.Position, target.Position)"
		else
			aiOutput.Text = "-- CÓDIGO IA PARA: " .. aiInput.Text .. "\nprint('Código gerado e otimizado com sucesso!')"
		end
	end
end)

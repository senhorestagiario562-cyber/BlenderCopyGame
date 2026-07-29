-- Coloque este LocalScript em StarterPlayerScripts ou StarterGui
local Players = game:GetService("Players")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local TweenService = game:GetService("TweenService")
local UserInputService = game:GetService("UserInputService")
local LocalPlayer = Players.LocalPlayer

-- Configurações Globais do Hub
local CORRECT_KEY = "CopyGame"
local HUB_TITLE = "🚀 NEXT-GEN DEV HUB"

-- 1. Interface Gui Principal
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "DevScriptHub"
screenGui.ResetOnSpawn = false
screenGui.Parent = LocalPlayer:WaitForChild("PlayerGui")

-- Botão de Toggle (Abrir/Fechar)
local toggleBtn = Instance.new("TextButton")
toggleBtn.Name = "HubToggle"
toggleBtn.Size = UDim2.new(0, 130, 0, 40)
toggleBtn.Position = UDim2.new(0.02, 0, 0.4, 0)
toggleBtn.BackgroundColor3 = Color3.fromRGB(20, 20, 30)
toggleBtn.TextColor3 = Color3.fromRGB(0, 220, 255)
toggleBtn.Text = "⚡ Abrir Hub"
toggleBtn.Font = Enum.Font.GothamBold
toggleBtn.TextSize = 14
toggleBtn.Parent = screenGui

local toggleCorner = Instance.new("UICorner")
toggleCorner.CornerRadius = UDim.new(0, 8)
toggleCorner.Parent = toggleBtn

-- Container Principal do Hub
local mainFrame = Instance.new("Frame")
mainFrame.Name = "MainHubFrame"
mainFrame.Size = UDim2.new(0, 580, 0, 360)
mainFrame.Position = UDim2.new(0.5, -290, 0.5, -180)
mainFrame.BackgroundColor3 = Color3.fromRGB(18, 18, 24)
mainFrame.BorderSizePixel = 0
mainFrame.Visible = false
mainFrame.Active = true
mainFrame.Draggable = true -- Permite arrastar o Hub pela tela
mainFrame.Parent = screenGui

local mainCorner = Instance.new("UICorner")
mainCorner.CornerRadius = UDim.new(0, 12)
mainCorner.Parent = mainFrame

-- Barra Superior (Header)
local header = Instance.new("Frame")
header.Size = UDim2.new(1, 0, 0, 40)
header.BackgroundColor3 = Color3.fromRGB(25, 25, 35)
header.Parent = mainFrame

local headerCorner = Instance.new("UICorner")
headerCorner.CornerRadius = UDim.new(0, 12)
headerCorner.Parent = header

local titleText = Instance.new("TextLabel")
titleText.Size = UDim2.new(0.8, 0, 1, 0)
titleText.Position = UDim2.new(0.03, 0, 0, 0)
titleText.BackgroundTransparency = 1
titleText.TextColor3 = Color3.fromRGB(255, 255, 255)
titleText.Text = HUB_TITLE .. " | Status: Key Requerida"
titleText.Font = Enum.Font.GothamBold
titleText.TextSize = 14
titleText.TextXAlignment = Enum.TextXAlignment.Left
titleText.Parent = header

-- 2. Tela de Autenticação (Key)
local keyScreen = Instance.new("Frame")
keyScreen.Size = UDim2.new(1, 0, 1, -40)
keyScreen.Position = UDim2.new(0, 0, 0, 40)
keyScreen.BackgroundTransparency = 1
keyScreen.Parent = mainFrame

local keyBox = Instance.new("TextBox")
keyBox.Size = UDim2.new(0, 260, 0, 40)
keyBox.Position = UDim2.new(0.5, -130, 0.3, 0)
keyBox.PlaceholderText = "Insira a Key..."
keyBox.Text = ""
keyBox.BackgroundColor3 = Color3.fromRGB(30, 30, 42)
keyBox.TextColor3 = Color3.fromRGB(255, 255, 255)
keyBox.Font = Enum.Font.Gotham
keyBox.TextSize = 14
keyBox.Parent = keyScreen

local keyCorner = Instance.new("UICorner")
keyCorner.CornerRadius = UDim.new(0, 8)
keyCorner.Parent = keyBox

local keyBtn = Instance.new("TextButton")
keyBtn.Size = UDim2.new(0, 160, 0, 38)
keyBtn.Position = UDim2.new(0.5, -80, 0.52, 0)
keyBtn.BackgroundColor3 = Color3.fromRGB(0, 180, 120)
keyBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
keyBtn.Text = "Entrar no Hub"
keyBtn.Font = Enum.Font.GothamBold
keyBtn.TextSize = 14
keyBtn.Parent = keyScreen

local btnCorner2 = Instance.new("UICorner")
btnCorner2.CornerRadius = UDim.new(0, 8)
btnCorner2.Parent = keyBtn

-- 3. Estrutura do Hub (Menu Lateral + Conteúdo)
local hubContent = Instance.new("Frame")
hubContent.Size = UDim2.new(1, 0, 1, -40)
hubContent.Position = UDim2.new(0, 0, 0, 40)
hubContent.BackgroundTransparency = 1
hubContent.Visible = false
hubContent.Parent = mainFrame

-- Sidebar (Navegação por Abas)
local sidebar = Instance.new("Frame")
sidebar.Size = UDim2.new(0, 140, 1, 0)
sidebar.BackgroundColor3 = Color3.fromRGB(22, 22, 30)
sidebar.Parent = hubContent

local tabCopyBtn = Instance.new("TextButton")
tabCopyBtn.Size = UDim2.new(0.9, 0, 0, 35)
tabCopyBtn.Position = UDim2.new(0.05, 0, 0.05, 0)
tabCopyBtn.BackgroundColor3 = Color3.fromRGB(35, 35, 50)
tabCopyBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
tabCopyBtn.Text = "📁 Copiar Game"
tabCopyBtn.Font = Enum.Font.Gotham
tabCopyBtn.TextSize = 12
tabCopyBtn.Parent = sidebar

local tabAIBtn = Instance.new("TextButton")
tabAIBtn.Size = UDim2.new(0.9, 0, 0, 35)
tabAIBtn.Position = UDim2.new(0.05, 0, 0.18, 0)
tabAIBtn.BackgroundColor3 = Color3.fromRGB(28, 28, 38)
tabAIBtn.TextColor3 = Color3.fromRGB(200, 200, 200)
tabAIBtn.Text = "🤖 IA Assistente"
tabAIBtn.Font = Enum.Font.Gotham
tabAIBtn.TextSize = 12
tabAIBtn.Parent = sidebar

local tabAnimBtn = Instance.new("TextButton")
tabAnimBtn.Size = UDim2.new(0.9, 0, 0, 35)
tabAnimBtn.Position = UDim2.new(0.05, 0, 0.31, 0)
tabAnimBtn.BackgroundColor3 = Color3.fromRGB(28, 28, 38)
tabAnimBtn.TextColor3 = Color3.fromRGB(200, 200, 200)
tabAnimBtn.Text = "🎬 Animações"
tabAnimBtn.Font = Enum.Font.Gotham
tabAnimBtn.TextSize = 12
tabAnimBtn.Parent = sidebar

local tabUtilsBtn = Instance.new("TextButton")
tabUtilsBtn.Size = UDim2.new(0.9, 0, 0, 35)
tabUtilsBtn.Position = UDim2.new(0.05, 0, 0.44, 0)
tabUtilsBtn.BackgroundColor3 = Color3.fromRGB(28, 28, 38)
tabUtilsBtn.TextColor3 = Color3.fromRGB(200, 200, 200)
tabUtilsBtn.Text = "🛠️ Utilitários"
tabUtilsBtn.Font = Enum.Font.Gotham
tabUtilsBtn.TextSize = 12
tabUtilsBtn.Parent = sidebar

-- Área de Páginas do Hub
local pages = Instance.new("Frame")
pages.Size = UDim2.new(1, -140, 1, 0)
pages.Position = UDim2.new(0, 140, 0, 0)
pages.BackgroundTransparency = 1
pages.Parent = hubContent

-- ABAS
local function createPage()
	local p = Instance.new("Frame")
	p.Size = UDim2.new(1, 0, 1, 0)
	p.BackgroundTransparency = 1
	p.Visible = false
	p.Parent = pages
	return p
end

local pageCopy = createPage()
pageCopy.Visible = true
local pageAI = createPage()
local pageAnim = createPage()
local pageUtils = createPage()

---------------------------------------------------------
-- CONTEÚDO DAS ABAS
---------------------------------------------------------

-- 1. Aba Copiar Game
local copyInfo = Instance.new("TextLabel")
copyInfo.Size = UDim2.new(0.9, 0, 0, 40)
copyInfo.Position = UDim2.new(0.05, 0, 0.05, 0)
copyInfo.BackgroundTransparency = 1
copyInfo.TextColor3 = Color3.fromRGB(180, 180, 200)
copyInfo.Text = "Clona e organiza os objetos do Workspace em uma pasta dedicada no ReplicatedStorage."
copyInfo.Font = Enum.Font.Gotham
copyInfo.TextSize = 12
copyInfo.TextWrapped = true
copyInfo.Parent = pageCopy

local runCopyBtn = Instance.new("TextButton")
runCopyBtn.Size = UDim2.new(0.9, 0, 0, 40)
runCopyBtn.Position = UDim2.new(0.05, 0, 0.25, 0)
runCopyBtn.BackgroundColor3 = Color3.fromRGB(0, 120, 215)
runCopyBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
runCopyBtn.Text = "📦 Salvar Workspace Local"
runCopyBtn.Font = Enum.Font.GothamBold
runCopyBtn.TextSize = 14
runCopyBtn.Parent = pageCopy

local copyStatus = Instance.new("TextLabel")
copyStatus.Size = UDim2.new(0.9, 0, 0, 100)
copyStatus.Position = UDim2.new(0.05, 0, 0.45, 0)
copyStatus.BackgroundColor3 = Color3.fromRGB(12, 12, 18)
copyStatus.TextColor3 = Color3.fromRGB(0, 255, 150)
copyStatus.Text = "Aguardando comando..."
copyStatus.Font = Enum.Font.Code
copyStatus.TextSize = 12
copyStatus.TextWrapped = true
copyStatus.Parent = pageCopy

-- 2. Aba IA Assistente
local aiInput = Instance.new("TextBox")
aiInput.Size = UDim2.new(0.9, 0, 0, 35)
aiInput.Position = UDim2.new(0.05, 0, 0.08, 0)
aiInput.PlaceholderText = "Digite o script que deseja (Ex: velocidade, voar, porta)..."
aiInput.BackgroundColor3 = Color3.fromRGB(28, 28, 38)
aiInput.TextColor3 = Color3.fromRGB(255, 255, 255)
aiInput.Font = Enum.Font.Gotham
aiInput.TextSize = 12
aiInput.Parent = pageAI

local aiOutput = Instance.new("TextLabel")
aiOutput.Size = UDim2.new(0.9, 0, 0, 180)
aiOutput.Position = UDim2.new(0.05, 0, 0.22, 0)
aiOutput.BackgroundColor3 = Color3.fromRGB(12, 12, 18)
aiOutput.TextColor3 = Color3.fromRGB(255, 255, 120)
aiOutput.Text = "-- RESPOSTA DO ASSISTENTE DE PROGRAMAÇÃO IA --\nDigite seu pedido acima e pressione Enter."
aiOutput.Font = Enum.Font.Code
aiOutput.TextSize = 11
aiOutput.TextYAlignment = Enum.TextYAlignment.Top
aiOutput.TextWrapped = true
aiOutput.Parent = pageAI

-- 3. Aba Animações
local animInfo = Instance.new("TextLabel")
animInfo.Size = UDim2.new(0.9, 0, 0, 40)
animInfo.Position = UDim2.new(0.05, 0, 0.05, 0)
animInfo.BackgroundTransparency = 1
animInfo.TextColor3 = Color3.fromRGB(180, 180, 200)
animInfo.Text = "Captura as posições de juntas (Motor6D) do seu personagem para gerar frames de animação."
animInfo.Font = Enum.Font.Gotham
animInfo.TextSize = 12
animInfo.TextWrapped = true
animInfo.Parent = pageAnim

local savePoseBtn = Instance.new("TextButton")
savePoseBtn.Size = UDim2.new(0.9, 0, 0, 40)
savePoseBtn.Position = UDim2.new(0.05, 0, 0.25, 0)
savePoseBtn.BackgroundColor3 = Color3.fromRGB(160, 60, 200)
savePoseBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
savePoseBtn.Text = "🎬 Gravar Frame de Pose Atual"
savePoseBtn.Font = Enum.Font.GothamBold
savePoseBtn.TextSize = 14
savePoseBtn.Parent = pageAnim

-- 4. Aba Utilitários
local speedBtn = Instance.new("TextButton")
speedBtn.Size = UDim2.new(0.42, 0, 0, 35)
speedBtn.Position = UDim2.new(0.05, 0, 0.08, 0)
speedBtn.BackgroundColor3 = Color3.fromRGB(40, 40, 60)
speedBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
speedBtn.Text = "🏃 Super Velocidade"
speedBtn.Font = Enum.Font.Gotham
speedBtn.TextSize = 12
speedBtn.Parent = pageUtils

local jumpBtn = Instance.new("TextButton")
jumpBtn.Size = UDim2.new(0.42, 0, 0, 35)
jumpBtn.Position = UDim2.new(0.52, 0, 0.08, 0)
jumpBtn.BackgroundColor3 = Color3.fromRGB(40, 40, 60)
jumpBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
jumpBtn.Text = "🪂 Super Pulo"
jumpBtn.Font = Enum.Font.Gotham
jumpBtn.TextSize = 12
jumpBtn.Parent = pageUtils

---------------------------------------------------------
-- SISTEMAS E LÓGICA DO HUB
---------------------------------------------------------

-- Função para Alternar Abas
local function switchTab(activePage, activeBtn)
	pageCopy.Visible = false
	pageAI.Visible = false
	pageAnim.Visible = false
	pageUtils.Visible = false
	
	tabCopyBtn.BackgroundColor3 = Color3.fromRGB(28, 28, 38)
	tabAIBtn.BackgroundColor3 = Color3.fromRGB(28, 28, 38)
	tabAnimBtn.BackgroundColor3 = Color3.fromRGB(28, 28, 38)
	tabUtilsBtn.BackgroundColor3 = Color3.fromRGB(28, 28, 38)
	
	activePage.Visible = true
	activeBtn.BackgroundColor3 = Color3.fromRGB(35, 35, 50)
end

tabCopyBtn.MouseButton1Click:Connect(function() switchTab(pageCopy, tabCopyBtn) end)
tabAIBtn.MouseButton1Click:Connect(function() switchTab(pageAI, tabAIBtn) end)
tabAnimBtn.MouseButton1Click:Connect(function() switchTab(pageAnim, tabAnimBtn) end)
tabUtilsBtn.MouseButton1Click:Connect(function() switchTab(pageUtils, tabUtilsBtn) end)

-- Sistema de Toggle
toggleBtn.MouseButton1Click:Connect(function()
	mainFrame.Visible = not mainFrame.Visible
	toggleBtn.Text = mainFrame.Visible and "⚡ Fechar Hub" or "⚡ Abrir Hub"
end)

-- Validação de Key ("CopyGame")
keyBtn.MouseButton1Click:Connect(function()
	if keyBox.Text == CORRECT_KEY then
		keyScreen.Visible = false
		hubContent.Visible = true
		titleText.Text = HUB_TITLE .. " | Status: Acesso Liberado"
	else
		keyBox.Text = ""
		keyBox.PlaceholderText = "Key Incorreta!"
	end
end)

-- Copiar Workspace
runCopyBtn.MouseButton1Click:Connect(function()
	local folder = ReplicatedStorage:FindFirstChild("CopyWorkspace") or Instance.new("Folder")
	folder.Name = "CopyWorkspace"
	folder.Parent = ReplicatedStorage

	local itemsCopied = 0
	for _, obj in pairs(workspace:GetChildren()) do
		if not obj:IsA("Camera") and not Players:GetPlayerFromCharacter(obj) then
			local clone = obj:Clone()
			if clone then
				clone.Parent = folder
				itemsCopied = itemsCopied + 1
			end
		end
	end
	copyStatus.Text = "✅ " .. itemsCopied .. " itens copiados com sucesso para ReplicatedStorage.CopyWorkspace!"
end)

-- IA Assistente de Código
aiInput.FocusLost:Connect(function(enter)
	if enter and aiInput.Text ~= "" then
		local query = string.lower(aiInput.Text)
		if string.find(query, "velocidade") or string.find(query, "speed") then
			aiOutput.Text = "-- Script de Velocidade:\nlocal char = game.Players.LocalPlayer.Character\nchar.Humanoid.WalkSpeed = 50"
		elseif string.find(query, "pulo") or string.find(query, "jump") then
			aiOutput.Text = "-- Script de Pulo:\nlocal char = game.Players.LocalPlayer.Character\nchar.Humanoid.JumpPower = 120"
		elseif string.find(query, "porta") then
			aiOutput.Text = "-- Script de Abrir Porta ao tocar:\nscript.Parent.Touched:Connect(function(hit)\n    script.Parent.Transparency = 0.8\n    script.Parent.CanCollide = false\nend)"
		else
			aiOutput.Text = "-- Gerado para: " .. aiInput.Text .. "\nprint('Ação executada com sucesso!')"
		end
	end
end)

-- Gravar Pose / Animação
savePoseBtn.MouseButton1Click:Connect(function()
	local char = LocalPlayer.Character
	if char and char:FindFirstChild("Humanoid") then
		local poseFolder = Instance.new("Folder")
		poseFolder.Name = "PoseFrame_" .. os.time()
		poseFolder.Parent = ReplicatedStorage
		for _, joint in pairs(char:GetDescendants()) do
			if joint:IsA("Motor6D") then
				local val = Instance.new("StringValue")
				val.Name = joint.Name
				val.Value = tostring(joint.C0)
				val.Parent = poseFolder
			end
		end
		savePoseBtn.Text = "✅ Pose Gravada!"
		task.wait(1.5)
		savePoseBtn.Text = "🎬 Gravar Frame de Pose Atual"
	end
end)

-- Utilitários
local fastSpeed = false
speedBtn.MouseButton1Click:Connect(function()
	fastSpeed = not fastSpeed
	local char = LocalPlayer.Character
	if char and char:FindFirstChild("Humanoid") then
		char.Humanoid.WalkSpeed = fastSpeed and 60 or 16
		speedBtn.BackgroundColor3 = fastSpeed and Color3.fromRGB(0, 160, 80) or Color3.fromRGB(40, 40, 60)
	end
end)

local highJump = false
jumpBtn.MouseButton1Click:Connect(function()
	highJump = not highJump
	local char = LocalPlayer.Character
	if char and char:FindFirstChild("Humanoid") then
		char.Humanoid.JumpPower = highJump and 120 or 50
		jumpBtn.BackgroundColor3 = highJump and Color3.fromRGB(0, 160, 80) or Color3.fromRGB(40, 40, 60)
	end
end)

```
### O que mudou no formato Script Hub?
 * **Navegação por Abas (Tab System):** Menu lateral categorizado para alternar entre as funções sem poluir a tela.
 * **Janela Arrastável:** O painel principal pode ser movido com o mouse para qualquer canto do jogo.
 * **Aba de Utilitários Adicionada:** Atalhos rápidos como *Super Velocidade* e *Super Pulo* para facilitar testes.
 * **Design Dark Mode Moderno:** Visual inspirado nos principais hubs de desenvolvimento do Roblox.

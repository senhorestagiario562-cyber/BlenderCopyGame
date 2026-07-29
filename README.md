-- Coloque este LocalScript dentro de StarterPlayer -> StarterPlayerScripts ou StarterGui
local Players = game:GetService("Players")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local TweenService = game:GetService("TweenService")
local LocalPlayer = Players.LocalPlayer

-- Configurações Globais
local CORRECT_KEY = "CopyGame"
local isKeyVerified = false

-- 1. Criação da Interface do Usuário (ScreenGui)
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "DevStudioPanel"
screenGui.ResetOnSpawn = false
screenGui.Parent = LocalPlayer:WaitForChild("PlayerGui")

-- Botão Flutuante de Abrir/Fechar
local toggleButton = Instance.new("TextButton")
toggleButton.Name = "ToggleButton"
toggleButton.Size = UDim2.new(0, 120, 0, 40)
toggleButton.Position = UDim2.new(0.02, 0, 0.45, 0)
toggleButton.BackgroundColor3 = Color3.fromRGB(35, 35, 45)
toggleButton.TextColor3 = Color3.fromRGB(255, 255, 255)
toggleButton.Text = "Abrir Painel"
toggleButton.Font = Enum.Font.SourceSansBold
toggleButton.TextSize = 16
toggleButton.Parent = screenGui

local btnCorner = Instance.new("UICorner")
btnCorner.CornerRadius = UDim.new(0, 8)
btnCorner.Parent = toggleButton

-- Painel Principal
local mainFrame = Instance.new("Frame")
mainFrame.Name = "MainFrame"
mainFrame.Size = UDim2.new(0, 480, 0, 320)
mainFrame.Position = UDim2.new(0.5, -240, 0.5, -160)
mainFrame.BackgroundColor3 = Color3.fromRGB(25, 25, 30)
mainFrame.Visible = false
mainFrame.Parent = screenGui

local frameCorner = Instance.new("UICorner")
frameCorner.CornerRadius = UDim.new(0, 10)
frameCorner.Parent = mainFrame

-- Título do Painel
local titleLabel = Instance.new("TextLabel")
titleLabel.Size = UDim2.new(1, 0, 0, 40)
titleLabel.BackgroundColor3 = Color3.fromRGB(40, 40, 50)
titleLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
titleLabel.Text = "🛠️ Painel Dev & Studio Assistant"
titleLabel.Font = Enum.Font.SourceSansBold
titleLabel.TextSize = 18
titleLabel.Parent = mainFrame

-- 2. Sistema de Key (Código "CopyGame")
local keyFrame = Instance.new("Frame")
keyFrame.Size = UDim2.new(1, 0, 1, -40)
keyFrame.Position = UDim2.new(0, 0, 0, 40)
keyFrame.BackgroundTransparency = 1
keyFrame.Parent = mainFrame

local keyInput = Instance.new("TextBox")
keyInput.Size = UDim2.new(0, 250, 0, 40)
keyInput.Position = UDim2.new(0.5, -125, 0.3, 0)
keyInput.PlaceholderText = "Digite a Key aqui..."
keyInput.Text = ""
keyInput.BackgroundColor3 = Color3.fromRGB(45, 45, 55)
keyInput.TextColor3 = Color3.fromRGB(255, 255, 255)
keyInput.Font = Enum.Font.SourceSans
keyInput.TextSize = 16
keyInput.Parent = keyFrame

local keySubmit = Instance.new("TextButton")
keySubmit.Size = UDim2.new(0, 150, 0, 35)
keySubmit.Position = UDim2.new(0.5, -75, 0.55, 0)
keySubmit.BackgroundColor3 = Color3.fromRGB(0, 170, 100)
keySubmit.TextColor3 = Color3.fromRGB(255, 255, 255)
keySubmit.Text = "Verificar Key"
keySubmit.Font = Enum.Font.SourceSansBold
keySubmit.TextSize = 16
keySubmit.Parent = keyFrame

-- Conteúdo Principal (Liberado após a Key)
local contentFrame = Instance.new("Frame")
contentFrame.Size = UDim2.new(1, 0, 1, -40)
contentFrame.Position = UDim2.new(0, 0, 0, 40)
contentFrame.BackgroundTransparency = 1
contentFrame.Visible = false
contentFrame.Parent = mainFrame

-- Botões do Painel de Ferramentas
local btnCopyWorkspace = Instance.new("TextButton")
btnCopyWorkspace.Size = UDim2.new(0, 200, 0, 40)
btnCopyWorkspace.Position = UDim2.new(0.05, 0, 0.1, 0)
btnCopyWorkspace.BackgroundColor3 = Color3.fromRGB(50, 100, 200)
btnCopyWorkspace.TextColor3 = Color3.fromRGB(255, 255, 255)
btnCopyWorkspace.Text = "📦 Salvar Workspace Local"
btnCopyWorkspace.Font = Enum.Font.SourceSansBold
btnCopyWorkspace.TextSize = 14
btnCopyWorkspace.Parent = contentFrame

local btnAnimTool = Instance.new("TextButton")
btnAnimTool.Size = UDim2.new(0, 200, 0, 40)
btnAnimTool.Position = UDim2.new(0.52, 0, 0.1, 0)
btnAnimTool.BackgroundColor3 = Color3.fromRGB(180, 80, 200)
btnAnimTool.TextColor3 = Color3.fromRGB(255, 255, 255)
btnAnimTool.Text = "🎬 Criar Animação (Pose)"
btnAnimTool.Font = Enum.Font.SourceSansBold
btnAnimTool.TextSize = 14
btnAnimTool.Parent = contentFrame

-- Área do Assistente de IA
local aiLabel = Instance.new("TextLabel")
aiLabel.Size = UDim2.new(0.9, 0, 0, 25)
aiLabel.Position = UDim2.new(0.05, 0, 0.32, 0)
aiLabel.BackgroundTransparency = 1
aiLabel.TextColor3 = Color3.fromRGB(200, 200, 200)
aiLabel.Text = "🤖 Assistente de Programação IA:"
aiLabel.Font = Enum.Font.SourceSansBold
aiLabel.TextSize = 14
aiLabel.TextXAlignment = Enum.TextXAlignment.Left
aiLabel.Parent = contentFrame

local aiPrompt = Instance.new("TextBox")
aiPrompt.Size = UDim2.new(0.9, 0, 0, 35)
aiPrompt.Position = UDim2.new(0.05, 0, 0.42, 0)
aiPrompt.PlaceholderText = "Ex: Crie um script de corrida ou porta..."
aiPrompt.BackgroundColor3 = Color3.fromRGB(40, 40, 50)
aiPrompt.TextColor3 = Color3.fromRGB(255, 255, 255)
aiPrompt.Font = Enum.Font.SourceSans
aiPrompt.TextSize = 14
aiPrompt.Parent = contentFrame

local aiResponse = Instance.new("TextLabel")
aiResponse.Size = UDim2.new(0.9, 0, 0, 80)
aiResponse.Position = UDim2.new(0.05, 0, 0.58, 0)
aiResponse.BackgroundColor3 = Color3.fromRGB(15, 15, 20)
aiResponse.TextColor3 = Color3.fromRGB(100, 255, 100)
aiResponse.Text = "Aguardando seu pedido de script..."
aiResponse.Font = Enum.Font.Code
aiResponse.TextSize = 12
aiResponse.TextWrapped = true
aiResponse.Parent = contentFrame

-- 3. Lógica e Funcionalidades

-- Botão Abrir / Fechar
toggleButton.MouseButton1Click:Connect(function()
	mainFrame.Visible = not mainFrame.Visible
	toggleButton.Text = mainFrame.Visible and "Fechar Painel" or "Abrir Painel"
end)

-- Validação da Key "CopyGame"
keySubmit.MouseButton1Click:Connect(function()
	if keyInput.Text == CORRECT_KEY then
		isKeyVerified = true
		keyFrame.Visible = false
		contentFrame.Visible = true
		titleLabel.Text = "🛠️ Painel Dev - Acesso Liberado"
	else
		keyInput.Text = ""
		keyInput.PlaceholderText = "Key Incorreta! Tente novamente."
	end
end)

-- Funcionalidade: Salvar/Copiar Objetos do Workspace para uma Pasta
btnCopyWorkspace.MouseButton1Click:Connect(function()
	local copyFolder = ReplicatedStorage:FindFirstChild("CopyWorkspace")
	if not copyFolder then
		copyFolder = Instance.new("Folder")
		copyFolder.Name = "CopyWorkspace"
		copyFolder.Parent = ReplicatedStorage
	end

	local copiedCount = 0
	for _, item in pairs(workspace:GetChildren()) do
		-- Evita copiar a câmera e os personagens dos jogadores
		if not item:IsA("Camera") and not Players:GetPlayerFromCharacter(item) then
			local clone = item:Clone()
			if clone then
				clone.Parent = copyFolder
				copiedCount = copiedCount + 1
			end
		end
	end

	aiResponse.Text = "✅ " .. tostring(copiedCount) .. " objetos do Workspace foram duplicados com sucesso para ReplicatedStorage.CopyWorkspace!"
end)

-- Funcionalidade: Ferramenta de Animação Básica (Salvar Pose R6/R15)
btnAnimTool.MouseButton1Click:Connect(function()
	local char = LocalPlayer.Character
	if char and char:FindFirstChild("Humanoid") then
		local poseFolder = Instance.new("Folder")
		poseFolder.Name = "SavedPose_" .. os.time()
		poseFolder.Parent = ReplicatedStorage

		for _, joint in pairs(char:GetDescendants()) do
			if joint:IsA("Motor6D") then
				local jointData = Instance.new("StringValue")
				jointData.Name = joint.Name
				jointData.Value = tostring(joint.C0)
				jointData.Parent = poseFolder
			end
		end
		aiResponse.Text = "🎬 Pose atual do seu personagem foi capturada e salva em ReplicatedStorage!"
	else
		aiResponse.Text = "❌ Erro: Personagem não encontrado."
	end
end)

-- Funcionalidade: Resposta do Assistente de Código (IA de Exemplo)
aiPrompt.FocusLost:Connect(function(enterPressed)
	if enterPressed and aiPrompt.Text ~= "" then
		local query = string.lower(aiPrompt.Text)
		
		if string.find(query, "corrida") or string.find(query, "speed") then
			aiResponse.Text = "-- Script de Velocidade:\nHumanoid.WalkSpeed = 32"
		elseif string.find(query, "pulo") or string.find(query, "jump") then
			aiResponse.Text = "-- Script de Pulo:\nHumanoid.JumpPower = 100"
		elseif string.find(query, "porta") or string.find(query, "door") then
			aiResponse.Text = "-- Script de Abrir Porta:\nscript.Parent.Touched:Connect(function()\n  script.Parent.Transparency = 0.5\n  script.Parent.CanCollide = false\nend)"
		else
			aiResponse.Text = "-- Código gerado para: " .. aiPrompt.Text .. "\nprint('Executando ação personalizada...')"
		end
	end
end)

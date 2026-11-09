-- Garantir compatibilidade com Delta Executor
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local TweenService = game:GetService("TweenService")

local player = Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local humanoidRootPart = character:WaitForChild("HumanoidRootPart")
local humanoid = character:WaitForChild("Humanoid")

-- Proteção contra duplicação
if player.PlayerGui:FindFirstChild("FireGui") then
	player.PlayerGui.FireGui:Destroy()
end

-- Criando a ScreenGui principal
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "FireGui"
screenGui.ResetOnSpawn = false
screenGui.Parent = player:WaitForChild("PlayerGui")

-- Janela Principal
local mainFrame = Instance.new("Frame")
mainFrame.Name = "MainFrame"
mainFrame.Size = UDim2.new(0, 320, 0, 420) -- Tamanho compacto com rolagem
mainFrame.Position = UDim2.new(0.5, -160, 0.5, -210)
mainFrame.BackgroundColor3 = Color3.fromRGB(25, 25, 30)
mainFrame.BorderSizePixel = 0
mainFrame.Parent = screenGui

local cornerMain = Instance.new("UICorner")
cornerMain.CornerRadius = UDim.new(0, 12)
cornerMain.Parent = mainFrame

local strokeMain = Instance.new("UIStroke")
strokeMain.Color = Color3.fromRGB(45, 45, 55)
strokeMain.Thickness = 1.5
strokeMain.Parent = mainFrame

-- Barra Superior
local topBar = Instance.new("Frame")
topBar.Name = "TopBar"
topBar.Size = UDim2.new(1, 0, 0, 45)
topBar.BackgroundColor3 = Color3.fromRGB(30, 30, 38)
topBar.BorderSizePixel = 0
topBar.Parent = mainFrame

local cornerTop = Instance.new("UICorner")
cornerTop.CornerRadius = UDim.new(0, 12)
cornerTop.Parent = topBar

local fixTop = Instance.new("Frame")
fixTop.Size = UDim2.new(1, 0, 0, 10)
fixTop.Position = UDim2.new(0, 0, 1, -10)
fixTop.BackgroundColor3 = Color3.fromRGB(30, 30, 38)
fixTop.BorderSizePixel = 0
fixTop.Parent = topBar

local titleLabel = Instance.new("TextLabel")
titleLabel.Size = UDim2.new(1, -50, 1, 0)
titleLabel.Position = UDim2.new(0, 15, 0, 0)
titleLabel.BackgroundTransparency = 1
titleLabel.Font = Enum.Font.GothamBold
titleLabel.Text = "Fire GUI"
titleLabel.TextColor3 = Color3.fromRGB(240, 240, 245)
titleLabel.TextSize = 15
titleLabel.TextXAlignment = Enum.TextXAlignment.Left
titleLabel.Parent = topBar

-- Botão Minimizar (-)
local minimizeBtn = Instance.new("TextButton")
minimizeBtn.Size = UDim2.new(0, 30, 0, 30)
minimizeBtn.Position = UDim2.new(1, -35, 0.5, -15)
minimizeBtn.BackgroundColor3 = Color3.fromRGB(40, 40, 50)
minimizeBtn.Font = Enum.Font.GothamBold
minimizeBtn.Text = "-"
minimizeBtn.TextColor3 = Color3.fromRGB(200, 200, 210)
minimizeBtn.TextSize = 16
minimizeBtn.Parent = topBar

local cornerMin = Instance.new("UICorner")
cornerMin.CornerRadius = UDim.new(0, 8)
cornerMin.Parent = minimizeBtn

-- Container de Botões (Scrolling)
local container = Instance.new("ScrollingFrame")
container.Size = UDim2.new(1, -20, 1, -60)
container.Position = UDim2.new(0, 10, 0, 52)
container.BackgroundTransparency = 1
container.BorderSizePixel = 0
container.CanvasSize = UDim2.new(0, 0, 0, 420)
container.ScrollBarThickness = 4
container.Parent = mainFrame

local uiList = Instance.new("UIListLayout")
uiList.SortOrder = Enum.SortOrder.LayoutOrder
uiList.Padding = UDim.new(0, 12)
uiList.Parent = container

-- Função para criar botões
local function createButton(name, text, defaultColor)
	local btn = Instance.new("TextButton")
	btn.Name = name
	btn.Size = UDim2.new(1, 0, 0, 48)
	btn.BackgroundColor3 = defaultColor
	btn.Font = Enum.Font.GothamSemibold
	btn.Text = text .. ": OFF"
	btn.TextColor3 = Color3.fromRGB(255, 255, 255)
	btn.TextSize = 14
	btn.Parent = container

	local corner = Instance.new("UICorner")
	corner.CornerRadius = UDim.new(0, 8)
	corner.Parent = btn

	return btn
end

local noclipBtn = createButton("NoclipBtn", "Noclip", Color3.fromRGB(50, 50, 65))
local floatBtn = createButton("FloatBtn", "Float (Flutuar)", Color3.fromRGB(50, 50, 65))
local speedBtn = createButton("SpeedBtn", "Super Velocidade", Color3.fromRGB(50, 50, 65))
local jumpBtn = createButton("JumpBtn", "Super Pulo", Color3.fromRGB(50, 50, 65))
local flyBtn = createButton("FlyBtn", "Fly (Voo)", Color3.fromRGB(50, 50, 65))

-- Configuração de Velocidade do Voo (Visual Slider)
local speedContainer = Instance.new("Frame")
speedContainer.Size = UDim2.new(1, 0, 0, 50)
speedContainer.BackgroundColor3 = Color3.fromRGB(35, 35, 45)
speedContainer.Parent = container

local cornerSpeed = Instance.new("UICorner")
cornerSpeed.CornerRadius = UDim.new(0, 8)
cornerSpeed.Parent = speedContainer

local speedLabel = Instance.new("TextLabel")
speedLabel.Size = UDim2.new(1, -20, 0, 20)
speedLabel.Position = UDim2.new(0, 10, 0, 5)
speedLabel.BackgroundTransparency = 1
speedLabel.Font = Enum.Font.GothamSemibold
speedLabel.Text = "Velocidade do Voo: 50"
speedLabel.TextColor3 = Color3.fromRGB(200, 200, 210)
speedLabel.TextSize = 12
speedLabel.TextXAlignment = Enum.TextXAlignment.Left
speedLabel.Parent = speedContainer

local speedSliderBg = Instance.new("Frame")
speedSliderBg.Size = UDim2.new(1, -20, 0, 8)
speedSliderBg.Position = UDim2.new(0, 10, 0, 32)
speedSliderBg.BackgroundColor3 = Color3.fromRGB(50, 50, 65)
speedSliderBg.Parent = speedContainer

local cornerSliderBg = Instance.new("UICorner")
cornerSliderBg.CornerRadius = UDim.new(0, 4)
cornerSliderBg.Parent = speedSliderBg

local speedSliderFill = Instance.new("Frame")
speedSliderFill.Size = UDim2.new(0.5, 0, 1, 0) -- Padrão 50
speedSliderFill.BackgroundColor3 = Color3.fromRGB(70, 180, 90)
speedSliderFill.Parent = speedSliderBg

local cornerSliderFill = Instance.new("UICorner")
cornerSliderFill.CornerRadius = UDim.new(0, 4)
cornerSliderFill.Parent = speedSliderFill

-- ==================== FUNÇÕES ====================

-- 1. NOCLIP
local noclipActive = false
RunService.Stepped:Connect(function()
	if noclipActive and character then
		for _, part in ipairs(character:GetDescendants()) do
			if part:IsA("BasePart") and part.CanCollide then
				part.CanCollide = false
			end
		end
	end
end)

noclipBtn.MouseButton1Click:Connect(function()
	noclipActive = not noclipActive
	if noclipActive then
		noclipBtn.Text = "Noclip: ON"
		noclipBtn.BackgroundColor3 = Color3.fromRGB(70, 180, 90)
	else
		noclipBtn.Text = "Noclip: OFF"
		noclipBtn.BackgroundColor3 = Color3.fromRGB(50, 50, 65)
	end
end)

-- 2. FLOAT
local floatActive = false
local floatBp

floatBtn.MouseButton1Click:Connect(function()
	floatActive = not floatActive
	if floatActive then
		floatBtn.Text = "Float (Flutuar): ON"
		floatBtn.BackgroundColor3 = Color3.fromRGB(70, 180, 90)
		
		floatBp = Instance.new("BodyPosition")
		floatBp.Name = "FloatPosition"
		floatBp.MaxForce = Vector3.new(0, math.huge, 0)
		floatBp.Position = humanoidRootPart.Position
		floatBp.Parent = humanoidRootPart
	else
		floatBtn.Text = "Float (Flutuar): OFF"
		floatBtn.BackgroundColor3 = Color3.fromRGB(50, 50, 65)
		if floatBp then floatBp:Destroy() end
	end
end)

-- 3. SUPER VELOCIDADE (WalkSpeed)
local speedActive = false
speedBtn.MouseButton1Click:Connect(function()
	speedActive = not speedActive
	if speedActive then
		speedBtn.Text = "Super Velocidade: ON"
		speedBtn.BackgroundColor3 = Color3.fromRGB(70, 180, 90)
		if humanoid then humanoid.WalkSpeed = 100 end
	else
		speedBtn.Text = "Super Velocidade: OFF"
		speedBtn.BackgroundColor3 = Color3.fromRGB(50, 50, 65)
		if humanoid then humanoid.WalkSpeed = 16 end -- Padrão do Roblox
	end
end)

-- 4. SUPER PULO (JumpPower)
local jumpActive = false
jumpBtn.MouseButton1Click:Connect(function()
	jumpActive = not jumpActive
	if jumpActive then
		jumpBtn.Text = "Super Pulo: ON"
		jumpBtn.BackgroundColor3 = Color3.fromRGB(70, 180, 90)
		if humanoid then
			humanoid.UseJumpPower = true
			humanoid.JumpPower = 150
		end
	else
		jumpBtn.Text = "Super Pulo: OFF"
		jumpBtn.BackgroundColor3 = Color3.fromRGB(50, 50, 65)
		if humanoid then
			humanoid.UseJumpPower = true
			humanoid.JumpPower = 50 -- Padrão do Roblox
		end
	end
end)

-- 5. FLY (Com Controles Mobile na Tela)
local flyActive = false
local bg, bv
local flySpeed = 50

local flyControls = Instance.new("Frame")
flyControls.Name = "FlyControls"
flyControls.Size = UDim2.new(0, 160, 0, 120)
flyControls.Position = UDim2.new(1, -180, 1, -140)
flyControls.BackgroundTransparency = 1
flyControls.Visible = false
flyControls.Parent = screenGui

local function createFlyButton(name, text, pos)
	local btn = Instance.new("TextButton")
	btn.Name = name
	btn.Size = UDim2.new(0, 45, 0, 45)
	btn.Position = pos
	btn.BackgroundColor3 = Color3.fromRGB(35, 35, 45)
	btn.Font = Enum.Font.GothamBold
	btn.Text = text
	btn.TextColor3 = Color3.fromRGB(255, 255, 255)
	btn.TextSize = 16
	btn.Parent = flyControls

	local corner = Instance.new("UICorner")
	corner.CornerRadius = UDim.new(0, 8)
	corner.Parent = btn
	
	local stroke = Instance.new("UIStroke")
	stroke.Color = Color3.fromRGB(70, 180, 90)
	stroke.Thickness = 1.5
	stroke.Parent = btn
	
	return btn
end

local btnUp = createFlyButton("BtnUp", "Subir", UDim2.new(0.5, -22, 0, 0))
local btnDown = createFlyButton("BtnDown", "Descer", UDim2.new(0.5, -22, 1, -45))
local btnForward = createFlyButton("BtnFwd", "Frente", UDim2.new(0, 0, 0.5, -22))
local btnBack = createFlyButton("BtnBwd", "Trás", UDim2.new(1, -45, 0.5, -22))

local movingUp, movingDown, movingForward, movingBack = false, false, false, false

btnUp.MouseButton1Down:Connect(function() movingUp = true end)
btnUp.MouseButton1Up:Connect(function() movingUp = false end)
btnDown.MouseButton1Down:Connect(function() movingDown = true end)
btnDown.MouseButton1Up:Connect(function() movingDown = false end)
btnForward.MouseButton1Down:Connect(function() movingForward = true end)
btnForward.MouseButton1Up:Connect(function() movingForward = false end)
btnBack.MouseButton1Down:Connect(function() movingBack = true end)
btnBack.MouseButton1Up:Connect(function() movingBack = false end)

flyBtn.MouseButton1Click:Connect(function()
	flyActive = not flyActive
	if flyActive then
		if floatActive then
			floatActive = false
			floatBtn.Text = "Float (Flutuar): OFF"
			floatBtn.BackgroundColor3 = Color3.fromRGB(50, 50, 65)
			if floatBp then floatBp:Destroy() end
		end
		
		flyBtn.Text = "Fly (Voo): ON"
		flyBtn.BackgroundColor3 = Color3.fromRGB(70, 180, 90)
		flyControls.Visible = true
		
		bg = Instance.new("BodyGyro")
		bg.MaxTorque = Vector3.new(math.huge, math.huge, math.huge)
		bg.Parent = humanoidRootPart
		
		bv = Instance.new("BodyVelocity")
		bv.MaxForce = Vector3.new(math.huge, math.huge, math.huge)
		bv.Parent = humanoidRootPart
		
		RunService.RenderStepped:Connect(function()
			if not flyActive then return end
			local camera = workspace.CurrentCamera
			local moveDirection = Vector3.new()
			
			if UserInputService:IsKeyDown(Enum.KeyCode.W) or movingForward then moveDirection = moveDirection + camera.CFrame.LookVector end
			if UserInputService:IsKeyDown(Enum.KeyCode.S) or movingBack then moveDirection = moveDirection - camera.CFrame.LookVector end
			if UserInputService:IsKeyDown(Enum.KeyCode.A) then moveDirection = moveDirection - camera.CFrame.RightVector end
			if UserInputService:IsKeyDown(Enum.KeyCode.D) then moveDirection = moveDirection + camera.CFrame.RightVector end
			if UserInputService:IsKeyDown(Enum.KeyCode.Space) or movingUp then moveDirection = moveDirection + Vector3.new(0, 1, 0) end
			if movingDown then moveDirection = moveDirection - Vector3.new(0, 1, 0) end
			
			bg.CFrame = camera.CFrame
			bv.Velocity = moveDirection * flySpeed
		end)
	else
		flyBtn.Text = "Fly (Voo): OFF"
		flyBtn.BackgroundColor3 = Color3.fromRGB(50, 50, 65)
		flyControls.Visible = false
		if bg then bg:Destroy() end
		if bv then bv:Destroy() end
	end
end)

-- Slider de Velocidade Interativo
local sliding = false
speedSliderBg.InputBegan:Connect(function(input)
	if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
		sliding = true
	end
end)

UserInputService.InputEnded:Connect(function(input)
	if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
		sliding = false
	end
end)

UserInputService.InputChanged:Connect(function(input)
	if sliding and (input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch) then
		local pos = input.Position.X
		local absPos = speedSliderBg.AbsolutePosition.X
		local absSize = speedSliderBg.AbsoluteSize.X
		local percentage = math.clamp((pos - absPos) / absSize, 0, 1)
		
		speedSliderFill.Size = UDim2.new(percentage, 0, 1, 0)
		flySpeed = math.floor(percentage * 150)
		speedLabel.Text = "Velocidade do Voo: " .. flySpeed
	end
end)

-- 6. MINIMIZAR / MAXIMIZAR GUI
local minimized = false
minimizeBtn.MouseButton1Click:Connect(function()
	minimized = not minimized
	local targetSize = minimized and UDim2.new(0, 320, 0, 45) or UDim2.new(0, 320, 0, 420)
	
	container.Visible = false
	local tween = TweenService:Create(mainFrame, TweenInfo.new(0.25, Enum.EasingStyle.Quad, Enum.EasingDirection.Out), {Size = targetSize})
	tween:Play()
	tween.Completed:Wait()
	
	container.Visible = not minimized
	minimizeBtn.Text = minimized and "+" or "-"
end)

-- Arrastar a Janela (Mobile/Mouse)
local dragging, dragInput, dragStart, startPos
topBar.InputBegan:Connect(function(input)
	if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
		dragging = true
		dragStart = input.Position
		startPos = mainFrame.Position
		
		input.Changed:Connect(function()
			if input.UserInputState == Enum.UserInputState.End then
				dragging = false
			end
		end)
	end
end)

topBar.InputChanged:Connect(function(input)
	if input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch then
		dragInput = input
	end
end)

UserInputService.InputChanged:Connect(function(input)
	if input == dragInput and dragging then
		local delta = input.Position - dragStart
		mainFrame.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
	end
end)

-- Resetar ao renascer e atualizar referências
player.CharacterAdded:Connect(function(newChar)
	character = newChar
	humanoidRootPart = character:WaitForChild("HumanoidRootPart")
	humanoid = character:WaitForChild("Humanoid")
	flyActive = false
	noclipActive = false
	floatActive = false
	speedActive = false
	jumpActive = false
	flyControls.Visible = false
end)

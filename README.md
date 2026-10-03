--// AZURE HUB - SIMPLE UI

local Players = game:GetService("Players")
local TweenService = game:GetService("TweenService")

local Player = Players.LocalPlayer
local PlayerGui = Player:WaitForChild("PlayerGui")

--// Cores
local Purple = Color3.fromRGB(126, 35, 255)
local Magenta = Color3.fromRGB(218, 35, 255)
local Background = Color3.fromRGB(14, 9, 25)
local Card = Color3.fromRGB(28, 17, 45)
local Text = Color3.fromRGB(248, 241, 255)

--// GUI
local Gui = Instance.new("ScreenGui")
Gui.Name = "AzureHub"
Gui.ResetOnSpawn = false
Gui.Parent = PlayerGui

--// Botão flutuante
local OpenButton = Instance.new("TextButton")
OpenButton.Size = UDim2.fromOffset(48, 48)
OpenButton.Position = UDim2.new(0, 15, 0.5, -24)
OpenButton.BackgroundColor3 = Purple
OpenButton.Text = "A"
OpenButton.TextColor3 = Text
OpenButton.TextSize = 22
OpenButton.Font = Enum.Font.Arcade
OpenButton.AutoButtonColor = false
OpenButton.Parent = Gui

local OpenCorner = Instance.new("UICorner")
OpenCorner.CornerRadius = UDim.new(1, 0)
OpenCorner.Parent = OpenButton

--// Janela
local Main = Instance.new("Frame")
Main.Size = UDim2.fromOffset(390, 260)
Main.Position = UDim2.new(0.5, -195, 0.5, -130)
Main.BackgroundColor3 = Background
Main.Visible = false
Main.Parent = Gui

local MainCorner = Instance.new("UICorner")
MainCorner.CornerRadius = UDim.new(0, 10)
MainCorner.Parent = Main

local Stroke = Instance.new("UIStroke")
Stroke.Color = Purple
Stroke.Thickness = 1.5
Stroke.Parent = Main

--// Título
local Title = Instance.new("TextLabel")
Title.Size = UDim2.new(1, -50, 0, 42)
Title.Position = UDim2.fromOffset(15, 0)
Title.BackgroundTransparency = 1
Title.Text = "AZURE HUB"
Title.TextColor3 = Text
Title.TextSize = 20
Title.Font = Enum.Font.Arcade
Title.TextXAlignment = Enum.TextXAlignment.Left
Title.Parent = Main

--// Fechar
local Close = Instance.new("TextButton")
Close.Size = UDim2.fromOffset(35, 35)
Close.Position = UDim2.new(1, -42, 0, 4)
Close.BackgroundTransparency = 1
Close.Text = "×"
Close.TextColor3 = Magenta
Close.TextSize = 25
Close.Font = Enum.Font.Arcade
Close.Parent = Main

--// Linha
local Line = Instance.new("Frame")
Line.Size = UDim2.new(1, -20, 0, 1)
Line.Position = UDim2.fromOffset(10, 42)
Line.BackgroundColor3 = Purple
Line.BorderSizePixel = 0
Line.Parent = Main

--// Abas
local Tabs = Instance.new("Frame")
Tabs.Size = UDim2.new(0, 100, 1, -55)
Tabs.Position = UDim2.fromOffset(10, 50)
Tabs.BackgroundTransparency = 1
Tabs.Parent = Main

local TabsLayout = Instance.new("UIListLayout")
TabsLayout.Padding = UDim.new(0, 6)
TabsLayout.Parent = Tabs

--// Conteúdo
local Content = Instance.new("Frame")
Content.Size = UDim2.new(1, -125, 1, -60)
Content.Position = UDim2.fromOffset(120, 50)
Content.BackgroundColor3 = Card
Content.Parent = Main

local ContentCorner = Instance.new("UICorner")
ContentCorner.CornerRadius = UDim.new(0, 8)
ContentCorner.Parent = Content

--// ScrollFrame (mantém o visual, só permite rolar quando tiver muitos botões)
local Scroll = Instance.new("ScrollingFrame")
Scroll.Size = UDim2.new(1, 0, 1, 0)
Scroll.BackgroundTransparency = 1
Scroll.BorderSizePixel = 0
Scroll.ScrollBarThickness = 3
Scroll.ScrollBarImageColor3 = Purple
Scroll.CanvasSize = UDim2.new(0, 0, 0, 0)
Scroll.AutomaticCanvasSize = Enum.AutomaticSize.Y
Scroll.Parent = Content

local ScrollLayout = Instance.new("UIListLayout")
ScrollLayout.Padding = UDim.new(0, 6)
ScrollLayout.SortOrder = Enum.SortOrder.LayoutOrder
ScrollLayout.Parent = Scroll

local ScrollPadding = Instance.new("UIPadding")
ScrollPadding.PaddingTop = UDim.new(0, 10)
ScrollPadding.PaddingLeft = UDim.new(0, 10)
ScrollPadding.PaddingRight = UDim.new(0, 8)
ScrollPadding.Parent = Scroll

--// Criar aba
local function CreateTab(Name)
	local Button = Instance.new("TextButton")

	Button.Size = UDim2.new(1, 0, 0, 38)
	Button.BackgroundColor3 = Card
	Button.Text = Name
	Button.TextColor3 = Text
	Button.TextSize = 13
	Button.Font = Enum.Font.Arcade
	Button.AutoButtonColor = false
	Button.Parent = Tabs

	local Corner = Instance.new("UICorner")
	Corner.CornerRadius = UDim.new(0, 7)
	Corner.Parent = Button

	return Button
end

--// Criar botões (mesmo visual do original)
local function CreateButton(Name, Order, Callback)
	local Button = Instance.new("TextButton")

	Button.Size = UDim2.new(1, 0, 0, 38)
	Button.BackgroundColor3 = Background
	Button.Text = Name
	Button.TextColor3 = Text
	Button.TextSize = 13
	Button.Font = Enum.Font.Arcade
	Button.AutoButtonColor = false
	Button.LayoutOrder = Order
	Button.Parent = Scroll

	local Corner = Instance.new("UICorner")
	Corner.CornerRadius = UDim.new(0, 7)
	Corner.Parent = Button

	Button.MouseEnter:Connect(function()
		TweenService:Create(
			Button,
			TweenInfo.new(0.15),
			{BackgroundColor3 = Purple}
		):Play()
	end)

	Button.MouseLeave:Connect(function()
		TweenService:Create(
			Button,
			TweenInfo.new(0.15),
			{BackgroundColor3 = Background}
		):Play()
	end)

	Button.MouseButton1Click:Connect(function()
		if Callback then
			Callback()
		end
	end)

	return Button
end

--// ABAS
local R15Tab = CreateTab("R15")
local R6Tab = CreateTab("R6")
local GuiTab = CreateTab("Gui")

--// Limpar conteúdo
local function ClearContent()
	for _, Object in ipairs(Scroll:GetChildren()) do
		if not Object:IsA("UIListLayout") and not Object:IsA("UIPadding") then
			Object:Destroy()
		end
	end
end

--// Executar script com segurança
local function Run(code)
	local ok, err = pcall(function()
		loadstring(code)()
	end)
	if not ok then
		warn("[Azure Hub] Erro: " .. tostring(err))
	end
end

--// R15
local function ShowR15()
	ClearContent()

	CreateButton("C00LKIDD", 1, function()
		Run([[loadstring(game:HttpGet("https://rawscripts.net/raw/Universal-Script-FE-C00LKID-R15-83560"))()]])
	end)

	CreateButton("Jonh Doe", 2, function()
		Run([[loadstring(game:HttpGet(('https://pastefy.ga/n42Ougzx/raw'),true))()]])
	end)

	CreateButton("Idk XD", 3, function()
		Run([[loadstring(game:HttpGet("https://rawscripts.net/raw/Universal-Script-Stretched-Like-flowers-the-whistler-140882"))()]])
	end)

	CreateButton("Sonic Exe", 4, function()
		Run([[loadstring(game:HttpGet("https://pastefy.app/XCtZsGhP/raw"))()]])
	end)
end

--// R6
local function ShowR6()
	ClearContent()

	CreateButton("Walk in Walls", 1, function()
		Run([[loadstring(game:HttpGet("https://rawscripts.net/raw/Universal-Script-Wall-Walk-9153"))()]])
	end)

	CreateButton("Gojo", 2, function()
		Run([[loadstring(game:HttpGet("https://pastebin.com/raw/GutMLF0i"))()]])
	end)

	CreateButton("Admin", 3, function()
		Run([[loadstring(game:HttpGet("https://raw.githubusercontent.com/gObl00x/Pendulum-Fixed-AND-Others-Scripts/refs/heads/main/Server%20Admin"))()]])
	end)

	CreateButton("Banhammer", 4, function()
		Run([[loadstring(game:HttpGet("https://pastebin.com/raw/vBdzb7ns"))()]])
	end)

	CreateButton("Knife v4", 5, function()
		Run([[loadstring(game:HttpGet("https://rawscripts.net/raw/Universal-Script-Grab-knife-v4-24753"))()]])
	end)

	CreateButton("S#ised Gun", 6, function()
		Run([[loadstring(game:HttpGet("https://pastefy.app/6yaoBpet/raw"))()]])
	end)

	CreateButton("Caducus", 7, function()
		Run([[loadstring(game:HttpGet("https://rawscripts.net/raw/Universal-Script-FE-caducus-check-description-73933"))()]])
	end)

	CreateButton("Hacker X", 8, function()
		Run([[loadstring(game:HttpGet("https://raw.githubusercontent.com/Jskfhggjxu/My-Script/refs/heads/main/ServerHacker-X.lua"))()]])
	end)

	CreateButton("AK47", 9, function()
		Run([[loadstring(game:HttpGet("https://raw.githubusercontent.com/sinret/rbxscript.com-scripts-reuploads-/main/ak47", true))()]])
	end)
end

--// GUI
local function ShowGui()
	ClearContent()

	CreateButton("007N7 FE", 1, function()
		Run([[loadstring(game:HttpGet("https://pastebin.com/raw/5WhyACnk"))()]])
	end)

	CreateButton("C00lgui Mini", 2, function()
		Run([[loadstring(game:HttpGet("https://pastefy.app/aubNKtUl/raw"))()]])
	end)

	CreateButton("C00lking Ejector", 3, function()
		Run([[loadstring(game:HttpGet("https://raw.githubusercontent.com/rtlsmonk/c00lgui-made-by-rtls_a1-on-discord/refs/heads/main/have-fun"))()]])
	end)

	CreateButton("Kita Clan", 4, function()
		Run([[loadstring(game:HttpGet("https://pastebin.com/raw/ex7KvLZD"))()]])
		print("Key: HEYINEEDTHEKEYMAN")
	end)

	CreateButton("C00lgui V2", 5, function()
		Run([[loadstring(game:HttpGet("https://raw.githubusercontent.com/sinret/rbxscript.com-scripts-reuploads-/main/ckid", true))()]])
	end)

	CreateButton("V3 NOOB666", 6, function()
		Run([[loadstring(game:HttpGet("https://pastebin.com/raw/pzx8FHa6"))()]])
	end)

	CreateButton("C00lgui Fake", 7, function()
		Run([[loadstring(game:HttpGet("https://rawscripts.net/raw/Universal-Script-c00lgui-Reborn-Rc7-by-v3rx-79664"))()]])
	end)

	CreateButton("Akira Hub", 8, function()
		Run([[loadstring(game:HttpGet("https://raw.githubusercontent.com/saxenaakira3-sudo/Akira-hub-Troll-experience-/refs/heads/main/Akira%20hub%20v10%20primary"))()]])
	end)
end

--// Trocar abas
R15Tab.MouseButton1Click:Connect(ShowR15)
R6Tab.MouseButton1Click:Connect(ShowR6)
GuiTab.MouseButton1Click:Connect(ShowGui)

--// Abrir
OpenButton.MouseButton1Click:Connect(function()
	Main.Visible = true
	OpenButton.Visible = false

	Main.Size = UDim2.fromOffset(350, 230)

	TweenService:Create(
		Main,
		TweenInfo.new(0.2, Enum.EasingStyle.Back),
		{Size = UDim2.fromOffset(390, 260)}
	):Play()
end)

--// Fechar
Close.MouseButton1Click:Connect(function()
	Main.Visible = false
	OpenButton.Visible = true
end)

--// Arrastar
local Dragging = false
local DragStart
local StartPosition

Title.InputBegan:Connect(function(Input)
	if Input.UserInputType == Enum.UserInputType.MouseButton1
		or Input.UserInputType == Enum.UserInputType.Touch then

		Dragging = true
		DragStart = Input.Position
		StartPosition = Main.Position

		Input.Changed:Connect(function()
			if Input.UserInputState == Enum.UserInputState.End then
				Dragging = false
			end
		end)
	end
end)

Title.InputChanged:Connect(function(Input)
	if Input.UserInputType == Enum.UserInputType.MouseMovement
		or Input.UserInputType == Enum.UserInputType.Touch then

		Input.Changed:Connect(function()
			if Dragging then
				local Delta = Input.Position - DragStart

				Main.Position = UDim2.new(
					StartPosition.X.Scale,
					StartPosition.X.Offset + Delta.X,
					StartPosition.Y.Scale,
					StartPosition.Y.Offset + Delta.Y
				)
			end
		end)
	end
end)

--// Responsividade
local Camera = workspace.CurrentCamera

local function UpdateScale()
	local Viewport = Camera.ViewportSize

	local Scale = math.clamp(
		math.min(Viewport.X / 500, Viewport.Y / 400),
		0.65,
		1
	)

	Main.Size = UDim2.fromOffset(
		390 * Scale,
		260 * Scale
	)
end

Camera:GetPropertyChangedSignal("ViewportSize"):Connect(UpdateScale)
UpdateScale()

--// Aba inicial
ShowR15()

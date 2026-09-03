--// ADMIN PANEL CLIENT
--// Put this LocalScript in StarterPlayerScripts

local Players = game:GetService("Players")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local TweenService = game:GetService("TweenService")
local UserInputService = game:GetService("UserInputService")

local player = Players.LocalPlayer
local playerGui = player:WaitForChild("PlayerGui")

--==================================================
-- REMOTES
--==================================================

local AdminRemotes = ReplicatedStorage:WaitForChild("AdminRemotes")
local Command = AdminRemotes:WaitForChild("Command")
local UI = AdminRemotes:WaitForChild("UI")

local OtherRemotes = ReplicatedStorage:WaitForChild("OtherRemotes")
local ActionAnnouncement = OtherRemotes:WaitForChild("ActionAnnouncement")

-- Poll system
local FirePollRemote = OtherRemotes:WaitForChild("FirePoll")
local EndPollRemote = OtherRemotes:WaitForChild("EndPoll")
local VotePollRemote = OtherRemotes:WaitForChild("VotePoll")

-- Moderation system
local KickPlayerRemote = OtherRemotes:WaitForChild("KickPlayer")
local BanPlayerRemote = OtherRemotes:WaitForChild("BanPlayer")
local UnbanPlayerRemote = OtherRemotes:WaitForChild("UnbanPlayer")
local GetBanListRemote = OtherRemotes:WaitForChild("GetBanList")
local GetModLogRemote = OtherRemotes:FindFirstChild("GetModLog")

--==================================================
-- REMOVE OLD PANEL
--==================================================

local oldGui = playerGui:FindFirstChild("AdminPanel")

if oldGui then
	oldGui:Destroy()
end

--==================================================
-- COLORS
--==================================================

local BG = Color3.fromRGB(13, 15, 19)
local PANEL = Color3.fromRGB(20, 23, 29)
local PANEL2 = Color3.fromRGB(25, 29, 36)
local BUTTON = Color3.fromRGB(31, 36, 44)
local BUTTON_HOVER = Color3.fromRGB(40, 47, 57)

local WHITE = Color3.fromRGB(240, 243, 248)
local GRAY = Color3.fromRGB(145, 153, 165)
local CYAN = Color3.fromRGB(55, 210, 255)

local GREEN = Color3.fromRGB(70, 220, 105)
local GOLD = Color3.fromRGB(255, 195, 45)
local RED = Color3.fromRGB(245, 65, 65)

--==================================================
-- HELPERS
--==================================================

local function corner(obj, radius)
	local c = Instance.new("UICorner")
	c.CornerRadius = UDim.new(0, radius or 8)
	c.Parent = obj
	return c
end

local function stroke(obj, color, thickness, transparency)
	local s = Instance.new("UIStroke")
	s.Color = color or Color3.fromRGB(50, 55, 65)
	s.Thickness = thickness or 1
	s.Transparency = transparency or 0
	s.Parent = obj
	return s
end

local function createText(parent, text, size, color)
	local label = Instance.new("TextLabel")
	label.BackgroundTransparency = 1
	label.Text = text or ""
	label.TextColor3 = color or WHITE
	label.TextSize = size or 14
	label.Font = Enum.Font.GothamSemibold
	label.TextXAlignment = Enum.TextXAlignment.Left
	label.Parent = parent
	return label
end

local function makeButton(parent, text)
	local button = Instance.new("TextButton")
	button.AutoButtonColor = false
	button.BackgroundColor3 = BUTTON
	button.Text = text
	button.TextColor3 = WHITE
	button.TextSize = 13
	button.Font = Enum.Font.GothamBold
	button.BorderSizePixel = 0
	button.Parent = parent

	corner(button, 8)

	button.MouseEnter:Connect(function()
		TweenService:Create(
			button,
			TweenInfo.new(0.12),
			{BackgroundColor3 = BUTTON_HOVER}
		):Play()
	end)

	button.MouseLeave:Connect(function()
		TweenService:Create(
			button,
			TweenInfo.new(0.12),
			{BackgroundColor3 = BUTTON}
		):Play()
	end)

	return button
end

local function makeTextBox(parent, placeholder)
	local box = Instance.new("TextBox")
	box.BackgroundColor3 = BUTTON
	box.BorderSizePixel = 0
	box.TextColor3 = WHITE
	box.PlaceholderColor3 = GRAY
	box.PlaceholderText = placeholder
	box.TextSize = 13
	box.Font = Enum.Font.Gotham
	box.ClearTextOnFocus = false
	box.Parent = parent

	corner(box, 8)
	stroke(box, Color3.fromRGB(48, 54, 64), 1, 0.2)

	return box
end

--==================================================
-- GUI
--==================================================

local gui = Instance.new("ScreenGui")
gui.Name = "AdminPanel"
gui.ResetOnSpawn = false
gui.IgnoreGuiInset = true
gui.Enabled = false
gui.Parent = playerGui

--==================================================
-- GEAR BUTTON
--==================================================

local openButton = Instance.new("TextButton")
openButton.Name = "OpenButton"
openButton.Size = UDim2.fromOffset(48, 48)
openButton.Position = UDim2.new(1, -65, 0, 18)
openButton.BackgroundColor3 = PANEL
openButton.Text = "⚙"
openButton.TextColor3 = WHITE
openButton.TextSize = 25
openButton.Font = Enum.Font.GothamBold
openButton.BorderSizePixel = 0
openButton.Visible = false
openButton.Parent = gui

corner(openButton, 12)
stroke(openButton, CYAN, 1, 0.35)

--==================================================
-- MAIN PANEL
--==================================================

local panel = Instance.new("Frame")
panel.Name = "MainPanel"
panel.Size = UDim2.fromOffset(650, 440)
panel.Position = UDim2.new(0.5, -325, 0.5, -220)
panel.BackgroundColor3 = PANEL
panel.BorderSizePixel = 0
panel.Visible = false
panel.Parent = gui

corner(panel, 14)
stroke(panel, Color3.fromRGB(55, 63, 75), 1, 0.15)

--==================================================
-- HEADER
--==================================================

local header = Instance.new("Frame")
header.Size = UDim2.new(1, 0, 0, 58)
header.BackgroundColor3 = PANEL2
header.BorderSizePixel = 0
header.Parent = panel

corner(header, 14)

local headerFix = Instance.new("Frame")
headerFix.Size = UDim2.new(1, 0, 0, 18)
headerFix.Position = UDim2.new(0, 0, 1, -18)
headerFix.BackgroundColor3 = PANEL2
headerFix.BorderSizePixel = 0
headerFix.Parent = header

local title = createText(header, "ADMIN PANEL", 19, WHITE)
title.Position = UDim2.fromOffset(18, 8)
title.Size = UDim2.new(1, -100, 0, 25)

local subtitle = createText(header, "SERVER MANAGEMENT", 10, GRAY)
subtitle.Position = UDim2.fromOffset(19, 32)
subtitle.Size = UDim2.new(1, -100, 0, 16)

local closeButton = Instance.new("TextButton")
closeButton.Size = UDim2.fromOffset(34, 34)
closeButton.Position = UDim2.new(1, -45, 0, 12)
closeButton.BackgroundTransparency = 1
closeButton.Text = "×"
closeButton.TextColor3 = GRAY
closeButton.TextSize = 26
closeButton.Font = Enum.Font.GothamBold
closeButton.Parent = header

--==================================================
-- TABS
--==================================================

local tabBar = Instance.new("Frame")
tabBar.Size = UDim2.new(1, -28, 0, 40)
tabBar.Position = UDim2.fromOffset(14, 66)
tabBar.BackgroundTransparency = 1
tabBar.Parent = panel

local tabLayout = Instance.new("UIListLayout")
tabLayout.FillDirection = Enum.FillDirection.Horizontal
tabLayout.Padding = UDim.new(0, 5)
tabLayout.SortOrder = Enum.SortOrder.LayoutOrder
tabLayout.Parent = tabBar

local tabs = {}

local function createTab(name)
	local tab = Instance.new("TextButton")
	tab.Size = UDim2.fromOffset(99, 36)
	tab.BackgroundColor3 = BUTTON
	tab.Text = name
	tab.TextColor3 = GRAY
	tab.TextSize = 11
	tab.Font = Enum.Font.GothamBold
	tab.BorderSizePixel = 0
	tab.AutoButtonColor = false
	tab.Parent = tabBar

	corner(tab, 7)

	table.insert(tabs, tab)

	return tab
end

local luckTab = createTab("LUCK")
local spawnerTab = createTab("SPAWNER")
local playersTab = createTab("PLAYERS")
local moderateTab = createTab("MODERATE")
local pollsTab = createTab("POLLS")
local announceTab = createTab("ANNOUNCE")

--==================================================
-- CONTENT
--==================================================

local content = Instance.new("Frame")
content.Size = UDim2.new(1, -28, 1, -120)
content.Position = UDim2.fromOffset(14, 112)
content.BackgroundTransparency = 1
content.Parent = panel

local currentPage

local function clearPage()
	if currentPage then
		currentPage:Destroy()
	end

	currentPage = Instance.new("Frame")
	currentPage.Size = UDim2.fromScale(1, 1)
	currentPage.BackgroundTransparency = 1
	currentPage.Parent = content
end

--==================================================
-- CLOVER ICON
-- NO EMOJI
-- NO BLACK OUTLINE
--==================================================

local function createClover(parent, color, size)
	local holder = Instance.new("Frame")
	holder.Size = UDim2.fromOffset(size, size)
	holder.BackgroundTransparency = 1
	holder.Parent = parent

	local function circle(x, y)
		local c = Instance.new("Frame")
		c.Size = UDim2.fromOffset(size * 0.43, size * 0.43)
		c.Position = UDim2.new(x, 0, y, 0)
		c.BackgroundColor3 = color
		c.BorderSizePixel = 0
		c.Parent = holder

		local co = Instance.new("UICorner")
		co.CornerRadius = UDim.new(1, 0)
		co.Parent = c

		return c
	end

	-- Four circles making a clover.
	circle(0.03, 0.03)
	circle(0.54, 0.03)
	circle(0.03, 0.54)
	circle(0.54, 0.54)

	-- Small center connection.
	local center = Instance.new("Frame")
	center.Size = UDim2.fromOffset(size * 0.22, size * 0.22)
	center.Position = UDim2.new(0.39, 0, 0.39, 0)
	center.BackgroundColor3 = color
	center.BorderSizePixel = 0
	center.Parent = holder

	local centerCorner = Instance.new("UICorner")
	centerCorner.CornerRadius = UDim.new(1, 0)
	centerCorner.Parent = center

	-- Small stem.
	local stem = Instance.new("Frame")
	stem.Size = UDim2.fromOffset(size * 0.13, size * 0.27)
	stem.Position = UDim2.new(0.44, 0, 0.70, 0)
	stem.Rotation = 20
	stem.BackgroundColor3 = color
	stem.BorderSizePixel = 0
	stem.Parent = holder

	corner(stem, 4)

	return holder
end

--==================================================
-- LUCK PAGE
--==================================================

local selectedScope = "Server"
local selectedLuck = nil

local function showLuckPage()
	clearPage()

	local page = currentPage

	local heading = createText(page, "SERVER LUCK", 18, WHITE)
	heading.Position = UDim2.fromOffset(4, 2)
	heading.Size = UDim2.new(1, 0, 0, 25)

	local desc = createText(
		page,
		"Increase the chance of better Brainrots spawning.",
		11,
		GRAY
	)
	desc.Position = UDim2.fromOffset(4, 27)
	desc.Size = UDim2.new(1, 0, 0, 20)

	-- Scope
	local scopeLabel = createText(page, "SCOPE", 11, GRAY)
	scopeLabel.Position = UDim2.fromOffset(4, 55)
	scopeLabel.Size = UDim2.fromOffset(100, 20)

	local serverButton = makeButton(page, "SERVER")
	serverButton.Size = UDim2.fromOffset(105, 34)
	serverButton.Position = UDim2.fromOffset(4, 79)

	local globalButton = makeButton(page, "GLOBAL")
	globalButton.Size = UDim2.fromOffset(105, 34)
	globalButton.Position = UDim2.fromOffset(115, 79)

	local function updateScope()
		if selectedScope == "Server" then
			serverButton.BackgroundColor3 = CYAN
			serverButton.TextColor3 = Color3.fromRGB(5, 10, 14)

			globalButton.BackgroundColor3 = BUTTON
			globalButton.TextColor3 = WHITE
		else
			globalButton.BackgroundColor3 = CYAN
			globalButton.TextColor3 = Color3.fromRGB(5, 10, 14)

			serverButton.BackgroundColor3 = BUTTON
			serverButton.TextColor3 = WHITE
		end
	end

	serverButton.MouseButton1Click:Connect(function()
		selectedScope = "Server"
		updateScope()
	end)

	globalButton.MouseButton1Click:Connect(function()
		selectedScope = "Global"
		updateScope()
	end)

	updateScope()

	-- Luck cards
	local cards = {
		{
			multiplier = 2,
			duration = 15 * 60,
			color = GREEN,
			name = "2X LUCK",
			desc = "15 MINUTES"
		},
		{
			multiplier = 4,
			duration = 10 * 60,
			color = GOLD,
			name = "4X LUCK",
			desc = "10 MINUTES"
		},
		{
			multiplier = 8,
			duration = 5 * 60,
			color = RED,
			name = "8X LUCK",
			desc = "5 MINUTES"
		}
	}

	for i, info in ipairs(cards) do
		local card = Instance.new("TextButton")
		card.Size = UDim2.fromOffset(190, 145)
		card.Position = UDim2.fromOffset(4 + ((i - 1) * 202), 130)
		card.BackgroundColor3 = PANEL2
		card.BorderSizePixel = 0
		card.Text = ""
		card.AutoButtonColor = false
		card.Parent = page

		corner(card, 12)
		stroke(card, info.color, 1, 0.45)

		local icon = createClover(card, info.color, 50)
		icon.Position = UDim2.fromOffset(70, 15)

		local multiplier = createText(card, info.name, 15, WHITE)
		multiplier.TextXAlignment = Enum.TextXAlignment.Center
		multiplier.Position = UDim2.fromOffset(5, 72)
		multiplier.Size = UDim2.new(1, -10, 0, 23)

		local duration = createText(card, info.desc, 10, GRAY)
		duration.TextXAlignment = Enum.TextXAlignment.Center
		duration.Position = UDim2.fromOffset(5, 96)
		duration.Size = UDim2.new(1, -10, 0, 18)

		card.MouseEnter:Connect(function()
			TweenService:Create(
				card,
				TweenInfo.new(0.12),
				{BackgroundColor3 = BUTTON_HOVER}
			):Play()
		end)

		card.MouseLeave:Connect(function()
			TweenService:Create(
				card,
				TweenInfo.new(0.12),
				{BackgroundColor3 = PANEL2}
			):Play()
		end)

		card.MouseButton1Click:Connect(function()
			selectedLuck = info.multiplier

			Command:FireServer(
				"ServerLuck",
				info.multiplier,
				info.duration,
				selectedScope
			)
		end)
	end
end

--==================================================
-- SPAWNER PAGE
--==================================================

local brainrots = {
	"TungTungSahur",
	"TralaleroTralala",
	"BombardiroCrocodilo",
	"CappuccinoAssassino",
	"LiriliLarila",
	"BallerinaCappuccina",
	"StudzillaRex",
	"NeonNugget",
	"VoltViper",
	"PrismPouncer",
	"MagmaMuffin",
	"Chromaclaw",
	"TurboToast",
	"GlitterGolem",
	"BoomblockBarry",
	"CascadeCrab"
}

local function showSpawnerPage()
	clearPage()

	local page = currentPage

	local heading = createText(page, "SPAWN BRAINROTS", 18, WHITE)
	heading.Position = UDim2.fromOffset(4, 2)
	heading.Size = UDim2.new(1, 0, 0, 25)

	local scope = "Server"

	local serverButton = makeButton(page, "SERVER")
	serverButton.Size = UDim2.fromOffset(100, 32)
	serverButton.Position = UDim2.fromOffset(4, 38)

	local globalButton = makeButton(page, "GLOBAL")
	globalButton.Size = UDim2.fromOffset(100, 32)
	globalButton.Position = UDim2.fromOffset(110, 38)

	local function update()
		if scope == "Server" then
			serverButton.BackgroundColor3 = CYAN
			serverButton.TextColor3 = Color3.fromRGB(5, 10, 14)

			globalButton.BackgroundColor3 = BUTTON
			globalButton.TextColor3 = WHITE
		else
			globalButton.BackgroundColor3 = CYAN
			globalButton.TextColor3 = Color3.fromRGB(5, 10, 14)

			serverButton.BackgroundColor3 = BUTTON
			serverButton.TextColor3 = WHITE
		end
	end

	serverButton.MouseButton1Click:Connect(function()
		scope = "Server"
		update()
	end)

	globalButton.MouseButton1Click:Connect(function()
		scope = "Global"
		update()
	end)

	update()

	local scroll = Instance.new("ScrollingFrame")
	scroll.Size = UDim2.new(1, -8, 1, -82)
	scroll.Position = UDim2.fromOffset(4, 78)
	scroll.BackgroundTransparency = 1
	scroll.BorderSizePixel = 0
	scroll.ScrollBarThickness = 4
	scroll.CanvasSize = UDim2.new()
	scroll.Parent = page

	local layout = Instance.new("UIGridLayout")
	layout.CellSize = UDim2.fromOffset(145, 42)
	layout.CellPadding = UDim2.fromOffset(8, 8)
	layout.Parent = scroll

	for _, brainrotId in ipairs(brainrots) do
		local button = makeButton(scroll, brainrotId)

		button.MouseButton1Click:Connect(function()
			Command:FireServer(
				"SpawnBrainrot",
				brainrotId,
				scope
			)
		end)
	end

	layout:GetPropertyChangedSignal("AbsoluteContentSize"):Connect(function()
		scroll.CanvasSize = UDim2.fromOffset(
			0,
			layout.AbsoluteContentSize.Y + 10
		)
	end)
end

--==================================================
-- PLAYERS / GIVE BRAINROT + MONEY
--==================================================

local function showPlayersPage()
	clearPage()

	local page = currentPage

	local heading = createText(page, "PLAYERS", 18, WHITE)
	heading.Position = UDim2.fromOffset(4, 2)
	heading.Size = UDim2.new(1, 0, 0, 25)

	-- Give Brainrot
	local brainrotTitle = createText(page, "GIVE BRAINROT", 12, GRAY)
	brainrotTitle.Position = UDim2.fromOffset(4, 40)
	brainrotTitle.Size = UDim2.fromOffset(200, 20)

	local username = makeTextBox(page, "Username")
	username.Size = UDim2.fromOffset(250, 38)
	username.Position = UDim2.fromOffset(4, 65)

	local brainrotBox = makeTextBox(page, "Brainrot ID")
	brainrotBox.Size = UDim2.fromOffset(250, 38)
	brainrotBox.Position = UDim2.fromOffset(4, 110)

	local giveBrainrot = makeButton(page, "GIVE BRAINROT")
	giveBrainrot.Size = UDim2.fromOffset(160, 38)
	giveBrainrot.Position = UDim2.fromOffset(4, 155)

	giveBrainrot.MouseButton1Click:Connect(function()
		if username.Text == "" or brainrotBox.Text == "" then
			return
		end

		Command:FireServer(
			"GiveBrainrot",
			username.Text,
			brainrotBox.Text
		)
	end)

	-- Give Money
	local moneyTitle = createText(page, "GIVE MONEY", 12, GRAY)
	moneyTitle.Position = UDim2.fromOffset(320, 40)
	moneyTitle.Size = UDim2.fromOffset(200, 20)

	local moneyUser = makeTextBox(page, "Username")
	moneyUser.Size = UDim2.fromOffset(250, 38)
	moneyUser.Position = UDim2.fromOffset(320, 65)

	local moneyAmount = makeTextBox(page, "Amount")
	moneyAmount.Size = UDim2.fromOffset(250, 38)
	moneyAmount.Position = UDim2.fromOffset(320, 110)

	local moneyScope = "Server"

	local moneyServer = makeButton(page, "SERVER")
	moneyServer.Size = UDim2.fromOffset(120, 34)
	moneyServer.Position = UDim2.fromOffset(320, 155)

	local moneyGlobal = makeButton(page, "GLOBAL")
	moneyGlobal.Size = UDim2.fromOffset(120, 34)
	moneyGlobal.Position = UDim2.fromOffset(446, 155)

	local function updateMoneyScope()
		if moneyScope == "Server" then
			moneyServer.BackgroundColor3 = CYAN
			moneyServer.TextColor3 = Color3.fromRGB(5, 10, 14)

			moneyGlobal.BackgroundColor3 = BUTTON
			moneyGlobal.TextColor3 = WHITE
		else
			moneyGlobal.BackgroundColor3 = CYAN
			moneyGlobal.TextColor3 = Color3.fromRGB(5, 10, 14)

			moneyServer.BackgroundColor3 = BUTTON
			moneyServer.TextColor3 = WHITE
		end
	end

	moneyServer.MouseButton1Click:Connect(function()
		moneyScope = "Server"
		updateMoneyScope()
	end)

	moneyGlobal.MouseButton1Click:Connect(function()
		moneyScope = "Global"
		updateMoneyScope()
	end)

	updateMoneyScope()

	local giveMoney = makeButton(page, "GIVE MONEY")
	giveMoney.Size = UDim2.fromOffset(160, 38)
	giveMoney.Position = UDim2.fromOffset(320, 200)

	giveMoney.MouseButton1Click:Connect(function()
		local amount = tonumber(moneyAmount.Text)

		if moneyUser.Text == "" or not amount then
			return
		end

		Command:FireServer(
			"GiveMoney",
			moneyUser.Text,
			amount,
			moneyScope
		)
	end)
end

--==================================================
-- MODERATION PAGE
-- USES EXISTING MODERATION SYSTEM
--==================================================

local function showModeratePage()
	clearPage()

	local page = currentPage

	local heading = createText(page, "MODERATION", 18, WHITE)
	heading.Position = UDim2.fromOffset(4, 2)
	heading.Size = UDim2.new(1, 0, 0, 25)

	local username = makeTextBox(page, "Username")
	username.Size = UDim2.fromOffset(280, 38)
	username.Position = UDim2.fromOffset(4, 42)

	local reason = makeTextBox(page, "Reason")
	reason.Size = UDim2.fromOffset(280, 38)
	reason.Position = UDim2.fromOffset(4, 88)

	local kick = makeButton(page, "KICK")
	kick.Size = UDim2.fromOffset(130, 38)
	kick.Position = UDim2.fromOffset(4, 136)

	local ban = makeButton(page, "BAN")
	ban.Size = UDim2.fromOffset(130, 38)
	ban.Position = UDim2.fromOffset(144, 136)

	local unban = makeButton(page, "UNBAN")
	unban.Size = UDim2.fromOffset(130, 38)
	unban.Position = UDim2.fromOffset(284, 136)

	-- Existing ban duration value
	local banDuration = 0

	local durationLabel = createText(page, "BAN DURATION", 10, GRAY)
	durationLabel.Position = UDim2.fromOffset(320, 42)
	durationLabel.Size = UDim2.fromOffset(150, 20)

	local durationBox = makeTextBox(page, "Seconds (0 = permanent)")
	durationBox.Size = UDim2.fromOffset(280, 38)
	durationBox.Position = UDim2.fromOffset(320, 65)

	durationBox.FocusLost:Connect(function()
		local value = tonumber(durationBox.Text)

		if value then
			banDuration = value
		else
			banDuration = 0
		end
	end)

	kick.MouseButton1Click:Connect(function()
		if username.Text == "" then
			return
		end

		KickPlayerRemote:InvokeServer(
			username.Text,
			reason.Text
		)
	end)

	ban.MouseButton1Click:Connect(function()
		if username.Text == "" then
			return
		end

		BanPlayerRemote:InvokeServer(
			username.Text,
			reason.Text,
			banDuration
		)
	end)

	unban.MouseButton1Click:Connect(function()
		if username.Text == "" then
			return
		end

		UnbanPlayerRemote:InvokeServer(
			username.Text
		)
	end)

	-- Existing ban list
	local banListButton = makeButton(page, "GET BAN LIST")
	banListButton.Size = UDim2.fromOffset(150, 36)
	banListButton.Position = UDim2.fromOffset(4, 195)

	banListButton.MouseButton1Click:Connect(function()
		local success, result = pcall(function()
			return GetBanListRemote:InvokeServer()
		end)

		if success then
			print("BAN LIST:", result)
		end
	end)

	-- Existing mod log if available
	if GetModLogRemote then
		local logButton = makeButton(page, "GET MOD LOG")
		logButton.Size = UDim2.fromOffset(150, 36)
		logButton.Position = UDim2.fromOffset(162, 195)

		logButton.MouseButton1Click:Connect(function()
			local success, result = pcall(function()
				return GetModLogRemote:InvokeServer()
			end)

			if success then
				print("MOD LOG:", result)
			end
		end)
	end
end

--==================================================
-- POLL PAGE
-- USES EXISTING POLL SYSTEM
--==================================================

local function showPollPage()
	clearPage()

	local page = currentPage

	local heading = createText(page, "POLLS", 18, WHITE)
	heading.Position = UDim2.fromOffset(4, 2)
	heading.Size = UDim2.new(1, 0, 0, 25)

	local question = makeTextBox(page, "Question")
	question.Size = UDim2.fromOffset(590, 38)
	question.Position = UDim2.fromOffset(4, 42)

	local duration = makeTextBox(page, "Duration")
	duration.Size = UDim2.fromOffset(180, 38)
	duration.Position = UDim2.fromOffset(4, 88)

	local button1 = makeTextBox(page, "Answer 1")
	button1.Size = UDim2.fromOffset(190, 38)
	button1.Position = UDim2.fromOffset(4, 134)

	local button2 = makeTextBox(page, "Answer 2")
	button2.Size = UDim2.fromOffset(190, 38)
	button2.Position = UDim2.fromOffset(202, 134)

	local startPoll = makeButton(page, "START POLL")
	startPoll.Size = UDim2.fromOffset(150, 40)
	startPoll.Position = UDim2.fromOffset(4, 185)

	local endPoll = makeButton(page, "END POLL")
	endPoll.Size = UDim2.fromOffset(150, 40)
	endPoll.Position = UDim2.fromOffset(162, 185)

	startPoll.MouseButton1Click:Connect(function()
		local q = question.Text
		local d = tonumber(duration.Text) or 30
		local b1 = button1.Text
		local b2 = button2.Text

		if q == "" or b1 == "" or b2 == "" then
			return
		end

		FirePollRemote:FireServer(
			q,
			d,
			b1,
			b2
		)
	end)

	endPoll.MouseButton1Click:Connect(function()
		EndPollRemote:FireServer()
	end)
end

--==================================================
-- ANNOUNCEMENT PAGE
--==================================================

local function showAnnouncePage()
	clearPage()

	local page = currentPage

	local heading = createText(page, "ANNOUNCEMENT", 18, WHITE)
	heading.Position = UDim2.fromOffset(4, 2)
	heading.Size = UDim2.new(1, 0, 0, 25)

	local message = makeTextBox(page, "Announcement message...")
	message.Size = UDim2.fromOffset(590, 100)
	message.Position = UDim2.fromOffset(4, 45)
	message.TextWrapped = true
	message.TextYAlignment = Enum.TextYAlignment.Top

	local send = makeButton(page, "SEND ANNOUNCEMENT")
	send.Size = UDim2.fromOffset(190, 40)
	send.Position = UDim2.fromOffset(4, 155)

	send.MouseButton1Click:Connect(function()
		if message.Text == "" then
			return
		end

		ActionAnnouncement:FireServer(message.Text)
	end)
end

--==================================================
-- TAB SYSTEM
--==================================================

local function selectTab(selected)
	for _, tab in ipairs(tabs) do
		if tab == selected then
			tab.BackgroundColor3 = CYAN
			tab.TextColor3 = Color3.fromRGB(5, 10, 14)
		else
			tab.BackgroundColor3 = BUTTON
			tab.TextColor3 = GRAY
		end
	end
end

local function selectLuck()
	selectTab(luckTab)
	showLuckPage()
end

local function selectSpawner()
	selectTab(spawnerTab)
	showSpawnerPage()
end

local function selectPlayers()
	selectTab(playersTab)
	showPlayersPage()
end

local function selectModerate()
	selectTab(moderateTab)
	showModeratePage()
end

local function selectPolls()
	selectTab(pollsTab)
	showPollPage()
end

local function selectAnnounce()
	selectTab(announceTab)
	showAnnouncePage()
end

luckTab.MouseButton1Click:Connect(selectLuck)
spawnerTab.MouseButton1Click:Connect(selectSpawner)
playersTab.MouseButton1Click:Connect(selectPlayers)
moderateTab.MouseButton1Click:Connect(selectModerate)
pollsTab.MouseButton1Click:Connect(selectPolls)
announceTab.MouseButton1Click:Connect(selectAnnounce)

--==================================================
-- OPEN / CLOSE
--==================================================

local function openPanel()
	panel.Visible = true

	-- Always open at the full size.
	panel.Size = UDim2.fromOffset(650, 440)

	selectTab(luckTab)
	showLuckPage()
end

local function closePanel()
	panel.Visible = false
end

openButton.MouseButton1Click:Connect(function()
	if panel.Visible then
		closePanel()
	else
		openPanel()
	end
end)

closeButton.MouseButton1Click:Connect(closePanel)

--==================================================
-- E KEY
--==================================================

UserInputService.InputBegan:Connect(function(input, processed)
	if processed then
		return
	end

	if input.KeyCode == Enum.KeyCode.E then
		if not gui.Enabled then
			return
		end

		if panel.Visible then
			closePanel()
		else
			openPanel()
		end
	end
end)

--==================================================
-- EXISTING POLL DISPLAY
--==================================================

FirePollRemote.OnClientEvent:Connect(function(
	questionText,
	pollDuration,
	pollButton1,
	pollButton2,
	votes1,
	votes2
)
	print(
		"Poll:",
		questionText,
		pollDuration,
		pollButton1,
		pollButton2,
		votes1,
		votes2
	)
end)

EndPollRemote.OnClientEvent:Connect(function(votes1, votes2)
	print("Poll ended:", votes1, votes2)
end)

-- Existing vote system can still fire these.
local function vote1()
	VotePollRemote:FireServer(1)
end

local function vote2()
	VotePollRemote:FireServer(2)
end

--==================================================
-- ADMIN SERVER RESPONSES
--==================================================

UI.OnClientEvent:Connect(function(action, data)

	if action == "Authorized" then
		gui.Enabled = true
		openButton.Visible = true

	elseif action == "Error" then
		warn("[ADMIN PANEL]", data)

	elseif action == "Success" then
		print("[ADMIN PANEL]", data)

	elseif action == "Luck" then
		print(
			"Luck activated:",
			data.Multiplier,
			data.Duration,
			data.Scope
		)

	elseif action == "StopLuck" then
		print("Luck stopped")

	elseif action == "Announcement" then
		print("Announcement:", data)
	end
end)

--==================================================
-- EXISTING ACTION ANNOUNCEMENTS
--==================================================

ActionAnnouncement.OnClientEvent:Connect(function(message)
	print("Announcement:", message)
end)

--==================================================
-- CHECK ADMIN
--==================================================

task.wait(1)

Command:FireServer("CheckAdmin")

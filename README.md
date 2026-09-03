--==================================================
-- FIGHT FOR BRAINROTS
-- ADMIN PANEL CLIENT
--==================================================

local Players = game:GetService("Players")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local UserInputService = game:GetService("UserInputService")
local TweenService = game:GetService("TweenService")

local player = Players.LocalPlayer

--==================================================
-- ADMIN IDS
--==================================================

local ADMINS = {
	[11607704974] = true,

	-- Add more admins here:
	-- [123456789] = true,
}

local IS_ADMIN = ADMINS[player.UserId] == true

if not IS_ADMIN then
	return
end

--==================================================
-- ADMIN REMOTES
--==================================================

local AdminRemotes = ReplicatedStorage:WaitForChild("AdminRemotes")

local Command = AdminRemotes:WaitForChild("Command")
local UI = AdminRemotes:WaitForChild("UI")

--==================================================
-- OTHER REMOTES
--==================================================

local OtherRemotes = ReplicatedStorage:FindFirstChild("OtherRemotes")

local ActionAnnouncement

if OtherRemotes then
	ActionAnnouncement =
		OtherRemotes:FindFirstChild("ActionAnnouncement")
end

--==================================================
-- COLORS
--==================================================

local CYAN = Color3.fromRGB(0, 255, 220)

local DARK = Color3.fromRGB(7, 15, 16)
local DARK2 = Color3.fromRGB(11, 24, 25)
local DARK3 = Color3.fromRGB(17, 35, 35)
local DARK4 = Color3.fromRGB(23, 46, 45)

local WHITE = Color3.fromRGB(245, 255, 252)
local GRAY = Color3.fromRGB(145, 170, 165)

local GREEN = Color3.fromRGB(45, 220, 90)
local GOLD = Color3.fromRGB(255, 200, 45)
local RED = Color3.fromRGB(235, 60, 60)
local PURPLE = Color3.fromRGB(155, 80, 255)

--==================================================
-- SCREEN GUI
--==================================================

local gui = Instance.new("ScreenGui")
gui.Name = "FightForBrainrotsAdmin"
gui.ResetOnSpawn = false
gui.IgnoreGuiInset = true
gui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
gui.Parent = player:WaitForChild("PlayerGui")

--==================================================
-- SOUND
--==================================================

local clickSound = Instance.new("Sound")
clickSound.Name = "ClickSound"
clickSound.SoundId = "rbxasset://sounds/electronicpingshort.wav"
clickSound.Volume = 0.35
clickSound.Parent = gui

local function playClick()
	pcall(function()
		clickSound:Play()
	end)
end

--==================================================
-- HELPERS
--==================================================

local function addCorner(object, radius)

	local corner = Instance.new("UICorner")
	corner.CornerRadius = UDim.new(0, radius or 7)
	corner.Parent = object

	return corner
end

local function addNoOutline(object)

	object.BorderSizePixel = 0

end

local function tween(object, info, properties)

	return TweenService:Create(
		object,
		info,
		properties
	)

end

--==================================================
-- OPEN BUTTON
--==================================================

local openButton = Instance.new("TextButton")

openButton.Name = "OpenButton"

openButton.AnchorPoint =
	Vector2.new(1, 0)

openButton.Position =
	UDim2.new(1, -18, 0, 18)

openButton.Size =
	UDim2.fromOffset(44, 44)

openButton.BackgroundColor3 = DARK2
openButton.BackgroundTransparency = 0.05

-- Gear
openButton.Text = "⚙"
openButton.TextColor3 = CYAN
openButton.TextSize = 25
openButton.Font = Enum.Font.GothamBold

openButton.AutoButtonColor = false

addNoOutline(openButton)
addCorner(openButton, 9)

openButton.Parent = gui

local openGradient = Instance.new("UIGradient")

openGradient.Rotation = 90

openGradient.Color =
	ColorSequence.new({
		ColorSequenceKeypoint.new(
			0,
			DARK4
		),

		ColorSequenceKeypoint.new(
			1,
			DARK2
		)
	})

openGradient.Parent = openButton

openButton.MouseEnter:Connect(function()

	tween(
		openButton,
		TweenInfo.new(0.12),
		{
			BackgroundColor3 = DARK4,
			TextColor3 = WHITE
		}
	):Play()

end)

openButton.MouseLeave:Connect(function()

	tween(
		openButton,
		TweenInfo.new(0.12),
		{
			BackgroundColor3 = DARK2,
			TextColor3 = CYAN
		}
	):Play()

end)

--==================================================
-- MAIN PANEL
--==================================================

local panel = Instance.new("Frame")

panel.Name = "MainPanel"

panel.AnchorPoint =
	Vector2.new(0.5, 0.5)

panel.Position =
	UDim2.fromScale(0.5, 0.5)

panel.Size =
	UDim2.fromOffset(560, 365)

panel.BackgroundColor3 = DARK
panel.BackgroundTransparency = 0.02

panel.Visible = false

addNoOutline(panel)
addCorner(panel, 10)

panel.Parent = gui

--==================================================
-- HEADER
--==================================================

local header = Instance.new("Frame")

header.Size =
	UDim2.new(1, 0, 0, 48)

header.BackgroundColor3 = CYAN

addNoOutline(header)
addCorner(header, 10)

header.Parent = panel

local headerBottom = Instance.new("Frame")

headerBottom.Position =
	UDim2.new(0, 0, 1, -10)

headerBottom.Size =
	UDim2.new(1, 0, 0, 10)

headerBottom.BackgroundColor3 = CYAN

addNoOutline(headerBottom)

headerBottom.Parent = header

local title = Instance.new("TextLabel")

title.BackgroundTransparency = 1

title.Position =
	UDim2.fromOffset(15, 0)

title.Size =
	UDim2.new(1, -65, 1, 0)

title.Text = "ADMIN TERMINAL"

title.TextColor3 =
	Color3.fromRGB(4, 28, 27)

title.TextSize = 18
title.Font = Enum.Font.GothamBold

title.TextXAlignment =
	Enum.TextXAlignment.Left

title.Parent = header

local subtitle = Instance.new("TextLabel")

subtitle.BackgroundTransparency = 1

subtitle.Position =
	UDim2.new(1, -205, 0, 0)

subtitle.Size =
	UDim2.fromOffset(155, 48)

subtitle.Text = "FIGHT FOR BRAINROTS"

subtitle.TextColor3 =
	Color3.fromRGB(5, 55, 51)

subtitle.TextSize = 8
subtitle.Font = Enum.Font.GothamBold

subtitle.TextXAlignment =
	Enum.TextXAlignment.Right

subtitle.Parent = header

--==================================================
-- CLOSE BUTTON
--==================================================

local close = Instance.new("TextButton")

close.Size =
	UDim2.fromOffset(34, 34)

close.Position =
	UDim2.new(1, -41, 0, 7)

close.BackgroundColor3 = DARK2

close.Text = "×"

close.TextColor3 = CYAN
close.TextSize = 22
close.Font = Enum.Font.GothamBold

close.AutoButtonColor = false

addNoOutline(close)
addCorner(close, 7)

close.Parent = header

--==================================================
-- CONTENT
--==================================================

local content = Instance.new("Frame")

content.Position =
	UDim2.fromOffset(10, 58)

content.Size =
	UDim2.new(1, -20, 1, -68)

content.BackgroundTransparency = 1

content.Parent = panel

--==================================================
-- TABS
--==================================================

local tabHolder = Instance.new("Frame")

tabHolder.Size =
	UDim2.new(1, 0, 0, 42)

tabHolder.BackgroundTransparency = 1

tabHolder.Parent = content

local tabLayout = Instance.new("UIListLayout")

tabLayout.FillDirection =
	Enum.FillDirection.Horizontal

tabLayout.Padding =
	UDim.new(0, 5)

tabLayout.HorizontalAlignment =
	Enum.HorizontalAlignment.Center

tabLayout.VerticalAlignment =
	Enum.VerticalAlignment.Center

tabLayout.Parent = tabHolder

local function makeTab(text)

	local button = Instance.new("TextButton")

	button.Size =
		UDim2.fromOffset(98, 38)

	button.BackgroundColor3 = DARK2

	button.Text = text

	button.TextColor3 = GRAY
	button.TextSize = 10
	button.Font = Enum.Font.GothamBold

	button.AutoButtonColor = false

	addNoOutline(button)
	addCorner(button, 6)

	button.Parent = tabHolder

	return button
end

local luckTab =
	makeTab("LUCK")

local spawnTab =
	makeTab("SPAWNER")

local playerTab =
	makeTab("PLAYERS")

local moderateTab =
	makeTab("MODERATE")

local announcementTab =
	makeTab("ANNOUNCE")

--==================================================
-- PAGE HOLDER
--==================================================

local pageHolder = Instance.new("Frame")

pageHolder.Position =
	UDim2.fromOffset(0, 48)

pageHolder.Size =
	UDim2.new(1, 0, 1, -48)

pageHolder.BackgroundTransparency = 1

pageHolder.Parent = content

local function clearPage()

	for _, child in ipairs(
		pageHolder:GetChildren()
	) do

		child:Destroy()

	end

end

--==================================================
-- UI HELPERS
--==================================================

local function makeLabel(
	parent,
	text,
	position,
	size
)

	local label = Instance.new("TextLabel")

	label.BackgroundTransparency = 1

	label.Position = position
	label.Size = size

	label.Text = text

	label.TextColor3 = CYAN
	label.TextSize = 14
	label.Font = Enum.Font.GothamBold

	label.TextXAlignment =
		Enum.TextXAlignment.Left

	label.Parent = parent

	return label
end

local function makeButton(
	parent,
	text,
	position,
	size,
	color
)

	local button = Instance.new("TextButton")

	button.Position = position
	button.Size = size

	button.BackgroundColor3 =
		color or DARK3

	button.Text = text

	button.TextColor3 = WHITE
	button.TextSize = 13
	button.Font = Enum.Font.GothamBold

	button.AutoButtonColor = false

	addNoOutline(button)
	addCorner(button, 7)

	button.Parent = parent

	button.MouseEnter:Connect(function()

		tween(
			button,
			TweenInfo.new(0.1),
			{
				BackgroundColor3 =
					(color or DARK3):Lerp(
						Color3.new(1, 1, 1),
						0.07
					)
			}
		):Play()

	end)

	button.MouseLeave:Connect(function()

		tween(
			button,
			TweenInfo.new(0.1),
			{
				BackgroundColor3 =
					color or DARK3
			}
		):Play()

	end)

	button.MouseButton1Click:Connect(
		function()
			playClick()
		end
	)

	return button
end

local function makeBox(
	parent,
	placeholder,
	position,
	size
)

	local box = Instance.new("TextBox")

	box.Position = position
	box.Size = size

	box.BackgroundColor3 = DARK2

	box.Text = ""

	box.PlaceholderText =
		placeholder

	box.PlaceholderColor3 =
		GRAY

	box.TextColor3 = WHITE
	box.TextSize = 13
	box.Font = Enum.Font.Gotham

	box.ClearTextOnFocus = false

	addNoOutline(box)
	addCorner(box, 6)

	box.Parent = parent

	return box
end

--==================================================
-- GLOBAL
--==================================================

local globalEnabled = false

--==================================================
-- LUCK HUD
--==================================================

local luckHud = Instance.new("Frame")

luckHud.Name = "LuckHUD"

luckHud.AnchorPoint =
	Vector2.new(1, 1)

luckHud.Position =
	UDim2.new(1, -16, 1, -16)

luckHud.Size =
	UDim2.fromOffset(180, 66)

luckHud.BackgroundColor3 = DARK
luckHud.BackgroundTransparency = 0.02

luckHud.Visible = false

addNoOutline(luckHud)
addCorner(luckHud, 9)

luckHud.Parent = gui

--==================================================
-- CLOVER
--==================================================

local cloverHolder = Instance.new("Frame")

cloverHolder.Name = "Clover"

cloverHolder.BackgroundTransparency = 1

cloverHolder.Position =
	UDim2.fromOffset(8, 9)

cloverHolder.Size =
	UDim2.fromOffset(45, 45)

cloverHolder.Parent = luckHud

-- No UIStroke is used anywhere on the clover.

local cloverLeaves = {}

local function makeLeaf(
	position,
	size
)

	local leaf = Instance.new("Frame")

	leaf.Position = position
	leaf.Size = size

	leaf.BackgroundColor3 = GREEN

	-- Completely remove outline
	leaf.BorderSizePixel = 0

	addCorner(
		leaf,
		math.floor(
			math.min(
				size.X.Offset,
				size.Y.Offset
			) / 2
		)
	)

	leaf.Parent = cloverHolder

	table.insert(
		cloverLeaves,
		leaf
	)

	return leaf
end

-- Four large round leaves
makeLeaf(
	UDim2.fromOffset(10, 0),
	UDim2.fromOffset(22, 22)
)

makeLeaf(
	UDim2.fromOffset(23, 10),
	UDim2.fromOffset(22, 22)
)

makeLeaf(
	UDim2.fromOffset(10, 23),
	UDim2.fromOffset(22, 22)
)

makeLeaf(
	UDim2.fromOffset(-3, 10),
	UDim2.fromOffset(22, 22)
)

-- Small center
local cloverCenter = Instance.new("Frame")

cloverCenter.Position =
	UDim2.fromOffset(14, 14)

cloverCenter.Size =
	UDim2.fromOffset(16, 16)

cloverCenter.BackgroundColor3 =
	GREEN

cloverCenter.BorderSizePixel = 0

addCorner(
	cloverCenter,
	8
)

cloverCenter.Parent =
	cloverHolder

table.insert(
	cloverLeaves,
	cloverCenter
)

-- Small stem
local stem = Instance.new("Frame")

stem.Position =
	UDim2.fromOffset(18, 32)

stem.Size =
	UDim2.fromOffset(6, 13)

stem.BackgroundColor3 =
	GREEN

stem.BorderSizePixel = 0

stem.Rotation = -15

addCorner(
	stem,
	3
)

stem.Parent =
	cloverHolder

table.insert(
	cloverLeaves,
	stem
)

--==================================================
-- LUCK TEXT
--==================================================

local luckMultiplier = Instance.new("TextLabel")

luckMultiplier.BackgroundTransparency = 1

luckMultiplier.Position =
	UDim2.fromOffset(62, 7)

luckMultiplier.Size =
	UDim2.new(1, -68, 0, 24)

luckMultiplier.Text = "2X LUCK"

luckMultiplier.TextColor3 = WHITE
luckMultiplier.TextSize = 17
luckMultiplier.Font = Enum.Font.GothamBold

luckMultiplier.TextXAlignment =
	Enum.TextXAlignment.Left

luckMultiplier.Parent = luckHud

local luckScope = Instance.new("TextLabel")

luckScope.BackgroundTransparency = 1

luckScope.Position =
	UDim2.fromOffset(62, 29)

luckScope.Size =
	UDim2.new(1, -68, 0, 15)

luckScope.Text = "SERVER"

luckScope.TextColor3 = CYAN
luckScope.TextSize = 9
luckScope.Font = Enum.Font.GothamBold

luckScope.TextXAlignment =
	Enum.TextXAlignment.Left

luckScope.Parent = luckHud

local luckTimer = Instance.new("TextLabel")

luckTimer.BackgroundTransparency = 1

luckTimer.Position =
	UDim2.fromOffset(62, 45)

luckTimer.Size =
	UDim2.new(1, -68, 0, 15)

luckTimer.Text = "15:00"

luckTimer.TextColor3 = GRAY
luckTimer.TextSize = 11
luckTimer.Font = Enum.Font.Gotham

luckTimer.TextXAlignment =
	Enum.TextXAlignment.Left

luckTimer.Parent = luckHud

--==================================================
-- LUCK STATE
--==================================================

local luckEndTime = 0
local currentLuckMultiplier = nil

local function setCloverColor(color)

	for _, part in ipairs(
		cloverLeaves
	) do

		part.BackgroundColor3 =
			color

	end

end

local function updateLuckColor(
	multiplier
)

	if multiplier == 2 then

		setCloverColor(GREEN)

	elseif multiplier == 4 then

		setCloverColor(GOLD)

	elseif multiplier == 8 then

		setCloverColor(RED)

	end

end

--==================================================
-- LUCK TIMER
--==================================================

task.spawn(function()

	while true do

		task.wait(0.25)

		if luckHud.Visible then

			local remaining =
				math.max(
					0,
					luckEndTime -
						os.clock()
				)

			local minutes =
				math.floor(
					remaining / 60
				)

			local seconds =
				math.floor(
					remaining % 60
				)

			luckTimer.Text =
				string.format(
					"%02d:%02d",
					minutes,
					seconds
				)

			if remaining <= 0 then

				luckHud.Visible =
					false

				currentLuckMultiplier =
					nil

				luckEndTime = 0

			end

		end

	end

end)

--==================================================
-- ANNOUNCEMENT
--==================================================

local announcementFrame =
	Instance.new("Frame")

announcementFrame.Name =
	"Announcement"

announcementFrame.AnchorPoint =
	Vector2.new(0.5, 0)

announcementFrame.Position =
	UDim2.new(
		0.5,
		0,
		0,
		-80
	)

announcementFrame.Size =
	UDim2.fromOffset(470, 54)

announcementFrame.BackgroundColor3 =
	DARK

announcementFrame.BackgroundTransparency =
	0.03

announcementFrame.Visible =
	false

-- NO OUTLINE
addNoOutline(
	announcementFrame
)

addCorner(
	announcementFrame,
	8
)

announcementFrame.Parent =
	gui

local announcementAccent =
	Instance.new("Frame")

announcementAccent.Position =
	UDim2.fromOffset(0, 0)

announcementAccent.Size =
	UDim2.fromOffset(4, 54)

announcementAccent.BackgroundColor3 =
	CYAN

addNoOutline(
	announcementAccent
)

addCorner(
	announcementAccent,
	4
)

announcementAccent.Parent =
	announcementFrame

local announcementText =
	Instance.new("TextLabel")

announcementText.BackgroundTransparency =
	1

announcementText.Position =
	UDim2.fromOffset(15, 5)

announcementText.Size =
	UDim2.new(
		1,
		-25,
		1,
		-10
	)

announcementText.Text = ""

announcementText.TextColor3 =
	WHITE

announcementText.TextSize = 15
announcementText.Font =
	Enum.Font.GothamBold

announcementText.TextWrapped = true

announcementText.Parent =
	announcementFrame

local announcementNumber = 0

local function showAnnouncement(
	text
)

	if typeof(text) ~= "string"
		or text == "" then

		return

	end

	announcementNumber += 1

	local current =
		announcementNumber

	announcementText.Text =
		text

	announcementFrame.Visible =
		true

	announcementFrame.Position =
		UDim2.new(
			0.5,
			0,
			0,
			-80
		)

	tween(
		announcementFrame,
		TweenInfo.new(
			0.2,
			Enum.EasingStyle.Quad,
			Enum.EasingDirection.Out
		),
		{
			Position =
				UDim2.new(
					0.5,
					0,
					0,
					14
				)
		}
	):Play()

	task.delay(
		4,
		function()

			if current ~=
				announcementNumber then

				return

			end

			local animation =
				tween(
					announcementFrame,
					TweenInfo.new(
						0.2,
						Enum.EasingStyle.Quad,
						Enum.EasingDirection.In
					),
					{
						Position =
							UDim2.new(
								0.5,
								0,
								0,
								-80
							)
					}
				)

			animation:Play()

			animation.Completed:Wait()

			if current ==
				announcementNumber then

				announcementFrame.Visible =
					false

			end

		end
	)

end

--==================================================
-- LUCK PAGE
--==================================================

local function showLuckPage()

	clearPage()

	makeLabel(
		pageHolder,
		"SERVER LUCK",
		UDim2.fromOffset(0, 0),
		UDim2.new(1, 0, 0, 25)
	)

	local info =
		Instance.new("TextLabel")

	info.BackgroundTransparency = 1

	info.Position =
		UDim2.fromOffset(0, 24)

	info.Size =
		UDim2.new(1, 0, 0, 22)

	info.Text =
		"Choose a multiplier • same multiplier adds time"

	info.TextColor3 = GRAY
	info.TextSize = 11
	info.Font = Enum.Font.Gotham

	info.TextXAlignment =
		Enum.TextXAlignment.Left

	info.Parent = pageHolder

	--==================================================
	-- 2X
	--==================================================

	local luck2 =
		makeButton(
			pageHolder,
			"🍀   2X",
			UDim2.fromOffset(
				12,
				57
			),
			UDim2.fromOffset(
				165,
				55
			),
			Color3.fromRGB(
				25,
				125,
				65
			)
		)

	luck2.TextColor3 =
		GREEN

	--==================================================
	-- 4X
	--==================================================

	local luck4 =
		makeButton(
			pageHolder,
			"🍀   4X",
			UDim2.fromOffset(
				190,
				57
			),
			UDim2.fromOffset(
				165,
				55
			),
			Color3.fromRGB(
				175,
				130,
				20
			)
		)

	luck4.TextColor3 =
		GOLD

	--==================================================
	-- 8X
	--==================================================

	local luck8 =
		makeButton(
			pageHolder,
			"🍀   8X",
			UDim2.fromOffset(
				368,
				57
			),
			UDim2.fromOffset(
				165,
				55
			),
			Color3.fromRGB(
				170,
				35,
				35
			)
		)

	luck8.TextColor3 =
		RED

	--==================================================
	-- GLOBAL
	--==================================================

	local globalButton =
		makeButton(
			pageHolder,
			"",
			UDim2.fromOffset(
				105,
				125
			),
			UDim2.fromOffset(
				350,
				46
			),
			DARK3
		)

	local function updateGlobalButton()

		if globalEnabled then

			globalButton.Text =
				"GLOBAL: ON"

			globalButton.TextColor3 =
				WHITE

			globalButton.BackgroundColor3 =
				Color3.fromRGB(
					25,
					150,
					65
				)

		else

			globalButton.Text =
				"GLOBAL: OFF  •  SERVER ONLY"

			globalButton.TextColor3 =
				WHITE

			globalButton.BackgroundColor3 =
				Color3.fromRGB(
					35,
					80,
					60
				)

		end

	end

	updateGlobalButton()

	globalButton.MouseButton1Click:Connect(
		function()

			globalEnabled =
				not globalEnabled

			updateGlobalButton()

		end
	)

	--==================================================
	-- STOP ALL
	--==================================================

	local stop =
		makeButton(
			pageHolder,
			"STOP ALL LUCK",
			UDim2.fromOffset(
				105,
				182
			),
			UDim2.fromOffset(
				350,
				46
			),
			RED
		)

	--==================================================
	-- ACTIVATE
	--==================================================

	local function activate(
		multiplier
	)

		Command:FireServer(
			"ServerLuck",
			{
				Multiplier = multiplier,
				Global = globalEnabled
			}
		)

	end

	luck2.MouseButton1Click:Connect(
		function()
			activate(2)
		end
	)

	luck4.MouseButton1Click:Connect(
		function()
			activate(4)
		end
	)

	luck8.MouseButton1Click:Connect(
		function()
			activate(8)
		end
	)

	stop.MouseButton1Click:Connect(
		function()

			Command:FireServer(
				"StopLuck",
				{
					Global = globalEnabled
				}
			)

		end
	)

end

--==================================================
-- BRAINROTS
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
	"CascadeCrab",

}

--==================================================
-- SPAWNER PAGE
--==================================================

local function showSpawnerPage()

	clearPage()

	makeLabel(
		pageHolder,
		"SPAWNER",
		UDim2.fromOffset(0, 0),
		UDim2.new(1, 0, 0, 25)
	)

	local scroll =
		Instance.new("ScrollingFrame")

	scroll.Position =
		UDim2.fromOffset(
			8,
			32
		)

	scroll.Size =
		UDim2.new(
			1,
			-16,
			1,
			-32
		)

	scroll.BackgroundColor3 =
		DARK2

	scroll.BackgroundTransparency =
		0.05

	scroll.BorderSizePixel = 0

	scroll.ScrollBarThickness = 3

	scroll.ScrollBarImageColor3 =
		CYAN

	scroll.CanvasSize =
		UDim2.new(
			0,
			0,
			0,
			#brainrots * 38
		)

	scroll.Parent =
		pageHolder

	addCorner(
		scroll,
		6
	)

	local layout =
		Instance.new("UIListLayout")

	layout.Padding =
		UDim.new(
			0,
			4
		)

	layout.Parent =
		scroll

	for _, id in ipairs(
		brainrots
	) do

		local button =
			makeButton(
				scroll,
				id,
				UDim2.new(),
				UDim2.new(
					1,
					-8,
					0,
					34
				),
				DARK3
			)

		button.MouseButton1Click:Connect(
			function()

				Command:FireServer(
					"SpawnBrainrot",
					{
						BrainrotId = id,
						Global =
							globalEnabled
					}
				)

			end
		)

	end

end

--==================================================
-- PLAYERS PAGE
--==================================================

local function showPlayersPage()

	clearPage()

	makeLabel(
		pageHolder,
		"GIVE BRAINROT",
		UDim2.fromOffset(0, 0),
		UDim2.new(1, 0, 0, 25)
	)

	local username =
		makeBox(
			pageHolder,
			"Username...",
			UDim2.fromOffset(
				12,
				32
			),
			UDim2.new(
				1,
				-24,
				0,
				40
			)
		)

	local brainrotBox =
		makeBox(
			pageHolder,
			"Brainrot ID...",
			UDim2.fromOffset(
				12,
				80
			),
			UDim2.new(
				1,
				-24,
				0,
				40
			)
		)

	local giveBrainrot =
		makeButton(
			pageHolder,
			"GIVE BRAINROT",
			UDim2.fromOffset(
				130,
				130
			),
			UDim2.fromOffset(
				300,
				42
			),
			PURPLE
		)

	makeLabel(
		pageHolder,
		"GIVE MONEY",
		UDim2.fromOffset(
			0,
			185
		),
		UDim2.new(
			1,
			0,
			0,
			25
		)
	)

	local moneyUser =
		makeBox(
			pageHolder,
			"Username...",
			UDim2.fromOffset(
				12,
				218
			),
			UDim2.fromOffset(
				260,
				40
			)
		)

	local amount =
		makeBox(
			pageHolder,
			"Amount...",
			UDim2.fromOffset(
				285,
				218
			),
			UDim2.fromOffset(
				260,
				40
			)
		)

	local giveMoney =
		makeButton(
			pageHolder,
			"GIVE MONEY",
			UDim2.fromOffset(
				130,
				268
			),
			UDim2.fromOffset(
				300,
				42
			),
			GREEN
		)

	giveBrainrot.MouseButton1Click:Connect(
		function()

			Command:FireServer(
				"GiveBrainrot",
				{
					Target =
						username.Text,

					BrainrotId =
						brainrotBox.Text
				}
			)

		end
	)

	giveMoney.MouseButton1Click:Connect(
		function()

			Command:FireServer(
				"GiveMoney",
				{
					Target =
						moneyUser.Text,

					Amount =
						amount.Text,

					Global =
						globalEnabled
				}
			)

		end
	)

end

--==================================================
-- MODERATION PAGE
--==================================================

local function showModeratePage()

	clearPage()

	makeLabel(
		pageHolder,
		"MODERATION",
		UDim2.fromOffset(0, 0),
		UDim2.new(1, 0, 0, 25)
	)

	local info =
		Instance.new("TextLabel")

	info.BackgroundTransparency = 1

	info.Position =
		UDim2.fromOffset(
			0,
			45
		)

	info.Size =
		UDim2.new(
			1,
			0,
			0,
			80
		)

	info.Text =
		"Your existing moderation system stays separate from this panel."

	info.TextColor3 =
		GRAY

	info.TextSize = 13
	info.Font = Enum.Font.Gotham

	info.TextWrapped = true

	info.Parent =
		pageHolder

end

--==================================================
-- ANNOUNCEMENT PAGE
--==================================================

local function showAnnouncementPage()

	clearPage()

	makeLabel(
		pageHolder,
		"ANNOUNCEMENT",
		UDim2.fromOffset(0, 0),
		UDim2.new(1, 0, 0, 25)
	)

	local announcementBox =
		Instance.new("TextBox")

	announcementBox.Position =
		UDim2.fromOffset(
			12,
			32
		)

	announcementBox.Size =
		UDim2.new(
			1,
			-24,
			0,
			115
		)

	announcementBox.BackgroundColor3 =
		DARK2

	announcementBox.Text = ""

	announcementBox.PlaceholderText =
		"Type your announcement..."

	announcementBox.PlaceholderColor3 =
		GRAY

	announcementBox.TextColor3 =
		WHITE

	announcementBox.TextSize = 16
	announcementBox.Font =
		Enum.Font.GothamMedium

	announcementBox.TextWrapped =
		true

	announcementBox.TextXAlignment =
		Enum.TextXAlignment.Left

	announcementBox.TextYAlignment =
		Enum.TextYAlignment.Top

	announcementBox.MultiLine =
		true

	announcementBox.ClearTextOnFocus =
		false

	addNoOutline(
		announcementBox
	)

	addCorner(
		announcementBox,
		7
	)

	announcementBox.Parent =
		pageHolder

	local counter =
		Instance.new("TextLabel")

	counter.BackgroundTransparency =
		1

	counter.Position =
		UDim2.new(
			1,
			-130,
			0,
			150
		)

	counter.Size =
		UDim2.fromOffset(
			115,
			20
		)

	counter.Text =
		"0 / 250"

	counter.TextColor3 =
		GRAY

	counter.TextSize = 11
	counter.Font =
		Enum.Font.Gotham

	counter.TextXAlignment =
		Enum.TextXAlignment.Right

	counter.Parent =
		pageHolder

	announcementBox:GetPropertyChangedSignal(
		"Text"
	):Connect(
		function()

			if #announcementBox.Text >
				250 then

				announcementBox.Text =
					string.sub(
						announcementBox.Text,
						1,
						250
					)

			end

			counter.Text =
				tostring(
					#announcementBox.Text
				)
				.. " / 250"

		end
	)

	local send =
		makeButton(
			pageHolder,
			"SEND ANNOUNCEMENT",
			UDim2.fromOffset(
				130,
				180
			),
			UDim2.fromOffset(
				300,
				44
			),
			CYAN
		)

	send.TextColor3 =
		Color3.fromRGB(
			5,
			30,
			28
		)

	send.MouseButton1Click:Connect(
		function()

			if not ActionAnnouncement then

				showAnnouncement(
					"Announcement system is not connected."
				)

				return
			end

			local text =
				announcementBox.Text

			text =
				string.gsub(
					text,
					"^%s+",
					""
				)

			text =
				string.gsub(
					text,
					"%s+$",
					""
				)

			if text == "" then
				return
			end

			ActionAnnouncement:FireServer(
				"Custom",
				text
			)

			announcementBox.Text = ""

		end
	)

end

--==================================================
-- TAB SWITCHING
--==================================================

local tabs = {

	luckTab,
	spawnTab,
	playerTab,
	moderateTab,
	announcementTab,

}

local function selectTab(selected)

	for _, tab in ipairs(tabs) do

		tab.BackgroundColor3 =
			DARK2

		tab.TextColor3 =
			GRAY

	end

	selected.BackgroundColor3 =
		CYAN

	selected.TextColor3 =
		Color3.fromRGB(
			5,
			30,
			28
		)

end

luckTab.MouseButton1Click:Connect(
	function()

		selectTab(luckTab)
		showLuckPage()

	end
)

spawnTab.MouseButton1Click:Connect(
	function()

		selectTab(spawnTab)
		showSpawnerPage()

	end
)

playerTab.MouseButton1Click:Connect(
	function()

		selectTab(playerTab)
		showPlayersPage()

	end
)

moderateTab.MouseButton1Click:Connect(
	function()

		selectTab(moderateTab)
		showModeratePage()

	end
)

announcementTab.MouseButton1Click:Connect(
	function()

		selectTab(announcementTab)
		showAnnouncementPage()

	end
)

--==================================================
-- OPEN PANEL
--==================================================

local function openPanel()

	if not IS_ADMIN then
		return
	end

	panel.Visible = true

	panel.Size =
		UDim2.fromOffset(
			520,
			340
		)

	tween(
		panel,
		TweenInfo.new(
			0.16,
			Enum.EasingStyle.Back,
			Enum.EasingDirection.Out
		),
		{
			Size =
				UDim2.fromOffset(
					560,
					365
				)
		}
	):Play()

	selectTab(luckTab)
	showLuckPage()

end

--==================================================
-- CLOSE PANEL
--==================================================

local function closePanel()

	panel.Visible = false

end

--==================================================
-- GEAR BUTTON
--==================================================

openButton.MouseButton1Click:Connect(
	function()

		playClick()

		if panel.Visible then

			closePanel()

		else

			openPanel()

		end

	end
)

--==================================================
-- CLOSE
--==================================================

close.MouseButton1Click:Connect(
	function()

		playClick()

		closePanel()

	end
)

--==================================================
-- E KEY
--==================================================

UserInputService.InputBegan:Connect(
	function(
		input,
		processed
	)

		if processed then
			return
		end

		if input.KeyCode ~=
			Enum.KeyCode.E then

			return

		end

		if not IS_ADMIN then
			return
		end

		playClick()

		if panel.Visible then

			closePanel()

		else

			openPanel()

		end

	end
)

--==================================================
-- SERVER UI EVENTS
--==================================================

UI.OnClientEvent:Connect(
	function(
		action,
		data
	)

		--==================================================
		-- AUTHORIZED
		--==================================================

		if action == "Authorized" then

			openButton.Visible = true

			return

		end

		--==================================================
		-- ERROR
		--==================================================

		if action == "Error" then

			warn(
				"[ADMIN ERROR]",
				data
			)

			showAnnouncement(
				"⚠ "
				.. tostring(data)
			)

			return

		end

		--==================================================
		-- SUCCESS
		--==================================================

		if action == "Success" then

			print(
				"[ADMIN]",
				data
			)

			showAnnouncement(
				"✓ "
				.. tostring(data)
			)

			return

		end

		--==================================================
		-- LUCK
		--==================================================

		if action == "Luck" then

			if typeof(data) ~=
				"table" then

				return

			end

			local multiplier =
				tonumber(
					data.Multiplier
				)

			local duration =
				tonumber(
					data.Duration
				)

			if not multiplier
				or not duration then

				return

			end

			-- Same multiplier = add time
			if currentLuckMultiplier ==
				multiplier
				and luckHud.Visible
				and luckEndTime >
					os.clock() then

				luckEndTime =
					luckEndTime +
					duration

			else

				-- Different multiplier
				-- replaces the old one
				luckEndTime =
					os.clock() +
					duration

			end

			currentLuckMultiplier =
				multiplier

			luckMultiplier.Text =
				tostring(multiplier)
				.. "X LUCK"

			updateLuckColor(
				multiplier
			)

			if data.Scope ==
				"Global" then

				luckScope.Text =
					"GLOBAL"

			else

				luckScope.Text =
					"SERVER"

			end

			luckHud.Visible =
				true

			return

		end

		--==================================================
		-- STOP LUCK
		--==================================================

		if action == "StopLuck" then

			luckHud.Visible =
				false

			luckEndTime = 0

			currentLuckMultiplier =
				nil

			return

		end

		--==================================================
		-- ANNOUNCEMENT
		--==================================================

		if action == "Announcement" then

			if typeof(data) ==
				"string" then

				showAnnouncement(data)

			elseif typeof(data) ==
				"table" then

				showAnnouncement(
					data.Text
						or data.Message
						or ""
				)

			end

			return

		end

	end
)

--==================================================
-- OPTIONAL EXISTING ANNOUNCEMENT SYSTEM
--==================================================

if ActionAnnouncement then

	ActionAnnouncement.OnClientEvent:Connect(
		function(
			action,
			message
		)

			if typeof(message) ==
				"string" then

				showAnnouncement(
					message
				)

			elseif typeof(action) ==
				"string" then

				showAnnouncement(
					action
				)

			end

		end
	)

end

--==================================================
-- ENABLE
--==================================================

gui.Enabled = true

openButton.Visible = true

--==================================================
-- CHECK ADMIN
--==================================================

task.delay(
	1,
	function()

		Command:FireServer(
			"CheckAdmin"
		)

	end
)

print(
	"Fight for Brainrots Admin Client Loaded | Admin:",
	IS_ADMIN
)

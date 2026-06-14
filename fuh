-- R3B3L - Final: Instant fly on/off, visible Aim Part, all features working
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local Camera = workspace.CurrentCamera
local LocalPlayer = Players.LocalPlayer
local Mouse = LocalPlayer:GetMouse()

local isAlive = true
local activeNotifications = {}

-- GUI (modern, card‑style)
local sg = Instance.new("ScreenGui")
sg.Name = "R3B3L"
sg.ResetOnSpawn = false
sg.Parent = game:GetService("CoreGui")

local mainFrame = Instance.new("Frame")
mainFrame.Size = UDim2.new(0, 380, 0, 520)
mainFrame.Position = UDim2.new(0.5, -190, 0.5, -260)
mainFrame.BackgroundColor3 = Color3.fromRGB(18, 18, 22)
mainFrame.BorderSizePixel = 0
mainFrame.ClipsDescendants = true
mainFrame.Active = true
mainFrame.Draggable = true
mainFrame.Parent = sg

local corner = Instance.new("UICorner")
corner.CornerRadius = UDim.new(0, 12)
corner.Parent = mainFrame

-- Title bar
local titleBar = Instance.new("Frame")
titleBar.Size = UDim2.new(1, 0, 0, 50)
titleBar.BackgroundColor3 = Color3.fromRGB(25, 25, 30)
titleBar.BorderSizePixel = 0
titleBar.Parent = mainFrame

local titleCorner = Instance.new("UICorner")
titleCorner.CornerRadius = UDim.new(0, 12)
titleCorner.Parent = titleBar

local titleLabel = Instance.new("TextLabel")
titleLabel.Size = UDim2.new(1, -50, 1, 0)
titleLabel.Position = UDim2.new(0, 15, 0, 0)
titleLabel.BackgroundTransparency = 1
titleLabel.Text = "R3B3L"
titleLabel.TextColor3 = Color3.fromRGB(0, 255, 200)
titleLabel.Font = Enum.Font.GothamBold
titleLabel.TextSize = 20
titleLabel.TextXAlignment = Enum.TextXAlignment.Left
titleLabel.Parent = titleBar

local hideBtn = Instance.new("TextButton")
hideBtn.Size = UDim2.new(0, 30, 0, 30)
hideBtn.Position = UDim2.new(1, -40, 0, 10)
hideBtn.BackgroundColor3 = Color3.fromRGB(40, 40, 45)
hideBtn.Text = "✕"
hideBtn.TextColor3 = Color3.fromRGB(200, 200, 200)
hideBtn.Font = Enum.Font.SourceSansBold
hideBtn.TextSize = 16
hideBtn.BorderSizePixel = 0
hideBtn.Parent = titleBar

local hideCorner = Instance.new("UICorner")
hideCorner.CornerRadius = UDim.new(0, 6)
hideCorner.Parent = hideBtn

-- Tabs
local tabFrame = Instance.new("Frame")
tabFrame.Size = UDim2.new(1, -20, 0, 45)
tabFrame.Position = UDim2.new(0, 10, 0, 55)
tabFrame.BackgroundTransparency = 1
tabFrame.Parent = mainFrame

local function createTabButton(name, posX, color)
    local btn = Instance.new("TextButton")
    btn.Size = UDim2.new(0.23, 0, 1, -5)
    btn.Position = UDim2.new(posX, 0, 0, 2)
    btn.BackgroundColor3 = Color3.fromRGB(30, 30, 35)
    btn.Text = name
    btn.TextColor3 = color or Color3.fromRGB(200, 200, 200)
    btn.Font = Enum.Font.SourceSansBold
    btn.TextSize = 13
    btn.BorderSizePixel = 0
    btn.Parent = tabFrame
    local btnCorner = Instance.new("UICorner")
    btnCorner.CornerRadius = UDim.new(0, 8)
    btnCorner.Parent = btn
    return btn
end

local aimBtn = createTabButton("Aimbot", 0, Color3.fromRGB(0, 255, 200))
local visBtn = createTabButton("Visuals", 0.26, Color3.fromRGB(200, 200, 200))
local playerBtn = createTabButton("Player", 0.52, Color3.fromRGB(200, 200, 200))
local settingsBtn = createTabButton("Settings", 0.78, Color3.fromRGB(200, 200, 200))

-- Content frames
local function createContentFrame()
    local frame = Instance.new("ScrollingFrame")
    frame.Size = UDim2.new(1, -20, 1, -110)
    frame.Position = UDim2.new(0, 10, 0, 105)
    frame.BackgroundColor3 = Color3.fromRGB(22, 22, 27)
    frame.BackgroundTransparency = 0
    frame.BorderSizePixel = 0
    frame.CanvasSize = UDim2.new(0, 0, 0, 520) -- increased to fit all items
    frame.ScrollBarThickness = 4
    frame.ScrollBarImageColor3 = Color3.fromRGB(50, 50, 60)
    frame.Visible = false
    local frameCorner = Instance.new("UICorner")
    frameCorner.CornerRadius = UDim.new(0, 10)
    frameCorner.Parent = frame
    frame.Parent = mainFrame
    return frame
end

local aimContent = createContentFrame()
aimContent.Visible = true
local visContent = createContentFrame()
local playerContent = createContentFrame()
local settingsContent = createContentFrame()

-- Helper: create a card-style toggle
local function addToggle(parent, y, text, default)
    local card = Instance.new("Frame")
    card.Size = UDim2.new(1, -10, 0, 45)
    card.Position = UDim2.new(0, 5, 0, y)
    card.BackgroundColor3 = Color3.fromRGB(28, 28, 34)
    card.BorderSizePixel = 0
    card.Parent = parent
    local cardCorner = Instance.new("UICorner")
    cardCorner.CornerRadius = UDim.new(0, 8)
    cardCorner.Parent = card

    local lbl = Instance.new("TextLabel")
    lbl.Size = UDim2.new(0.6, 0, 1, 0)
    lbl.Position = UDim2.new(0, 15, 0, 0)
    lbl.BackgroundTransparency = 1
    lbl.Text = text
    lbl.TextColor3 = Color3.fromRGB(220, 220, 220)
    lbl.Font = Enum.Font.SourceSans
    lbl.TextSize = 14
    lbl.TextXAlignment = Enum.TextXAlignment.Left
    lbl.Parent = card

    local btn = Instance.new("TextButton")
    btn.Size = UDim2.new(0, 60, 0, 30)
    btn.Position = UDim2.new(1, -75, 0.5, -15)
    btn.BackgroundColor3 = default == "ON" and Color3.fromRGB(0, 180, 0) or Color3.fromRGB(40, 40, 50)
    btn.Text = default
    btn.TextColor3 = Color3.fromRGB(255, 255, 255)
    btn.Font = Enum.Font.SourceSansBold
    btn.TextSize = 13
    btn.BorderSizePixel = 0
    btn.Parent = card
    local btnCorner = Instance.new("UICorner")
    btnCorner.CornerRadius = UDim.new(0, 6)
    btnCorner.Parent = btn
    return btn
end

-- Helper: create a card-style slider
local function createSlider(parent, y, label, minVal, maxVal, default, callback)
    local card = Instance.new("Frame")
    card.Size = UDim2.new(1, -10, 0, 70)
    card.Position = UDim2.new(0, 5, 0, y)
    card.BackgroundColor3 = Color3.fromRGB(28, 28, 34)
    card.BorderSizePixel = 0
    card.Parent = parent
    local cardCorner = Instance.new("UICorner")
    cardCorner.CornerRadius = UDim.new(0, 8)
    cardCorner.Parent = card

    local lbl = Instance.new("TextLabel")
    lbl.Size = UDim2.new(1, -20, 0, 25)
    lbl.Position = UDim2.new(0, 15, 0, 5)
    lbl.BackgroundTransparency = 1
    lbl.Text = label .. ": " .. tostring(default)
    lbl.TextColor3 = Color3.fromRGB(220, 220, 220)
    lbl.Font = Enum.Font.SourceSans
    lbl.TextSize = 14
    lbl.TextXAlignment = Enum.TextXAlignment.Left
    lbl.Parent = card

    local sliderBg = Instance.new("Frame")
    sliderBg.Size = UDim2.new(1, -30, 0, 6)
    sliderBg.Position = UDim2.new(0, 15, 0, 45)
    sliderBg.BackgroundColor3 = Color3.fromRGB(50, 50, 55)
    sliderBg.BorderSizePixel = 0
    sliderBg.Parent = card
    local sliderBgCorner = Instance.new("UICorner")
    sliderBgCorner.CornerRadius = UDim.new(0, 3)
    sliderBgCorner.Parent = sliderBg

    local sliderFill = Instance.new("Frame")
    sliderFill.Size = UDim2.new((default - minVal) / (maxVal - minVal), 0, 1, 0)
    sliderFill.BackgroundColor3 = Color3.fromRGB(0, 200, 100)
    sliderFill.BorderSizePixel = 0
    sliderFill.Parent = sliderBg
    local sliderFillCorner = Instance.new("UICorner")
    sliderFillCorner.CornerRadius = UDim.new(0, 3)
    sliderFillCorner.Parent = sliderFill

    local sliderBtn = Instance.new("TextButton")
    sliderBtn.Size = UDim2.new(0, 16, 0, 16)
    sliderBtn.Position = UDim2.new((default - minVal) / (maxVal - minVal), -8, 0, -5)
    sliderBtn.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
    sliderBtn.Text = ""
    sliderBtn.BorderSizePixel = 0
    sliderBtn.Parent = sliderBg
    local sliderBtnCorner = Instance.new("UICorner")
    sliderBtnCorner.CornerRadius = UDim.new(0, 8)
    sliderBtnCorner.Parent = sliderBtn

    local value = default
    local dragging = false

    local function updateValue(newVal)
        value = math.clamp(newVal, minVal, maxVal)
        local percent = (value - minVal) / (maxVal - minVal)
        sliderFill.Size = UDim2.new(percent, 0, 1, 0)
        sliderBtn.Position = UDim2.new(percent, -8, 0, -5)
        lbl.Text = label .. ": " .. math.floor(value)
        if callback then callback(value) end
        return value
    end

    sliderBtn.MouseButton1Down:Connect(function()
        dragging = true
        local connection
        connection = UserInputService.InputChanged:Connect(function(input)
            if dragging and input.UserInputType == Enum.UserInputType.MouseMovement then
                local mousePos = UserInputService:GetMouseLocation()
                local bgPos = sliderBg.AbsolutePosition
                local bgWidth = sliderBg.AbsoluteSize.X
                local percent = math.clamp((mousePos.X - bgPos.X) / bgWidth, 0, 1)
                updateValue(minVal + percent * (maxVal - minVal))
            end
        end)
        local releaseConn
        releaseConn = UserInputService.InputEnded:Connect(function(input)
            if input.UserInputType == Enum.UserInputType.MouseButton1 then
                dragging = false
                connection:Disconnect()
                releaseConn:Disconnect()
            end
        end)
    end)

    sliderBg.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 then
            local mousePos = UserInputService:GetMouseLocation()
            local bgPos = sliderBg.AbsolutePosition
            local bgWidth = sliderBg.AbsoluteSize.X
            local percent = math.clamp((mousePos.X - bgPos.X) / bgWidth, 0, 1)
            updateValue(minVal + percent * (maxVal - minVal))
        end
    end)

    return card, function() return value end
end

-- Helper: create a card-style input
local function addInput(parent, y, text, default)
    local card = Instance.new("Frame")
    card.Size = UDim2.new(1, -10, 0, 55)
    card.Position = UDim2.new(0, 5, 0, y)
    card.BackgroundColor3 = Color3.fromRGB(28, 28, 34)
    card.BorderSizePixel = 0
    card.Parent = parent
    local cardCorner = Instance.new("UICorner")
    cardCorner.CornerRadius = UDim.new(0, 8)
    cardCorner.Parent = card

    local lbl = Instance.new("TextLabel")
    lbl.Size = UDim2.new(0.5, 0, 1, 0)
    lbl.Position = UDim2.new(0, 15, 0, 0)
    lbl.BackgroundTransparency = 1
    lbl.Text = text
    lbl.TextColor3 = Color3.fromRGB(220, 220, 220)
    lbl.Font = Enum.Font.SourceSans
    lbl.TextSize = 14
    lbl.TextXAlignment = Enum.TextXAlignment.Left
    lbl.Parent = card

    local box = Instance.new("TextBox")
    box.Size = UDim2.new(0, 80, 0, 35)
    box.Position = UDim2.new(1, -95, 0.5, -17)
    box.BackgroundColor3 = Color3.fromRGB(40, 40, 48)
    box.Text = tostring(default)
    box.TextColor3 = Color3.fromRGB(255, 255, 255)
    box.Font = Enum.Font.SourceSans
    box.TextSize = 14
    box.TextXAlignment = Enum.TextXAlignment.Center
    box.BorderSizePixel = 0
    box.ClearTextOnFocus = false
    box.Parent = card
    local boxCorner = Instance.new("UICorner")
    boxCorner.CornerRadius = UDim.new(0, 6)
    boxCorner.Parent = box
    return box
end

-- Aimbot UI (reordered to make Aim Part visible)
local aimToggle = addToggle(aimContent, 10, "Aimbot", "OFF")
local lockToggle = addToggle(aimContent, 65, "Lock-on", "OFF")
local teamToggle = addToggle(aimContent, 120, "Team Check", "OFF")
local wallToggle = addToggle(aimContent, 175, "Wall Check", "OFF")
local hpToggle = addToggle(aimContent, 230, "HP Check", "OFF")
local fovToggle = addToggle(aimContent, 285, "Show FOV", "OFF")
local fovInput = addInput(aimContent, 340, "FOV Size", "200")
local smoothInput = addInput(aimContent, 405, "Smoothness", "5")
local partInput = addInput(aimContent, 470, "Aim Part", "Head") -- fully visible now

-- Visuals UI
local espToggle = addToggle(visContent, 10, "ESP (Name, Distance, Health)", "OFF")

-- Player UI
local wsSlider, getWS = createSlider(playerContent, 10, "Walk Speed", 1, 500, 16, function(val)
    local char = LocalPlayer.Character
    if char then
        local hum = char:FindFirstChildOfClass("Humanoid")
        if hum then hum.WalkSpeed = math.floor(val) end
    end
end)
local jpSlider, getJP = createSlider(playerContent, 90, "Jump Power", 1, 500, 50, function(val)
    local char = LocalPlayer.Character
    if char then
        local hum = char:FindFirstChildOfClass("Humanoid")
        if hum then hum.JumpPower = math.floor(val) end
    end
end)
local noclipToggle = addToggle(playerContent, 170, "Noclip", "OFF")
local flyToggle = addToggle(playerContent, 225, "Fly", "OFF")
local flySpeedSlider, getFlySpeed = createSlider(playerContent, 275, "Fly Speed", 1, 500, 50)

-- Settings UI
local menuKeyBtn = Instance.new("TextButton")
menuKeyBtn.Size = UDim2.new(0.9, 0, 0, 45)
menuKeyBtn.Position = UDim2.new(0.05, 0, 0.05, 0)
menuKeyBtn.BackgroundColor3 = Color3.fromRGB(28, 28, 34)
menuKeyBtn.Text = "Menu Keybind: LeftCtrl"
menuKeyBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
menuKeyBtn.Font = Enum.Font.SourceSansBold
menuKeyBtn.TextSize = 14
menuKeyBtn.BorderSizePixel = 0
menuKeyBtn.Parent = settingsContent
local menuKeyCorner = Instance.new("UICorner")
menuKeyCorner.CornerRadius = UDim.new(0, 8)
menuKeyCorner.Parent = menuKeyBtn

local lockKeyBtn = Instance.new("TextButton")
lockKeyBtn.Size = UDim2.new(0.9, 0, 0, 45)
lockKeyBtn.Position = UDim2.new(0.05, 0, 0.15, 0)
lockKeyBtn.BackgroundColor3 = Color3.fromRGB(28, 28, 34)
lockKeyBtn.Text = "Lock-on Keybind: Q"
lockKeyBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
lockKeyBtn.Font = Enum.Font.SourceSansBold
lockKeyBtn.TextSize = 14
lockKeyBtn.BorderSizePixel = 0
lockKeyBtn.Parent = settingsContent
local lockKeyCorner = Instance.new("UICorner")
lockKeyCorner.CornerRadius = UDim.new(0, 8)
lockKeyCorner.Parent = lockKeyBtn

local resetPosBtn = Instance.new("TextButton")
resetPosBtn.Size = UDim2.new(0.9, 0, 0, 45)
resetPosBtn.Position = UDim2.new(0.05, 0, 0.25, 0)
resetPosBtn.BackgroundColor3 = Color3.fromRGB(28, 28, 34)
resetPosBtn.Text = "Reset Menu Position"
resetPosBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
resetPosBtn.Font = Enum.Font.SourceSansBold
resetPosBtn.TextSize = 14
resetPosBtn.BorderSizePixel = 0
resetPosBtn.Parent = settingsContent
local resetCorner = Instance.new("UICorner")
resetCorner.CornerRadius = UDim.new(0, 8)
resetCorner.Parent = resetPosBtn

local uninjectBtn = Instance.new("TextButton")
uninjectBtn.Size = UDim2.new(0.9, 0, 0, 50)
uninjectBtn.Position = UDim2.new(0.05, 0, 0.36, 0)
uninjectBtn.BackgroundColor3 = Color3.fromRGB(180, 20, 20)
uninjectBtn.Text = "UNINJECT"
uninjectBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
uninjectBtn.Font = Enum.Font.SourceSansBold
uninjectBtn.TextSize = 16
uninjectBtn.BorderSizePixel = 0
uninjectBtn.Parent = settingsContent
local unijCorner = Instance.new("UICorner")
unijCorner.CornerRadius = UDim.new(0, 8)
unijCorner.Parent = uninjectBtn

-- State
local state = {
    aim = false, lockEnabled = false, teamCheck = false, wall = false, hp = false, fovVis = false,
    esp = false, fly = false, noclip = false,
    fovSize = 200, smoothness = 5, aimPart = "Head",
    menuKey = Enum.KeyCode.LeftControl, lockKey = Enum.KeyCode.Q, menuVisible = true
}

local espObjects = {}
local flyBodyVel = nil
local flyBodyGyro = nil
local listeningForKey = false
local listeningForLock = false
local lockedTarget = nil
local flyActive = false

local fovCircle = Drawing.new("Circle")
fovCircle.Thickness = 2
fovCircle.Color = Color3.new(0, 1, 0)
fovCircle.Transparency = 0.4
fovCircle.NumSides = 64
fovCircle.Visible = false
fovCircle.Radius = state.fovSize

-- ========== NOCLIP ==========
local function updateNoclip()
    if not isAlive then return end
    local char = LocalPlayer.Character
    if not char then return end
    for _, part in ipairs(char:GetDescendants()) do
        if part:IsA("BasePart") then
            part.CanCollide = not state.noclip
        end
    end
end

local function setupNoclipHandler()
    local char = LocalPlayer.Character
    if not char then return end
    local function onPartAdded(part)
        if part:IsA("BasePart") then
            part.CanCollide = not state.noclip
        end
    end
    for _, part in ipairs(char:GetDescendants()) do
        if part:IsA("BasePart") then
            part.CanCollide = not state.noclip
        end
    end
    char.DescendantAdded:Connect(onPartAdded)
end

-- ========== STACKED NOTIFICATIONS ==========
local function showNotification(title, desc, isError, isLockOn, player)
    local notifHeight = 65
    local offset = #activeNotifications * (notifHeight + 5)

    local notification = Instance.new("ScreenGui")
    notification.Name = "Notification"
    notification.Parent = game:GetService("CoreGui")
    notification.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
    notification.IgnoreGuiInset = true

    local frame = Instance.new("Frame")
    frame.Size = UDim2.new(0, isLockOn and 240 or 320, 0, notifHeight)
    frame.Position = UDim2.new(0.5, isLockOn and -120 or -160, 0.2, offset)
    frame.BackgroundColor3 = Color3.fromRGB(25, 25, 30)
    frame.BackgroundTransparency = 0.05
    frame.BorderSizePixel = 0
    frame.ClipsDescendants = true
    frame.Parent = notification
    local frameCorner = Instance.new("UICorner")
    frameCorner.CornerRadius = UDim.new(0, 8)
    frameCorner.Parent = frame

    local notificationData = {gui = notification, frame = frame}
    table.insert(activeNotifications, notificationData)

    local function updatePositions()
        for i, notif in ipairs(activeNotifications) do
            notif.frame.Position = UDim2.new(0.5, (notif.frame.Size.X.Offset == 240 and -120 or -160), 0.2, (i - 1) * (notifHeight + 5))
        end
    end

    if isLockOn and player then
        local avatar = Instance.new("ImageLabel")
        avatar.Size = UDim2.new(0, 40, 0, 40)
        avatar.Position = UDim2.new(0, 12, 0, 12)
        avatar.BackgroundTransparency = 1
        avatar.Image = "rbxthumb://type=AvatarHeadShot&id=" .. player.UserId .. "&w=150&h=150"
        avatar.Parent = frame
        local avatarCorner = Instance.new("UICorner")
        avatarCorner.CornerRadius = UDim.new(0, 8)
        avatarCorner.Parent = avatar

        local titleLbl = Instance.new("TextLabel")
        titleLbl.Size = UDim2.new(1, -65, 1, 0)
        titleLbl.Position = UDim2.new(0, 65, 0, 0)
        titleLbl.BackgroundTransparency = 1
        titleLbl.Text = title
        titleLbl.TextColor3 = isError and Color3.fromRGB(255, 80, 80) or Color3.fromRGB(0, 255, 100)
        titleLbl.Font = Enum.Font.SourceSansBold
        titleLbl.TextSize = 16
        titleLbl.TextXAlignment = Enum.TextXAlignment.Left
        titleLbl.TextYAlignment = Enum.TextYAlignment.Center
        titleLbl.Parent = frame
    else
        local titleLbl = Instance.new("TextLabel")
        titleLbl.Size = UDim2.new(1, -20, 0, 30)
        titleLbl.Position = UDim2.new(0, 10, 0, 8)
        titleLbl.BackgroundTransparency = 1
        titleLbl.Text = title
        titleLbl.TextColor3 = isError and Color3.fromRGB(255, 80, 80) or Color3.fromRGB(0, 255, 100)
        titleLbl.Font = Enum.Font.SourceSansBold
        titleLbl.TextSize = 16
        titleLbl.TextXAlignment = Enum.TextXAlignment.Left
        titleLbl.Parent = frame

        local descLbl = Instance.new("TextLabel")
        descLbl.Size = UDim2.new(1, -20, 0, 25)
        descLbl.Position = UDim2.new(0, 10, 0, 35)
        descLbl.BackgroundTransparency = 1
        descLbl.Text = desc
        descLbl.TextColor3 = Color3.fromRGB(180, 180, 190)
        descLbl.Font = Enum.Font.SourceSans
        descLbl.TextSize = 12
        descLbl.TextXAlignment = Enum.TextXAlignment.Left
        descLbl.TextWrapped = true
        descLbl.Parent = frame
    end

    task.wait(2.5)

    for i, notif in ipairs(activeNotifications) do
        if notif == notificationData then
            table.remove(activeNotifications, i)
            break
        end
    end
    notification:Destroy()
    updatePositions()
end

-- ========== ESP (lowered text) ==========
local function getHeadAndFoot(character)
    local hrp = character:FindFirstChild("HumanoidRootPart")
    local head = character:FindFirstChild("Head")
    if not hrp then return nil, nil end
    local footWorld = hrp.Position - Vector3.new(0, hrp.Size.Y/2, 0)
    local headWorld = head and head.Position or (hrp.Position + Vector3.new(0, 3, 0))
    local footScreen, footVis = Camera:WorldToScreenPoint(footWorld)
    local headScreen, headVis = Camera:WorldToScreenPoint(headWorld)
    if not footVis or not headVis then return nil, nil end
    return headScreen, footScreen
end

local function getPlayerColor(player)
    if player == LocalPlayer then return Color3.fromRGB(255,255,255) end
    local lt, pt = LocalPlayer.Team, player.Team
    if lt and pt then
        return lt == pt and Color3.fromRGB(0,100,255) or Color3.fromRGB(255,0,0)
    end
    return Color3.fromRGB(0,255,0)
end

local function clearESP()
    for player, drawings in pairs(espObjects) do
        for _, obj in ipairs(drawings) do
            pcall(function() obj:Remove() end)
        end
    end
    espObjects = {}
end

local function updateESP()
    if not isAlive then return end
    clearESP()
    if not state.esp then return end
    for _, player in ipairs(Players:GetPlayers()) do
        if player == LocalPlayer then continue end
        local char = player.Character
        if not char then continue end
        local hum = char:FindFirstChildOfClass("Humanoid")
        if not hum or hum.Health <= 0 then continue end
        local color = getPlayerColor(player)
        local headScreen, footScreen = getHeadAndFoot(char)
        if not headScreen or not footScreen then continue end
        local objs = {}
        local centerX = (headScreen.X + footScreen.X) / 2
        local headY = headScreen.Y
        local textY = headY - 10

        local nameText = Drawing.new("Text")
        nameText.Text = player.Name
        nameText.Position = Vector2.new(centerX, textY)
        nameText.Size = 16
        nameText.Color = color
        nameText.Center = true
        nameText.Outline = true
        nameText.OutlineColor = Color3.new(0,0,0)
        nameText.Visible = true
        table.insert(objs, nameText)
        textY = textY + 18

        local hpPercent = math.floor((hum.Health / hum.MaxHealth) * 100)
        local hpColor = Color3.fromRGB(255 - hpPercent*2.55, hpPercent*2.55, 0)
        local hpText = Drawing.new("Text")
        hpText.Text = hpPercent .. "%"
        hpText.Position = Vector2.new(centerX, textY)
        hpText.Size = 14
        hpText.Color = hpColor
        hpText.Center = true
        hpText.Outline = true
        hpText.OutlineColor = Color3.new(0,0,0)
        hpText.Visible = true
        table.insert(objs, hpText)
        textY = textY + 16

        local hrp = char:FindFirstChild("HumanoidRootPart")
        if hrp then
            local dist = math.floor((Camera.CFrame.Position - hrp.Position).Magnitude)
            local distText = Drawing.new("Text")
            distText.Text = dist .. "m"
            distText.Position = Vector2.new(centerX, textY)
            distText.Size = 12
            distText.Color = Color3.fromRGB(255,255,255)
            distText.Center = true
            distText.Outline = true
            distText.OutlineColor = Color3.new(0,0,0)
            distText.Visible = true
            table.insert(objs, distText)
        end
        espObjects[player] = objs
    end
end

-- ========== FLY (INSTANT ON/OFF, NO DELAY) ==========
local function startFly()
    if not isAlive then return end
    local char = LocalPlayer.Character
    if not char then return end
    local hrp = char:FindFirstChild("HumanoidRootPart")
    if not hrp then return end
    -- Clean up old instances
    if flyBodyVel then flyBodyVel:Destroy() end
    if flyBodyGyro then flyBodyGyro:Destroy() end
    flyBodyVel = Instance.new("BodyVelocity")
    flyBodyVel.MaxForce = Vector3.new(1e5, 1e5, 1e5)
    flyBodyVel.Velocity = Vector3.new(0, 0, 0)
    flyBodyVel.Parent = hrp
    flyBodyGyro = Instance.new("BodyGyro")
    flyBodyGyro.MaxTorque = Vector3.new(1e5, 1e5, 1e5)
    flyBodyGyro.CFrame = hrp.CFrame
    flyBodyGyro.Parent = hrp
    flyActive = true
    char.Humanoid.PlatformStand = true
end

local function stopFly()
    flyActive = false
    if flyBodyVel then flyBodyVel:Destroy(); flyBodyVel = nil end
    if flyBodyGyro then flyBodyGyro:Destroy(); flyBodyGyro = nil end
    local char = LocalPlayer.Character
    if char and char:FindFirstChildOfClass("Humanoid") then
        char.Humanoid.PlatformStand = false
    end
end

local function updateFly()
    if not isAlive then return end
    if not state.fly then
        if flyActive then stopFly() end
        return
    end
    if not flyActive then
        startFly()
        return
    end
    local char = LocalPlayer.Character
    if not char then return end
    local hrp = char:FindFirstChild("HumanoidRootPart")
    if not hrp then return
    if not flyBodyVel or not flyBodyVel.Parent then
        startFly()
        return
    end
    local speed = getFlySpeed()
    speed = math.clamp(speed, 1, 500)
    local moveDir = Vector3.new(0, 0, 0)
    if UserInputService:IsKeyDown(Enum.KeyCode.W) then moveDir = moveDir + Camera.CFrame.LookVector end
    if UserInputService:IsKeyDown(Enum.KeyCode.S) then moveDir = moveDir - Camera.CFrame.LookVector end
    if UserInputService:IsKeyDown(Enum.KeyCode.A) then moveDir = moveDir - Camera.CFrame.RightVector end
    if UserInputService:IsKeyDown(Enum.KeyCode.D) then moveDir = moveDir + Camera.CFrame.RightVector end
    flyBodyVel.Velocity = moveDir * speed
    flyBodyGyro.CFrame = Camera.CFrame
    char.Humanoid.PlatformStand = true
end

-- ========== AIMBOT ==========
local function getTargetPart(character)
    if not character then return nil end
    if state.aimPart == "Head" then
        local head = character:FindFirstChild("Head")
        if head then return head end
    end
    return character:FindFirstChild("HumanoidRootPart") or character:FindFirstChild("UpperTorso") or character:FindFirstChild("Torso")
end

local function isAliveChar(character)
    local hum = character:FindFirstChildOfClass("Humanoid")
    return hum and hum.Health > 0
end

local function isEnemy(player)
    if not state.teamCheck then return true end
    if player == LocalPlayer then return false end
    local lt, pt = LocalPlayer.Team, player.Team
    if lt and pt then return lt ~= pt end
    return true
end

local function canSee(part)
    if not state.wall then return true end
    local origin = Camera.CFrame.Position
    local dir = (part.Position - origin).Unit
    local dist = (origin - part.Position).Magnitude
    local params = RaycastParams.new()
    params.FilterType = Enum.RaycastFilterType.Blacklist
    params.FilterDescendantsInstances = {LocalPlayer.Character}
    local result = workspace:Raycast(origin, dir * dist, params)
    return not result or result.Instance:IsDescendantOf(part.Parent)
end

local function getClosestEnemy()
    local closest, closestDist = nil, state.fovSize
    local center = Vector2.new(Camera.ViewportSize.X/2, Camera.ViewportSize.Y/2)
    for _, player in ipairs(Players:GetPlayers()) do
        if player == LocalPlayer then continue end
        if not isEnemy(player) then continue end
        local char = player.Character
        if not char or not isAliveChar(char) then continue end
        if state.hp then
            local hum = char:FindFirstChildOfClass("Humanoid")
            if hum and hum.Health / hum.MaxHealth < 0.2 then continue end
        end
        local part = getTargetPart(char)
        if not part or not canSee(part) then continue end
        local screen, on = Camera:WorldToScreenPoint(part.Position)
        if on then
            local dist = (Vector2.new(screen.X, screen.Y) - center).Magnitude
            if dist < closestDist then
                closestDist = dist
                closest = player
            end
        end
    end
    return closest
end

local function aimAt(pos)
    if not pos then return end
    local targetCF = CFrame.new(Camera.CFrame.Position, pos)
    local smooth = tonumber(smoothInput.Text) or 5
    if smooth <= 1 then
        Camera.CFrame = targetCF
    else
        local step = 0.3 / smooth
        Camera.CFrame = Camera.CFrame:Lerp(targetCF, step)
    end
end

-- ========== LOCK-ON ==========
local function getPlayerUnderCursor()
    local mousePos = Vector2.new(Mouse.X, Mouse.Y)
    local closestPlayer = nil
    local closestDist = 50
    for _, player in ipairs(Players:GetPlayers()) do
        if player == LocalPlayer then continue end
        if not isEnemy(player) then continue end
        local char = player.Character
        if not char or not isAliveChar(char) then continue end
        local hrp = char:FindFirstChild("HumanoidRootPart")
        if hrp then
            local screenPos, onScreen = Camera:WorldToScreenPoint(hrp.Position)
            if onScreen then
                local dist = (Vector2.new(screenPos.X, screenPos.Y) - mousePos).Magnitude
                if dist < closestDist then
                    closestDist = dist
                    closestPlayer = player
                end
            end
        end
    end
    return closestPlayer
end

local function performLockOn()
    if not isAlive then return end
    if not state.lockEnabled then
        showNotification("Lock-on disabled", "Enable it in the Aimbot tab", true, false)
        return
    end
    local target = getPlayerUnderCursor()
    if target then
        lockedTarget = target
        showNotification("Locked on!", "", false, true, target)
    else
        if lockedTarget ~= nil then
            lockedTarget = nil
            showNotification("Unlocked!", "Lock-on cleared", false, false)
        else
            showNotification("Not locked on!", "Place cursor on enemy and press " .. tostring(state.lockKey):gsub("Enum.KeyCode.", "") .. " to lock on", true, true, LocalPlayer)
        end
    end
end

-- ========== MENU & KEYBINDS ==========
local function toggleMenu()
    if not isAlive then return end
    state.menuVisible = not state.menuVisible
    mainFrame.Visible = state.menuVisible
end

local function startKeybinding()
    listeningForKey = true
    menuKeyBtn.Text = "Press any key..."
    menuKeyBtn.BackgroundColor3 = Color3.fromRGB(50, 50, 60)
end

local function startLockKeybinding()
    listeningForLock = true
    lockKeyBtn.Text = "Press any key..."
    lockKeyBtn.BackgroundColor3 = Color3.fromRGB(50, 50, 60)
end

local function fullCleanup()
    if not isAlive then return end
    isAlive = false
    stopFly()
    fovCircle:Remove()
    clearESP()
    sg:Destroy()
end

-- ========== INPUT HANDLING ==========
UserInputService.InputBegan:Connect(function(input, gp)
    if not isAlive then return end
    if gp then return end
    if listeningForKey then
        local key = input.KeyCode
        if key ~= Enum.KeyCode.Unknown then
            state.menuKey = key
            listeningForKey = false
            local keyName = tostring(key):gsub("Enum.KeyCode.", "")
            menuKeyBtn.Text = "Menu Keybind: " .. keyName
            menuKeyBtn.BackgroundColor3 = Color3.fromRGB(28, 28, 34)
        end
        return
    end
    if listeningForLock then
        local key = input.KeyCode
        if key ~= Enum.KeyCode.Unknown then
            state.lockKey = key
            listeningForLock = false
            local keyName = tostring(key):gsub("Enum.KeyCode.", "")
            lockKeyBtn.Text = "Lock-on Keybind: " .. keyName
            lockKeyBtn.BackgroundColor3 = Color3.fromRGB(28, 28, 34)
        end
        return
    end
    if input.KeyCode == state.menuKey then toggleMenu() end
    if input.KeyCode == state.lockKey then performLockOn() end
    if not state.menuVisible then return end
    if input.KeyCode == Enum.KeyCode.F1 then aimToggle.MouseButton1Click:Click()
    elseif input.KeyCode == Enum.KeyCode.F2 then espToggle.MouseButton1Click:Click()
    elseif input.KeyCode == Enum.KeyCode.F3 then wallToggle.MouseButton1Click:Click()
    elseif input.KeyCode == Enum.KeyCode.F4 then hpToggle.MouseButton1Click:Click()
    elseif input.KeyCode == Enum.KeyCode.F5 then flyToggle.MouseButton1Click:Click()
    elseif input.KeyCode == Enum.KeyCode.F7 then fovToggle.MouseButton1Click:Click()
    elseif input.KeyCode == Enum.KeyCode.F9 then teamToggle.MouseButton1Click:Click()
    end
end)

-- ========== UI CONNECTIONS ==========
local function updateToggle(btn, on)
    btn.Text = on and "ON" or "OFF"
    btn.BackgroundColor3 = on and Color3.fromRGB(0, 180, 0) or Color3.fromRGB(40, 40, 50)
end

local function makeToggleHandler(toggleVar, stateKey, featureName)
    return function()
        if not isAlive then return end
        state[stateKey] = not state[stateKey]
        updateToggle(toggleVar, state[stateKey])
        local status = state[stateKey] and "ON" or "OFF"
        showNotification(featureName, "Turned " .. status, not state[stateKey], false)
        if stateKey == "esp" then updateESP() end
        if stateKey == "lockEnabled" and not state[stateKey] then
            lockedTarget = nil
        end
        if stateKey == "fly" then
            if state[stateKey] then
                startFly()
            else
                stopFly()   -- instant stop
            end
        end
        if stateKey == "noclip" then
            updateNoclip()
        end
    end
end

aimToggle.MouseButton1Click:Connect(makeToggleHandler(aimToggle, "aim", "Aimbot"))
lockToggle.MouseButton1Click:Connect(makeToggleHandler(lockToggle, "lockEnabled", "Lock-on"))
teamToggle.MouseButton1Click:Connect(makeToggleHandler(teamToggle, "teamCheck", "Team Check"))
wallToggle.MouseButton1Click:Connect(makeToggleHandler(wallToggle, "wall", "Wall Check"))
hpToggle.MouseButton1Click:Connect(makeToggleHandler(hpToggle, "hp", "HP Check"))
fovToggle.MouseButton1Click:Connect(function()
    if not isAlive then return end
    state.fovVis = not state.fovVis
    updateToggle(fovToggle, state.fovVis)
    fovCircle.Visible = state.fovVis
    if state.fovVis then fovCircle.Position = Vector2.new(Camera.ViewportSize.X/2, Camera.ViewportSize.Y/2) end
    showNotification("FOV Circle", "Turned " .. (state.fovVis and "ON" or "OFF"), not state.fovVis, false)
end)
espToggle.MouseButton1Click:Connect(makeToggleHandler(espToggle, "esp", "ESP"))
noclipToggle.MouseButton1Click:Connect(makeToggleHandler(noclipToggle, "noclip", "Noclip"))
flyToggle.MouseButton1Click:Connect(makeToggleHandler(flyToggle, "fly", "Fly"))

fovInput.FocusLost:Connect(function() if isAlive then local v = tonumber(fovInput.Text); if v and v > 0 then state.fovSize = v; fovCircle.Radius = v else fovInput.Text = tostring(state.fovSize) end end end)
smoothInput.FocusLost:Connect(function() if isAlive then local v = tonumber(smoothInput.Text); if v then state.smoothness = math.clamp(v,1,10) else smoothInput.Text = tostring(state.smoothness) end end end)
partInput.FocusLost:Connect(function() if isAlive then local v = partInput.Text; if v == "Head" or v == "Torso" then state.aimPart = v else partInput.Text = state.aimPart end end end)

resetPosBtn.MouseButton1Click:Connect(function() if isAlive then mainFrame.Position = UDim2.new(0.5, -190, 0.5, -260) end end)
uninjectBtn.MouseButton1Click:Connect(fullCleanup)
hideBtn.MouseButton1Click:Connect(toggleMenu)
menuKeyBtn.MouseButton1Click:Connect(startKeybinding)
lockKeyBtn.MouseButton1Click:Connect(startLockKeybinding)

-- Tab switching
local function switchTab(tab)
    if not isAlive then return end
    aimContent.Visible = (tab == "aim")
    visContent.Visible = (tab == "vis")
    playerContent.Visible = (tab == "player")
    settingsContent.Visible = (tab == "settings")
    aimBtn.TextColor3 = (tab == "aim") and Color3.fromRGB(0, 255, 200) or Color3.fromRGB(200, 200, 200)
    visBtn.TextColor3 = (tab == "vis") and Color3.fromRGB(0, 255, 200) or Color3.fromRGB(200, 200, 200)
    playerBtn.TextColor3 = (tab == "player") and Color3.fromRGB(0, 255, 200) or Color3.fromRGB(200, 200, 200)
    settingsBtn.TextColor3 = (tab == "settings") and Color3.fromRGB(0, 255, 200) or Color3.fromRGB(200, 200, 200)
end
aimBtn.MouseButton1Click:Connect(function() switchTab("aim") end)
visBtn.MouseButton1Click:Connect(function() switchTab("vis") end)
playerBtn.MouseButton1Click:Connect(function() switchTab("player") end)
settingsBtn.MouseButton1Click:Connect(function() switchTab("settings") end)

-- Camera resize
Camera:GetPropertyChangedSignal("ViewportSize"):Connect(function()
    if isAlive and state.fovVis then fovCircle.Position = Vector2.new(Camera.ViewportSize.X/2, Camera.ViewportSize.Y/2) end
end)

-- Player events
Players.PlayerAdded:Connect(function(p)
    if not isAlive then return end
    p.CharacterAdded:Connect(function() if isAlive then task.wait(0.5); updateESP(); updateNoclip() end end)
    updateESP()
end)
Players.PlayerRemoving:Connect(function() if isAlive then clearESP() end end)

LocalPlayer.CharacterAdded:Connect(function()
    if not isAlive then return end
    if state.fly then
        stopFly()
        task.wait(0.1)
        startFly()
    end
    if state.noclip then
        task.wait(0.1)
        updateNoclip()
        setupNoclipHandler()
    end
    task.wait(0.5)
    updateESP()
end)

-- Main loops
RunService.RenderStepped:Connect(function()
    if not isAlive then return end
    if state.esp then updateESP() end
    if state.fly then updateFly() end
    if state.aim then
        local target = nil
        if state.lockEnabled then
            if lockedTarget and lockedTarget.Character and isAliveChar(lockedTarget.Character) and isEnemy(lockedTarget) then
                target = lockedTarget
            else
                if lockedTarget then lockedTarget = nil end
                target = nil
            end
        else
            target = getClosestEnemy()
        end
        if target and target.Character then
            local part = getTargetPart(target.Character)
            if part then aimAt(part.Position) end
        end
    end
    if state.fovVis then
        fovCircle.Position = Vector2.new(Camera.ViewportSize.X/2, Camera.ViewportSize.Y/2)
        fovCircle.Visible = true
    else
        fovCircle.Visible = false
    end
end)

-- Initial setup
switchTab("aim")
updateToggle(lockToggle, state.lockEnabled)
updateToggle(flyToggle, state.fly)
updateToggle(noclipToggle, state.noclip)
setupNoclipHandler()

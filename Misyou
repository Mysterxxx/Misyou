-- misyou waterpark fly gui by misyou
local player = game.Players.LocalPlayer
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")

local flying, flySpeed = false, 150
local hrp, humanoid, bodyVelocity
local moveY = 0

-- GUI setup
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "misyouWaterparkFly"
if syn and syn.protect_gui then syn.protect_gui(screenGui) end
pcall(function() screenGui.Parent = game:GetService("CoreGui") end)
if not screenGui.Parent then screenGui.Parent = player:WaitForChild("PlayerGui") end

local frame = Instance.new("Frame", screenGui)
frame.Size = UDim2.new(0, 270, 0, 285)
frame.Position = UDim2.new(0, 60, 0, 60)
frame.BackgroundColor3 = Color3.fromRGB(47, 173, 231)
frame.BorderSizePixel = 0
Instance.new("UICorner", frame).CornerRadius = UDim.new(0, 18)
frame.BackgroundTransparency = 0.10

local header = Instance.new("Frame", frame)
header.Size = UDim2.new(1, -14, 0, 40)
header.Position = UDim2.new(0,7,0,7)
header.BackgroundColor3 = Color3.fromRGB(48, 116, 199)
header.BorderSizePixel = 0
Instance.new("UICorner", header).CornerRadius = UDim.new(0, 10)

local title = Instance.new("TextLabel", header)
title.Size = UDim2.new(1, -50, 1, 0)
title.Position = UDim2.new(0, 10, 0, 0)
title.Text = "🌊 misyou waterpark"
title.BackgroundTransparency = 1
title.TextColor3 = Color3.fromRGB(255,255,255)
title.TextScaled = true
title.Font = Enum.Font.FredokaOne

local closeBtn = Instance.new("TextButton", header)
closeBtn.Size = UDim2.new(0, 34, 1, 0)
closeBtn.Position = UDim2.new(1, -44, 0, 0)
closeBtn.Text = "✕"
closeBtn.TextColor3 = Color3.fromRGB(255,255,255)
closeBtn.BackgroundColor3 = Color3.fromRGB(230,80,80)
closeBtn.Font = Enum.Font.FredokaOne
closeBtn.TextScaled = true
Instance.new("UICorner", closeBtn).CornerRadius = UDim.new(0, 8)

local logoBtn = Instance.new("TextButton", screenGui)
logoBtn.Size = UDim2.new(0, 54, 0, 54)
logoBtn.Position = UDim2.new(0, 60, 0, 60)
logoBtn.Text = "🌊"
logoBtn.Visible = false
logoBtn.BackgroundColor3 = Color3.fromRGB(50, 200, 255)
logoBtn.TextColor3 = Color3.fromRGB(255,255,255)
logoBtn.Font = Enum.Font.FredokaOne
logoBtn.TextScaled = true
Instance.new("UICorner", logoBtn).CornerRadius = UDim.new(1, 0)

local content = Instance.new("Frame", frame)
content.Size = UDim2.new(1, -14, 1, -74)
content.Position = UDim2.new(0,7,0,49)
content.BackgroundTransparency = 1

local contentLayout = Instance.new("UIListLayout", content)
contentLayout.Padding = UDim.new(0, 7)
contentLayout.HorizontalAlignment = Enum.HorizontalAlignment.Center

local function createButton(txt,color,callback)
    local btn = Instance.new("TextButton", content)
    btn.Size = UDim2.new(1, -22, 0, 43)
    btn.Text = txt
    btn.BackgroundColor3 = color
    btn.TextColor3 = Color3.fromRGB(255,255,255)
    btn.Font = Enum.Font.FredokaOne
    btn.TextScaled = true
    Instance.new("UICorner", btn).CornerRadius = UDim.new(0, 10)
    btn.MouseButton1Click:Connect(callback)
    btn.AutoButtonColor = true
    return btn
end

local function toggleFly()
    if not player.Character then return end
    hrp = player.Character:FindFirstChild("HumanoidRootPart")
    humanoid = player.Character:FindFirstChild("Humanoid")
    if not hrp or not humanoid then return end
    flying = not flying
    if flying then
        bodyVelocity = Instance.new("BodyVelocity")
        bodyVelocity.MaxForce = Vector3.new(1e5,1e5,1e5)
        bodyVelocity.Velocity = Vector3.new(0,0,0)
        bodyVelocity.Parent = hrp
    else
        if bodyVelocity then bodyVelocity:Destroy() bodyVelocity = nil end
    end
end

local invincible = false
local function toggleInvincible()
    if not player.Character then return end
    humanoid = player.Character:FindFirstChild("Humanoid")
    if not humanoid then return end
    invincible = not invincible
    if invincible then
        spawn(function()
            while invincible do
                if humanoid.Health < humanoid.MaxHealth then
                    humanoid.Health = humanoid.MaxHealth
                end
                wait(0.05)
            end
        end)
        humanoid:SetStateEnabled(Enum.HumanoidStateType.FallingDown, false)
        humanoid:SetStateEnabled(Enum.HumanoidStateType.Freefall, false)
    else
        humanoid:SetStateEnabled(Enum.HumanoidStateType.FallingDown, true)
        humanoid:SetStateEnabled(Enum.HumanoidStateType.Freefall, true)
    end
end

RunService.RenderStepped:Connect(function()
    if flying and hrp and humanoid and bodyVelocity then
        local moveDir = humanoid.MoveDirection
        local moveVector = Vector3.new(moveDir.X, moveY, moveDir.Z)
        if moveVector.Magnitude > 0 then
            bodyVelocity.Velocity = moveVector.Unit * flySpeed
        else
            bodyVelocity.Velocity = Vector3.new(0,0,0)
        end
    end
end)

local function showFlyMenu()
    for _,v in pairs(content:GetChildren()) do
        if v:IsA("GuiObject") then v:Destroy() end
    end
    title.Text = "🌊 misyou waterpark"
    createButton("✈️ Aktif/Nonaktif Fly", Color3.fromRGB(46, 204, 113), toggleFly)
    createButton("🛡️ Aktif/Nonaktif Kebal", Color3.fromRGB(241, 196, 15), toggleInvincible)
end

-- Drag & drop
local dragging, dragInput, dragStart, startPos
frame.Active = true
frame.Draggable = false
local function update(input)
    local delta = input.Position - dragStart
    frame.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
end
header.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
        dragging = true
        dragStart = input.Position
        startPos = frame.Position
        input.Changed:Connect(function()
            if input.UserInputState == Enum.UserInputState.End then
                dragging = false
            end
        end)
    end
end)
header.InputChanged:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch then
        dragInput = input
    end
end)
UserInputService.InputChanged:Connect(function(input)
    if input == dragInput and dragging then update(input) end
end)

closeBtn.MouseButton1Click:Connect(function()
    frame.Visible = false
    logoBtn.Visible = true
    logoBtn.Position = frame.Position
end)
logoBtn.MouseButton1Click:Connect(function()
    frame.Visible = true
    logoBtn.Visible = false
end)

local function createDraggableButton(text, color, pos, parent)
    local btn = Instance.new("TextButton")
    btn.Size = UDim2.new(0,62,0,62)
    btn.Position = pos
    btn.Text = text
    btn.BackgroundColor3 = color
    btn.TextScaled = true
    btn.Font = Enum.Font.FredokaOne
    btn.TextColor3 = Color3.fromRGB(255,255,255)
    Instance.new("UICorner", btn).CornerRadius = UDim.new(1,0)
    btn.Parent = parent

    local dragging, dragInput, dragStart, startPos
    btn.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.Touch then
            dragging = true
            dragStart = input.Position
            startPos = btn.Position
            input.Changed:Connect(function()
                if input.UserInputState == Enum.UserInputState.End then
                    dragging = false
                end
            end)
        end
    end)
    btn.InputChanged:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.Touch then
            dragInput = input
        end
    end)
    UserInputService.InputChanged:Connect(function(input)
        if input == dragInput and dragging then
            local delta = input.Position - dragStart
            btn.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
        end
    end)
    return btn
end

local upBtn = createDraggableButton("⬆️", Color3.fromRGB(46, 204, 113), UDim2.new(0.89,0,0.7,0), screenGui)
local downBtn = createDraggableButton("⬇️", Color3.fromRGB(230,80,80), UDim2.new(0.89,0,0.81,0), screenGui)

upBtn.MouseButton1Down:Connect(function() moveY = 1 end)
upBtn.MouseButton1Up:Connect(function() moveY = 0 end)
downBtn.MouseButton1Down:Connect(function() moveY = -1 end)
downBtn.MouseButton1Up:Connect(function() moveY = 0 end)

showFlyMenu()

-- Watermark
local watermark = Instance.new("TextLabel", frame)
watermark.Size = UDim2.new(1, -12, 0, 20)
watermark.Position = UDim2.new(0, 6, 1, -24)
watermark.BackgroundTransparency = 1
watermark.Text = "Made by misyou"
watermark.TextColor3 = Color3.fromRGB(255,255,255)
watermark.TextStrokeTransparency = 0.7
watermark.TextScaled = true
watermark.Font = Enum.Font.FredokaOne

-- Services
local Players = game:GetService("Players")
local Workspace = game:GetService("Workspace")
local TweenService = game:GetService("TweenService")
local UserInputService = game:GetService("UserInputService")
local RunService = game:GetService("RunService")

-- Player Setup
local player = Players.LocalPlayer
local char = player.Character or player.CharacterAdded:Wait()
local hrp = char:WaitForChild("HumanoidRootPart")
local random = Random.new()

-- Teleport System
local tpAmt = 70
local teleporting = false
local void = CFrame.new(0, -3.4e+38, 0)

-- Dynamic ping adjustment
task.spawn(function()
    while task.wait(0.5) do
        local ping = player:GetNetworkPing() * 1000
        tpAmt = math.clamp(math.floor(ping * 0.8), 10, 150)
    end
end)

local function SafeTP(cframe)
    if not cframe or teleporting or not player.Character then return end
    teleporting = true
    
    local hrp = player.Character:FindFirstChild("HumanoidRootPart")
    if not hrp then 
        teleporting = false
        return 
    end
    
    local success = pcall(function()
        hrp.CFrame = cframe + Vector3.new(
            random:NextNumber(-0.0001, 0.0001),
            random:NextNumber(-0.0001, 0.0001),
            random:NextNumber(-0.0001, 0.0001)
        )
        task.wait()
    end)
    
    teleporting = false
    return success
end

-- Enhanced Functions
local function DeliverBrainrot()
    if not player.Character then return end
    local hrp = player.Character:FindFirstChild("HumanoidRootPart")
    if not hrp then return end

    for _, plot in pairs(Workspace.Plots:GetChildren()) do
        local sign = plot:FindFirstChild("PlotSign")
        if sign and sign:FindFirstChild("YourBase") and sign.YourBase.Enabled then
            local hitbox = plot:FindFirstChild("DeliveryHitbox")
            if hitbox then
                local target = hitbox.CFrame * CFrame.new(0, -3, 0)
                
                -- Arbix teleport sequence
                for i = 1, tpAmt do SafeTP(target) end
                for _ = 1, 2 do SafeTP(void) end
                for i = 1, math.floor(tpAmt/16) do SafeTP(target) end
                
                return true
            end
        end
    end
    return false
end

local function TPNearestBase()
    if not player.Character then return end
    local hrp = player.Character:FindFirstChild("HumanoidRootPart")
    if not hrp then return end

    local closest, minDist = nil, math.huge
    
    for _, plot in pairs(Workspace.Plots:GetChildren()) do
        local sign = plot:FindFirstChild("PlotSign")
        if sign and sign:FindFirstChild("YourBase") and not sign.YourBase.Enabled then
            local podiums = plot:FindFirstChild("AnimalPodiums")
            if podiums then
                for _, podium in pairs(podiums:GetChildren()) do
                    if podium:IsA("Model") and podium:FindFirstChild("Base") then
                        local spawn = podium.Base:FindFirstChild("Spawn")
                        if spawn then
                            local dist = (spawn.Position - hrp.Position).Magnitude
                            if dist < minDist then
                                minDist = dist
                                closest = spawn
                            end
                        end
                    end
                end
            end
        end
    end

    if closest then
        local target = closest.CFrame * CFrame.new(0, 2, 0)
        
        -- teleport sequence
        for i = 1, tpAmt do SafeTP(target) end
        for _ = 1, 2 do SafeTP(void) end
        for i = 1, math.floor(tpAmt/16) do SafeTP(target) end
        
        return true
    end
    return false
end

local function TweenSteal()
    if not player.Character then return end
    local hrp = player.Character:FindFirstChild("HumanoidRootPart")
    if not hrp then return end

    local function SmoothMove(target, steps)
        local startPos = hrp.Position
        for i = 1, steps do
            local t = i/steps
            local smoothT = t * t * (3 - 2 * t) -- Cubic easing
            local newPos = startPos:Lerp(target.Position, smoothT) + 
                Vector3.new(
                    random:NextNumber(-0.0002, 0.0002),
                    random:NextNumber(-0.0002, 0.0002),
                    random:NextNumber(-0.0002, 0.0002)
                )
            hrp.CFrame = CFrame.new(newPos) * (hrp.CFrame - hrp.Position)
            task.wait(0.01)
        end
    end

    for _, plot in pairs(Workspace.Plots:GetChildren()) do
        local sign = plot:FindFirstChild("PlotSign")
        if sign and sign:FindFirstChild("YourBase") and sign.YourBase.Enabled then
            local hitbox = plot:FindFirstChild("DeliveryHitbox")
            if hitbox then
                local target = hitbox.CFrame * CFrame.new(0, -2, 0)
                SmoothMove(target, 85)
                
                -- Void flicker
                for _ = 1, 2 do
                    SafeTP(void)
                    task.wait(0.05)
                    SafeTP(target)
                    task.wait(0.05)
                end
                
                return true
            end
        end
    end
    return false
end

-- UI Setup (N0X HUB)
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "N0XHubGui"
screenGui.ResetOnSpawn = false
screenGui.Parent = player:WaitForChild("PlayerGui")

local width = 430
local initialHeight = 130
local expandedHeight = 300

-- Notification function
local function showNotification(text)
    local notif = Instance.new("TextLabel")
    notif.Text = text
    notif.Font = Enum.Font.GothamBold
    notif.TextSize = 18
    notif.Size = UDim2.new(0, 200, 0, 40)
    notif.Position = UDim2.new(1, -210, 1, -50)
    notif.AnchorPoint = Vector2.new(1, 1)
    notif.BackgroundColor3 = Color3.fromRGB(40, 20, 80)
    notif.TextColor3 = Color3.fromRGB(220, 190, 255)
    notif.BorderSizePixel = 0
    notif.Parent = screenGui
    
    Instance.new("UICorner", notif).CornerRadius = UDim.new(0, 8)
    Instance.new("UIStroke", notif).Color = Color3.fromRGB(150, 50, 255)
    
    task.delay(3, function()
        local tween = TweenService:Create(notif, TweenInfo.new(0.5), {TextTransparency = 1, BackgroundTransparency = 1})
        tween:Play()
        tween.Completed:Wait()
        notif:Destroy()
    end)
end

-- Main Frame
local mainFrame = Instance.new("Frame")
mainFrame.Name = "MainFrame"
mainFrame.Size = UDim2.new(0, width, 0, initialHeight)
mainFrame.Position = UDim2.new(0.5, 0, 0.5, 0)
mainFrame.AnchorPoint = Vector2.new(0.5, 0.5)
mainFrame.BackgroundColor3 = Color3.fromRGB(30, 10, 60)
mainFrame.BorderSizePixel = 0
mainFrame.Parent = screenGui

local corner = Instance.new("UICorner", mainFrame)
corner.CornerRadius = UDim.new(0, 16)

local stroke = Instance.new("UIStroke", mainFrame)
stroke.Color = Color3.fromRGB(150, 50, 255)
stroke.Thickness = 3
stroke.Transparency = 0.15

-- Perfect Close Button (FredokaOne, lowercase x, corner position)
local closeButton = Instance.new("TextButton", mainFrame)
closeButton.Name = "CloseButton"
closeButton.Text = "x"
closeButton.Font = Enum.Font.FredokaOne
closeButton.TextSize = 22
closeButton.Size = UDim2.new(0, 26, 0, 26)
closeButton.Position = UDim2.new(1, -10, 0, 6) -- Perfect corner position
closeButton.AnchorPoint = Vector2.new(1, 0)
closeButton.BackgroundColor3 = Color3.fromRGB(80, 30, 120)
closeButton.TextColor3 = Color3.fromRGB(220, 190, 255)
closeButton.BorderSizePixel = 0

-- Button Styling
local closeButtonCorner = Instance.new("UICorner", closeButton)
closeButtonCorner.CornerRadius = UDim.new(0, 8)

local closeButtonStroke = Instance.new("UIStroke", closeButton)
closeButtonStroke.Color = Color3.fromRGB(180, 100, 255)
closeButtonStroke.Thickness = 2
closeButtonStroke.Transparency = 0.3

-- Button Effects
local function animateButton(button, isHovering)
    local targetColor = isHovering and Color3.fromRGB(110, 50, 180) or Color3.fromRGB(80, 30, 120)
    local targetTextColor = isHovering and Color3.fromRGB(255, 220, 255) or Color3.fromRGB(220, 190, 255)
    
    TweenService:Create(button, TweenInfo.new(0.2), {
        BackgroundColor3 = targetColor,
        TextColor3 = targetTextColor
    }):Play()
end

closeButton.MouseEnter:Connect(function()
    animateButton(closeButton, true)
end)

closeButton.MouseLeave:Connect(function()
    animateButton(closeButton, false)
end)

closeButton.MouseButton1Click:Connect(function()
    -- Click animation
    TweenService:Create(closeButton, TweenInfo.new(0.1), {
        Size = UDim2.new(0, 24, 0, 24),
        BackgroundColor3 = Color3.fromRGB(150, 70, 220)
    }):Play()
    task.wait(0.1)
    TweenService:Create(closeButton, TweenInfo.new(0.1), {
        Size = UDim2.new(0, 26, 0, 26),
        BackgroundColor3 = Color3.fromRGB(80, 30, 120)
    }):Play()
    
    mainFrame.Visible = false
    showNotification("UI Hidden (Press Left Ctrl)")
end)

-- Make UI draggable
local function makeDraggable(frame)
    local dragging, dragInput, dragStart, startPos
    frame.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 then
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
    frame.InputChanged:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseMovement then
            dragInput = input
        end
    end)
    UserInputService.InputChanged:Connect(function(input)
        if input == dragInput and dragging then
            local delta = input.Position - dragStart
            frame.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X,
                startPos.Y.Scale, startPos.Y.Offset + delta.Y)
        end
    end)
end
makeDraggable(mainFrame)

-- Header
local header = Instance.new("TextLabel", mainFrame)
header.Text = "🧠 N0X HUB"
header.Font = Enum.Font.GothamBlack
header.TextSize = 24
header.Size = UDim2.new(1, 0, 0, 42)
header.BackgroundTransparency = 1
header.TextColor3 = Color3.fromRGB(220, 190, 255)
header.TextStrokeTransparency = 0
header.TextStrokeColor3 = Color3.fromRGB(0, 0, 0)

-- Key input box
local keyBox = Instance.new("TextBox", mainFrame)
keyBox.Size = UDim2.new(0.8, 0, 0, 32)
keyBox.Position = UDim2.new(0.1, 0, 0, 50)
keyBox.PlaceholderText = "Enter your key here"
keyBox.Text = ""
keyBox.ClearTextOnFocus = false
keyBox.Font = Enum.Font.Gotham
keyBox.TextSize = 16
keyBox.TextColor3 = Color3.fromRGB(230, 230, 230)
keyBox.BackgroundColor3 = Color3.fromRGB(50, 25, 90)
keyBox.BorderSizePixel = 0
local keyCorner = Instance.new("UICorner", keyBox)
keyCorner.CornerRadius = UDim.new(0, 10)

local placeholderRemoved = false
keyBox.Focused:Connect(function()
    if not placeholderRemoved then
        keyBox.Text = ""
        placeholderRemoved = true
    end
end)

-- Submit button
local submitButton = Instance.new("TextButton", mainFrame)
submitButton.Size = UDim2.new(0.4, 0, 0, 32)
submitButton.Position = UDim2.new(0.3, 0, 0, 90)
submitButton.Text = "Submit Key"
submitButton.Font = Enum.Font.GothamBold
submitButton.TextSize = 16
submitButton.BackgroundColor3 = Color3.fromRGB(110, 40, 220)
submitButton.TextColor3 = Color3.new(1, 1, 1)
submitButton.BorderSizePixel = 0
local submitCorner = Instance.new("UICorner", submitButton)
submitCorner.CornerRadius = UDim.new(0, 10)

local VALID_KEYS = {
    ["N0XHub"] = true,
    ["N0XHUB"] = true,
    ["noxhub"] = true,
    ["N0XHUB"] = true,
    ["n0x hub"] = true,
    ["N0X Hub"] = true,
    ["NOX Hub"] = true,
    ["n0x hub"] = true,
    ["n0xHUB"] = true,
    ["noxHUB"] = true,
    ["n0xhub"] = true
}

-- Load full menu
local function loadMainButtons()
    TweenService:Create(mainFrame, TweenInfo.new(0.4), {Size = UDim2.new(0, width, 0, expandedHeight)}):Play()
    task.wait(0.4)
    
    -- Clear all children except the close button
    for _, child in ipairs(mainFrame:GetChildren()) do
        if child ~= closeButton then
            child:Destroy()
        end
    end

    local corner = Instance.new("UICorner", mainFrame)
    corner.CornerRadius = UDim.new(0, 16)
    local stroke = Instance.new("UIStroke", mainFrame)
    stroke.Color = Color3.fromRGB(150, 50, 255)
    stroke.Thickness = 3
    stroke.Transparency = 0.15
    makeDraggable(mainFrame)

    local header = Instance.new("TextLabel", mainFrame)
    header.Text = "🧠 N0X HUB"
    header.Font = Enum.Font.GothamBlack
    header.TextSize = 24
    header.Size = UDim2.new(1, 0, 0, 42)
    header.BackgroundTransparency = 1
    header.TextColor3 = Color3.fromRGB(220, 190, 255)
    header.TextStrokeTransparency = 0
    header.TextStrokeColor3 = Color3.fromRGB(0, 0, 0)

    local buttons = {
        {Text = "TP Nearest Base", Func = TPNearestBase},
        {Text = "Steal", Func = DeliverBrainrot},
        {Text = "Bypass Anticheat", Func = TweenSteal},
    }

    for i, data in ipairs(buttons) do
        local btn = Instance.new("TextButton", mainFrame)
        btn.Size = UDim2.new(0.9, 0, 0, 40)
        btn.Position = UDim2.new(0.05, 0, 0, 60 + (i - 1) * 52)
        btn.BackgroundColor3 = Color3.fromRGB(110, 40, 220)
        btn.TextColor3 = Color3.fromRGB(255, 255, 255)
        btn.Font = Enum.Font.GothamBold
        btn.TextSize = 18
        btn.Text = data.Text
        btn.BorderSizePixel = 0
        Instance.new("UICorner", btn).CornerRadius = UDim.new(0, 8)
        btn.MouseButton1Click:Connect(function()
            pcall(data.Func)
        end)
    end
end

-- Key submit logic
submitButton.MouseButton1Click:Connect(function()
    local key = keyBox.Text
    if VALID_KEYS[key] then
        keyBox:Destroy()
        submitButton:Destroy()
        loadMainButtons()
    else
        keyBox.Text = ""
        keyBox.PlaceholderText = "Invalid key! Try again."
    end
end)

-- Left Ctrl toggle for UI
UserInputService.InputBegan:Connect(function(input, gameProcessed)
    if not gameProcessed and input.KeyCode == Enum.KeyCode.LeftControl then
        mainFrame.Visible = not mainFrame.Visible
        if not mainFrame.Visible then
            showNotification("UI Hidden (Press Left Ctrl)")
        end
    end
end)

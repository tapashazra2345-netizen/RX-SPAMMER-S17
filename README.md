local function D(s) return s:reverse() end

local function sendMsg(msg)
    pcall(function()
        local chat = game:GetService("TextChatService")
        if chat.ChatVersion == Enum.ChatVersion.TextChatService then
            local channel = chat.TextChannels:FindFirstChild("RBXGeneral")
            if channel then channel:SendAsync(msg) end
        else
            game:GetService("ReplicatedStorage"):FindFirstChild("DefaultChatSystemChatEvents"):FindFirstChild("SayMessageRequest"):FireServer(msg, "All")
        end
    end)
end

local function getParentGui()
    local success, result = pcall(function() return game:GetService("CoreGui") end)
    if success and result then return result end
    return game.Players.LocalPlayer:WaitForChild("PlayerGui")
end

-- Setup ScreenGui
local sg = Instance.new("ScreenGui")
sg.Name = "RX_S16"
sg.ResetOnSpawn = false
sg.Parent = getParentGui()
for _, v in pairs(getParentGui():GetChildren()) do
    if v.Name == "RX_S16" and v ~= sg then v:Destroy() end
end

-- ===== LOADING SCREEN =====
local loadFrame = Instance.new("Frame")
loadFrame.Size = UDim2.new(0, 300, 0, 100)
loadFrame.Position = UDim2.new(0.5, -150, 0.5, -50)
loadFrame.BackgroundColor3 = Color3.fromRGB(10, 10, 20)
loadFrame.BorderSizePixel = 2
loadFrame.BorderColor3 = Color3.fromRGB(0, 120, 255)
loadFrame.Parent = sg

local loadCorner = Instance.new("UICorner")
loadCorner.CornerRadius = UDim.new(0, 8)
loadCorner.Parent = loadFrame

local loadTitle = Instance.new("TextLabel")
loadTitle.Size = UDim2.new(1, 0, 0, 30)
loadTitle.Position = UDim2.new(0, 0, 0, 10)
loadTitle.BackgroundTransparency = 1
loadTitle.Text = D("61S REMMAPS XR")
loadTitle.TextColor3 = Color3.fromRGB(0, 120, 255)
loadTitle.TextSize = 18
loadTitle.Font = Enum.Font.GothamBold
loadTitle.Parent = loadFrame

local loadSub = Instance.new("TextLabel")
loadSub.Size = UDim2.new(1, 0, 0, 20)
loadSub.Position = UDim2.new(0, 0, 0, 35)
loadSub.BackgroundTransparency = 1
loadSub.Text = D("...METSYS GNIZILAITINI")
loadSub.TextColor3 = Color3.fromRGB(200, 200, 200)
loadSub.TextSize = 12
loadSub.Font = Enum.Font.Gotham
loadSub.Parent = loadFrame

local barBg = Instance.new("Frame")
barBg.Size = UDim2.new(0.8, 0, 0, 6)
barBg.Position = UDim2.new(0.1, 0, 0, 65)
barBg.BackgroundColor3 = Color3.fromRGB(20, 20, 40)
barBg.BorderSizePixel = 0
barBg.Parent = loadFrame

local bar = Instance.new("Frame")
bar.Size = UDim2.new(0, 0, 1, 0)
bar.BackgroundColor3 = Color3.fromRGB(0, 120, 255)
bar.BorderSizePixel = 0
bar.Parent = barBg

-- ===== MAIN WINDOW =====
local mainFrame = Instance.new("Frame")
mainFrame.Size = UDim2.new(0, 420, 0, 195)
mainFrame.Position = UDim2.new(0.5, -210, 0.4, -98)
mainFrame.BackgroundColor3 = Color3.fromRGB(10, 10, 20)
mainFrame.BorderSizePixel = 2
mainFrame.BorderColor3 = Color3.fromRGB(0, 120, 255)
mainFrame.Draggable = true
mainFrame.Active = true
mainFrame.Visible = false
mainFrame.Parent = sg

local mainCorner = Instance.new("UICorner")
mainCorner.CornerRadius = UDim.new(0, 6)
mainCorner.Parent = mainFrame

local topBar = Instance.new("Frame")
topBar.Size = UDim2.new(1, 0, 0, 32)
topBar.BackgroundColor3 = Color3.fromRGB(15, 15, 18)
topBar.BorderSizePixel = 0
topBar.Parent = mainFrame

local topLabel = Instance.new("TextLabel")
topLabel.Size = UDim2.new(1, -40, 1, 0)
topLabel.Position = UDim2.new(0, 15, 0, 0)
topLabel.BackgroundTransparency = 1
topLabel.Text = D("NAYR & TIMUS | 61S REMMAPS XR")
topLabel.TextColor3 = Color3.fromRGB(0, 120, 255)
topLabel.TextSize = 13
topLabel.Font = Enum.Font.GothamBold
topLabel.TextXAlignment = Enum.TextXAlignment.Left
topLabel.Parent = topBar

local minBtn = Instance.new("TextButton")
minBtn.Size = UDim2.new(0, 30, 0, 30)
minBtn.Position = UDim2.new(1, -35, 0, 1)
minBtn.BackgroundColor3 = Color3.fromRGB(20, 20, 25)
minBtn.TextColor3 = Color3.new(1, 1, 1)
minBtn.Text = "_"
minBtn.TextSize = 16
minBtn.Font = Enum.Font.GothamBold
minBtn.BorderSizePixel = 0
minBtn.Parent = topBar

-- Settings
getgenv().Target = ""
getgenv().Prefix = "@"   -- default @ as you requested
getgenv().Count = 150
getgenv().Delay = 3.0
getgenv().Running = false

-- ✅ FIXED: 'RYXN' → 'RYAN'
local messages = {
    "TMX MARE RYAN AND SUMIT PAPA",   -- corrected
    "TMX MEH ROAD ROLLER",
    "TMX MARE VIYAY THALAPATHY",
    "TMX MARE WILD GORILA",
    "TMX MARE PHANTER",
    "TMX MEH OBSIDIAN",
    "TMX MEH FLAME THROWER",
    "TMX MEH LASAN",
    "TMX MEH MAGGI  🍜",
    "TMX MEH MERA DIL ❤"
}
local msgIndex = 1
local yPos = 40

local function addField(labelText, defaultValue, callback)
    local lbl = Instance.new("TextLabel")
    lbl.Size = UDim2.new(0, 80, 0, 20)
    lbl.Position = UDim2.new(0, 15, 0, yPos)
    lbl.BackgroundTransparency = 1
    lbl.Text = D(labelText)
    lbl.TextColor3 = Color3.new(1, 1, 1)
    lbl.TextSize = 11
    lbl.Font = Enum.Font.Gotham
    lbl.TextXAlignment = Enum.TextXAlignment.Left
    lbl.Parent = mainFrame

    local box = Instance.new("TextBox")
    box.Size = UDim2.new(0, 140, 0, 28)
    box.Position = UDim2.new(0, 85, 0, yPos - 4)
    box.Text = tostring(defaultValue)
    box.BackgroundColor3 = Color3.fromRGB(20, 20, 40)
    box.TextColor3 = Color3.new(1, 1, 1)
    box.TextSize = 12
    box.Font = Enum.Font.Gotham
    box.BorderSizePixel = 1
    box.BorderColor3 = Color3.fromRGB(60, 60, 70)
    box.Parent = mainFrame
    box.FocusLost:Connect(function()
        callback(box.Text)
    end)
    yPos = yPos + 35
end

addField(":YMENE", getgenv().Target, function(v) getgenv().Target = v end)
addField(":XIFERP", getgenv().Prefix, function(v) getgenv().Prefix = v end)
addField(":TNUOC", getgenv().Count, function(v) getgenv().Count = tonumber(v) or 150 end)
addField(":YALED", getgenv().Delay, function(v) getgenv().Delay = tonumber(v) or 3.0 end)

local toggleBtn = Instance.new("TextButton")
toggleBtn.Size = UDim2.new(0, 130, 0, 100)
toggleBtn.Position = UDim2.new(1, -155, 0, 60)
toggleBtn.BackgroundColor3 = Color3.fromRGB(0, 120, 255)
toggleBtn.Text = D("tratS")
toggleBtn.TextColor3 = Color3.new(1, 1, 1)
toggleBtn.TextSize = 20
toggleBtn.Font = Enum.Font.GothamBold
toggleBtn.BorderSizePixel = 1
toggleBtn.BorderColor3 = Color3.fromRGB(60, 60, 70)
toggleBtn.Parent = mainFrame

local statusLabel = Instance.new("TextLabel")
statusLabel.Size = UDim2.new(0, 200, 0, 16)
statusLabel.Position = UDim2.new(0, 15, 1, -22)
statusLabel.BackgroundTransparency = 1
statusLabel.TextColor3 = Color3.fromRGB(150, 150, 150)
statusLabel.Text = D("eldI")
statusLabel.TextSize = 10
statusLabel.Font = Enum.Font.Gotham
statusLabel.TextXAlignment = Enum.TextXAlignment.Left
statusLabel.Parent = mainFrame

toggleBtn.MouseButton1Click:Connect(function()
    getgenv().Running = not getgenv().Running
    toggleBtn.Text = getgenv().Running and D("potS") or D("tratS")
    statusLabel.Text = getgenv().Running and D("evitcA") or D("eldI")
end)

local floatBtn = Instance.new("TextButton")
floatBtn.Size = UDim2.new(0, 45, 0, 45)
floatBtn.Position = UDim2.new(0, 10, 0.5, -22)
floatBtn.BackgroundColor3 = Color3.fromRGB(0, 120, 255)
floatBtn.Text = D("XR")
floatBtn.TextColor3 = Color3.new(1, 1, 1)
floatBtn.TextSize = 14
floatBtn.Font = Enum.Font.GothamBold
floatBtn.Visible = false
floatBtn.BorderSizePixel = 0
floatBtn.Parent = sg

local floatCorner = Instance.new("UICorner")
floatCorner.CornerRadius = UDim.new(0, 8)
floatCorner.Parent = floatBtn

minBtn.MouseButton1Click:Connect(function()
    mainFrame.Visible = false
    floatBtn.Visible = true
end)

floatBtn.MouseButton1Click:Connect(function()
    mainFrame.Visible = true
    floatBtn.Visible = false
end)

-- ===== MAIN LOOP =====
task.spawn(function()
    while true do
        if getgenv().Running and #messages > 0 then
            local msg = messages[msgIndex]
            if msg and msg ~= "" then
                local cnt = getgenv().Count + math.random(-5, 5)
                if cnt < 1 then cnt = 1 end
                local finalMsg = string.rep(getgenv().Prefix, cnt) .. " " ..
                                (getgenv().Target ~= "" and "[" .. getgenv().Target .. "] " or "") .. msg
                sendMsg(finalMsg)
                msgIndex = (msgIndex % #messages) + 1
            end
        end
        task.wait(getgenv().Delay)
    end
end)

-- Loading animation
coroutine.wrap(function()
    for i = 1, 30 do
        bar.Size = UDim2.new(i / 30, 0, 1, 0)
        task.wait(0.05)
    end
    loadFrame:Destroy()
    mainFrame.Visible = true
    task.wait(0.5)
    sendMsg("RX SPAMMER S16 LOADED ✅")
end)()

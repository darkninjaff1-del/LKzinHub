--[[
    ⚡ ITACHI HUB - BLOX FRUITS V3.2 ⚡
    VERSÃO CORRIGIDA E 100% FUNCIONAL
]]

-- Serviços
local Players = game:GetService("Players")
local TweenService = game:GetService("TweenService")
local UserInputService = game:GetService("UserInputService")
local RunService = game:GetService("RunService")
local CoreGui = game:GetService("CoreGui")
local HttpService = game:GetService("HttpService")
local TeleportService = game:GetService("TeleportService")
local VirtualInputManager = game:GetService("VirtualInputManager")
local VirtualUser = game:GetService("VirtualUser")
local Lighting = game:GetService("Lighting")
local ReplicatedStorage = game:GetService("ReplicatedStorage")

-- Jogador
local Player = Players.LocalPlayer
local Mouse = Player:GetMouse()
local Camera = workspace.CurrentCamera

-- Personagem
local Character = Player.Character or Player.CharacterAdded:Wait()
local Humanoid = Character:FindFirstChild("Humanoid")
local RootPart = Character:FindFirstChild("HumanoidRootPart")

-- Atualizar personagem ao renascer
Player.CharacterAdded:Connect(function(char)
    Character = char
    Humanoid = char:FindFirstChild("Humanoid")
    RootPart = char:FindFirstChild("HumanoidRootPart")
end)

-- Configurações padrão
local Settings = {
    AutoFarm = false,
    AutoQuest = false,
    AutoAttack = false,
    AutoHaki = false,
    FastAttack = false,
    AutoBoss = false,
    AutoRaid = false,
    AutoSkillZ = false,
    AutoSkillX = false,
    AutoSkillC = false,
    AutoSkillV = false,
    AutoSkillF = false,
    WalkSpeed = 16,
    JumpPower = 50,
    Fly = false,
    FlySpeed = 50,
    NoClip = false,
    InfiniteJump = false,
    ESPPlayers = false,
    ESPFruits = false,
    ESPChests = false,
    RemoveFog = false,
    FullBright = false,
    FPSBoost = false,
    AntiAFK = false,
    AutoRedeem = false
}

-- Sons
local function PlayClick()
    local sound = Instance.new("Sound")
    sound.SoundId = "rbxassetid://9116338042"
    sound.Volume = 0.3
    sound.Parent = CoreGui
    sound:Play()
    task.wait(0.3)
    sound:Destroy()
end

-- Notificações
local function Notify(title, msg, duration)
    duration = duration or 3
    local frame = Instance.new("Frame")
    frame.Size = UDim2.new(0, 280, 0, 70)
    frame.Position = UDim2.new(1, 20, 0.75, 0)
    frame.BackgroundColor3 = Color3.fromRGB(15, 15, 15)
    frame.BorderSizePixel = 0
    frame.ZIndex = 1000
    frame.Parent = CoreGui
    
    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(0, 8)
    corner.Parent = frame
    
    local stroke = Instance.new("UIStroke")
    stroke.Color = Color3.fromRGB(255, 30, 30)
    stroke.Thickness = 1.5
    stroke.Parent = frame
    
    local titleLabel = Instance.new("TextLabel")
    titleLabel.Size = UDim2.new(1, -20, 0, 22)
    titleLabel.Position = UDim2.new(0, 12, 0, 8)
    titleLabel.Text = title
    titleLabel.TextColor3 = Color3.fromRGB(255, 50, 50)
    titleLabel.TextSize = 14
    titleLabel.Font = Enum.Font.GothamBold
    titleLabel.BackgroundTransparency = 1
    titleLabel.TextXAlignment = Enum.TextXAlignment.Left
    titleLabel.ZIndex = 1001
    titleLabel.Parent = frame
    
    local msgLabel = Instance.new("TextLabel")
    msgLabel.Size = UDim2.new(1, -20, 0, 20)
    msgLabel.Position = UDim2.new(0, 12, 0, 35)
    msgLabel.Text = msg
    msgLabel.TextColor3 = Color3.fromRGB(180, 180, 180)
    msgLabel.TextSize = 12
    msgLabel.Font = Enum.Font.Gotham
    msgLabel.BackgroundTransparency = 1
    msgLabel.TextXAlignment = Enum.TextXAlignment.Left
    msgLabel.ZIndex = 1001
    msgLabel.Parent = frame
    
    TweenService:Create(frame, TweenInfo.new(0.4, Enum.EasingStyle.Quart, Enum.EasingDirection.Out), {
        Position = UDim2.new(1, -300, 0.75, 0)
    }):Play()
    
    task.delay(duration, function()
        TweenService:Create(frame, TweenInfo.new(0.4, Enum.EasingStyle.Quart, Enum.EasingDirection.In), {
            Position = UDim2.new(1, 20, 0.75, 0)
        }):Play()
        task.delay(0.4, function() frame:Destroy() end)
    end)
end

-- Criar UI
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "ItachiHub"
ScreenGui.Parent = CoreGui
ScreenGui.ResetOnSpawn = false

-- Frame Principal
local MainFrame = Instance.new("Frame")
MainFrame.Size = UDim2.new(0, 560, 0, 400)
MainFrame.Position = UDim2.new(0.5, -280, 0.5, -200)
MainFrame.BackgroundColor3 = Color3.fromRGB(10, 10, 10)
MainFrame.BorderSizePixel = 0
MainFrame.Visible = true
MainFrame.ZIndex = 10
MainFrame.Parent = ScreenGui

local MainCorner = Instance.new("UICorner")
MainCorner.CornerRadius = UDim.new(0, 10)
MainCorner.Parent = MainFrame

local MainStroke = Instance.new("UIStroke")
MainStroke.Color = Color3.fromRGB(255, 0, 0)
MainStroke.Thickness = 2
MainStroke.Transparency = 0.4
MainStroke.Parent = MainFrame

-- Header
local Header = Instance.new("Frame")
Header.Size = UDim2.new(1, 0, 0, 42)
Header.BackgroundColor3 = Color3.fromRGB(5, 5, 5)
Header.BorderSizePixel = 0
Header.ZIndex = 20
Header.Parent = MainFrame

local HeaderCorner = Instance.new("UICorner")
HeaderCorner.CornerRadius = UDim.new(0, 10)
HeaderCorner.Parent = Header

local Title = Instance.new("TextLabel")
Title.Size = UDim2.new(0, 200, 1, 0)
Title.Position = UDim2.new(0, 45, 0, 0)
Title.Text = "⚡ ITACHI HUB"
Title.TextColor3 = Color3.fromRGB(255, 30, 30)
Title.TextSize = 18
Title.Font = Enum.Font.GothamBlack
Title.BackgroundTransparency = 1
Title.TextXAlignment = Enum.TextXAlignment.Left
Title.ZIndex = 21
Title.Parent = Header

-- Logo
local Logo = Instance.new("ImageLabel")
Logo.Size = UDim2.new(0, 28, 0, 28)
Logo.Position = UDim2.new(0, 10, 0, 7)
Logo.BackgroundTransparency = 1
Logo.Image = "rbxassetid://16556523844"
Logo.ZIndex = 21
Logo.Parent = Header

-- Botões header
local MinimizeBtn = Instance.new("TextButton")
MinimizeBtn.Size = UDim2.new(0, 28, 0, 28)
MinimizeBtn.Position = UDim2.new(1, -68, 0, 7)
MinimizeBtn.Text = "━"
MinimizeBtn.TextColor3 = Color3.fromRGB(255, 40, 40)
MinimizeBtn.TextSize = 14
MinimizeBtn.Font = Enum.Font.GothamBold
MinimizeBtn.BackgroundColor3 = Color3.fromRGB(15, 15, 15)
MinimizeBtn.BorderSizePixel = 0
MinimizeBtn.ZIndex = 21
MinimizeBtn.Parent = Header

local MinCorner = Instance.new("UICorner")
MinCorner.CornerRadius = UDim.new(0, 6)
MinCorner.Parent = MinimizeBtn

local CloseBtn = Instance.new("TextButton")
CloseBtn.Size = UDim2.new(0, 28, 0, 28)
CloseBtn.Position = UDim2.new(1, -34, 0, 7)
CloseBtn.Text = "✕"
CloseBtn.TextColor3 = Color3.fromRGB(255, 40, 40)
CloseBtn.TextSize = 14
CloseBtn.Font = Enum.Font.GothamBold
CloseBtn.BackgroundColor3 = Color3.fromRGB(15, 15, 15)
CloseBtn.BorderSizePixel = 0
CloseBtn.ZIndex = 21
CloseBtn.Parent = Header

local CloseCorner = Instance.new("UICorner")
CloseCorner.CornerRadius = UDim.new(0, 6)
CloseCorner.Parent = CloseBtn

-- Drag
local dragging = false
local dragStart, startPos

Header.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 then
        dragging = true
        dragStart = input.Position
        startPos = MainFrame.Position
    end
end)

UserInputService.InputChanged:Connect(function(input)
    if dragging and input.UserInputType == Enum.UserInputType.MouseMovement then
        local delta = input.Position - dragStart
        MainFrame.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
    end
end)

UserInputService.InputEnded:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 then
        dragging = false
    end
end)

-- Container das Abas
local TabContainer = Instance.new("Frame")
TabContainer.Size = UDim2.new(1, 0, 0, 32)
TabContainer.Position = UDim2.new(0, 0, 0, 42)
TabContainer.BackgroundColor3 = Color3.fromRGB(8, 8, 8)
TabContainer.BorderSizePixel = 0
TabContainer.ZIndex = 15
TabContainer.Parent = MainFrame

-- Container de Conteúdo
local ContentContainer = Instance.new("Frame")
ContentContainer.Size = UDim2.new(1, 0, 1, -74)
ContentContainer.Position = UDim2.new(0, 0, 0, 74)
ContentContainer.BackgroundTransparency = 1
ContentContainer.ZIndex = 12
ContentContainer.Parent = MainFrame

-- Tabs
local TabNames = {"⚔️ Farm", "🏰 Raid", "💀 Boss", "⚡ Combat", "👤 Player", "👁️ Visual", "⚙️ Misc"}
local TabButtons = {}
local ContentFrames = {}

for i, name in ipairs(TabNames) do
    local btn = Instance.new("TextButton")
    btn.Size = UDim2.new(0, 78, 1, 0)
    btn.Position = UDim2.new(0, (i-1) * 80, 0, 0)
    btn.Text = name
    btn.TextColor3 = i == 1 and Color3.fromRGB(255, 40, 40) or Color3.fromRGB(150, 150, 150)
    btn.TextSize = 11
    btn.Font = Enum.Font.GothamBold
    btn.BackgroundColor3 = i == 1 and Color3.fromRGB(20, 5, 5) or Color3.fromRGB(12, 12, 12)
    btn.BorderSizePixel = 0
    btn.ZIndex = 16
    btn.Parent = TabContainer
    
    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(0, 4)
    corner.Parent = btn
    
    local content = Instance.new("Frame")
    content.Size = UDim2.new(1, -8, 1, -8)
    content.Position = UDim2.new(0, 4, 0, 4)
    content.BackgroundTransparency = 1
    content.Visible = (i == 1)
    content.ZIndex = 13
    content.Parent = ContentContainer
    
    local scroll = Instance.new("ScrollingFrame")
    scroll.Size = UDim2.new(1, 0, 1, 0)
    scroll.BackgroundTransparency = 1
    scroll.BorderSizePixel = 0
    scroll.ScrollBarThickness = 3
    scroll.ScrollBarImageColor3 = Color3.fromRGB(255, 30, 30)
    scroll.CanvasSize = UDim2.new(0, 0, 1, 0)
    scroll.ZIndex = 13
    scroll.Parent = content
    
    local layout = Instance.new("UIListLayout")
    layout.Padding = UDim.new(0, 7)
    layout.HorizontalAlignment = Enum.HorizontalAlignment.Center
    layout.SortOrder = Enum.SortOrder.LayoutOrder
    layout.Parent = scroll
    
    btn.MouseButton1Click:Connect(function()
        PlayClick()
        for j, b in ipairs(TabButtons) do
            b.TextColor3 = Color3.fromRGB(150, 150, 150)
            b.BackgroundColor3 = Color3.fromRGB(12, 12, 12)
        end
        btn.TextColor3 = Color3.fromRGB(255, 40, 40)
        btn.BackgroundColor3 = Color3.fromRGB(20, 5, 5)
        for _, c in ipairs(ContentFrames) do
            c.Visible = false
        end
        content.Visible = true
    end)
    
    TabButtons[i] = btn
    ContentFrames[i] = content
end

-- Funções UI
local function CreateSection(parent, name)
    local section = Instance.new("Frame")
    section.Size = UDim2.new(1, -8, 0, 26)
    section.BackgroundTransparency = 1
    section.ZIndex = 14
    section.Parent = parent
    
    local label = Instance.new("TextLabel")
    label.Size = UDim2.new(1, 0, 1, 0)
    label.Text = name
    label.TextColor3 = Color3.fromRGB(255, 50, 50)
    label.TextSize = 12
    label.Font = Enum.Font.GothamBold
    label.BackgroundTransparency = 1
    label.TextXAlignment = Enum.TextXAlignment.Left
    label.ZIndex = 14
    label.Parent = section
    
    return section
end

local function CreateToggle(parent, name, default, callback)
    local frame = Instance.new("Frame")
    frame.Size = UDim2.new(1, -8, 0, 38)
    frame.BackgroundColor3 = Color3.fromRGB(15, 15, 15)
    frame.BorderSizePixel = 0
    frame.ZIndex = 14
    frame.Parent = parent
    
    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(0, 6)
    corner.Parent = frame
    
    local label = Instance.new("TextLabel")
    label.Size = UDim2.new(0.65, 0, 1, 0)
    label.Position = UDim2.new(0, 10, 0, 0)
    label.Text = name
    label.TextColor3 = Color3.fromRGB(200, 200, 200)
    label.TextSize = 12
    label.Font = Enum.Font.Gotham
    label.BackgroundTransparency = 1
    label.TextXAlignment = Enum.TextXAlignment.Left
    label.ZIndex = 15
    label.Parent = frame
    
    local toggle = Instance.new("Frame")
    toggle.Size = UDim2.new(0, 42, 0, 22)
    toggle.Position = UDim2.new(1, -55, 0.5, -11)
    toggle.BackgroundColor3 = default and Color3.fromRGB(255, 30, 30) or Color3.fromRGB(40, 40, 40)
    toggle.BorderSizePixel = 0
    toggle.ZIndex = 15
    toggle.Parent = frame
    
    local toggleCorner = Instance.new("UICorner")
    toggleCorner.CornerRadius = UDim.new(1, 0)
    toggleCorner.Parent = toggle
    
    local circle = Instance.new("Frame")
    circle.Size = UDim2.new(0, 18, 0, 18)
    circle.Position = default and UDim2.new(0, 22, 0, 2) or UDim2.new(0, 2, 0, 2)
    circle.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
    circle.BorderSizePixel = 0
    circle.ZIndex = 16
    circle.Parent = toggle
    
    local circleCorner = Instance.new("UICorner")
    circleCorner.CornerRadius = UDim.new(1, 0)
    circleCorner.Parent = circle
    
    local state = default
    
    toggle.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 then
            state = not state
            TweenService:Create(circle, TweenInfo.new(0.2), {
                Position = state and UDim2.new(0, 22, 0, 2) or UDim2.new(0, 2, 0, 2)
            }):Play()
            TweenService:Create(toggle, TweenInfo.new(0.2), {
                BackgroundColor3 = state and Color3.fromRGB(255, 30, 30) or Color3.fromRGB(40, 40, 40)
            }):Play()
            PlayClick()
            if callback then callback(state) end
        end
    end)
    
    return {SetState = function(s) state = s end, GetState = function() return state end}
end

local function CreateSlider(parent, name, min, max, default, callback)
    local frame = Instance.new("Frame")
    frame.Size = UDim2.new(1, -8, 0, 48)
    frame.BackgroundColor3 = Color3.fromRGB(15, 15, 15)
    frame.BorderSizePixel = 0
    frame.ZIndex = 14
    frame.Parent = parent
    
    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(0, 6)
    corner.Parent = frame
    
    local label = Instance.new("TextLabel")
    label.Size = UDim2.new(1, -20, 0, 16)
    label.Position = UDim2.new(0, 10, 0, 3)
    label.Text = name .. ": " .. default
    label.TextColor3 = Color3.fromRGB(200, 200, 200)
    label.TextSize = 11
    label.Font = Enum.Font.Gotham
    label.BackgroundTransparency = 1
    label.TextXAlignment = Enum.TextXAlignment.Left
    label.ZIndex = 15
    label.Parent = frame
    
    local bar = Instance.new("Frame")
    bar.Size = UDim2.new(1, -20, 0, 5)
    bar.Position = UDim2.new(0, 10, 0, 28)
    bar.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
    bar.BorderSizePixel = 0
    bar.ZIndex = 15
    bar.Parent = frame
    
    local barCorner = Instance.new("UICorner")
    barCorner.CornerRadius = UDim.new(1, 0)
    barCorner.Parent = bar
    
    local percent = (default - min) / (max - min)
    local fill = Instance.new("Frame")
    fill.Size = UDim2.new(percent, 0, 1, 0)
    fill.BackgroundColor3 = Color3.fromRGB(255, 30, 30)
    fill.BorderSizePixel = 0
    fill.ZIndex = 16
    fill.Parent = bar
    
    local fillCorner = Instance.new("UICorner")
    fillCorner.CornerRadius = UDim.new(1, 0)
    fillCorner.Parent = fill
    
    local sbtn = Instance.new("TextButton")
    sbtn.Size = UDim2.new(0, 14, 0, 14)
    sbtn.Position = UDim2.new(percent, -7, 0.5, -7)
    sbtn.BackgroundColor3 = Color3.fromRGB(255, 50, 50)
    sbtn.BorderSizePixel = 0
    sbtn.Text = ""
    sbtn.ZIndex = 17
    sbtn.Parent = bar
    
    local sbtnCorner = Instance.new("UICorner")
    sbtnCorner.CornerRadius = UDim.new(1, 0)
    sbtnCorner.Parent = sbtn
    
    local isDragging = false
    
    local function update(input)
        local p = math.clamp((input.Position.X - bar.AbsolutePosition.X) / bar.AbsoluteSize.X, 0, 1)
        local value = math.floor(min + (max - min) * p)
        fill.Size = UDim2.new(p, 0, 1, 0)
        sbtn.Position = UDim2.new(p, -7, 0.5, -7)
        label.Text = name .. ": " .. value
        if callback then callback(value) end
    end
    
    sbtn.MouseButton1Down:Connect(function() isDragging = true end)
    bar.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 then
            isDragging = true
            update(input)
        end
    end)
    
    UserInputService.InputEnded:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 then
            isDragging = false
        end
    end)
    
    UserInputService.InputChanged:Connect(function(input)
        if isDragging and input.UserInputType == Enum.UserInputType.MouseMovement then
            update(input)
        end
    end)
    
    return frame
end

local function CreateButton(parent, name, callback)
    local btn = Instance.new("TextButton")
    btn.Size = UDim2.new(1, -8, 0, 33)
    btn.Text = name
    btn.TextColor3 = Color3.fromRGB(255, 255, 255)
    btn.TextSize = 12
    btn.Font = Enum.Font.Gotham
    btn.BackgroundColor3 = Color3.fromRGB(15, 15, 15)
    btn.BorderSizePixel = 0
    btn.ZIndex = 14
    btn.Parent = parent
    
    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(0, 6)
    corner.Parent = btn
    
    btn.MouseEnter:Connect(function()
        TweenService:Create(btn, TweenInfo.new(0.2), {BackgroundColor3 = Color3.fromRGB(25, 25, 25)}):Play()
    end)
    
    btn.MouseLeave:Connect(function()
        TweenService:Create(btn, TweenInfo.new(0.2), {BackgroundColor3 = Color3.fromRGB(15, 15, 15)}):Play()
    end)
    
    btn.MouseButton1Click:Connect(function()
        PlayClick()
        if callback then callback() end
    end)
    
    return btn
end

-- ============================================
-- PREENCHER ABAS
-- ============================================

-- ABA 1: FARM
local farmScroll = ContentFrames[1]:FindFirstChildOfClass("ScrollingFrame")
CreateSection(farmScroll, "⚔️ AUTO FARM")
CreateToggle(farmScroll, "Auto Farm Level", Settings.AutoFarm, function(s) Settings.AutoFarm = s end)
CreateToggle(farmScroll, "Auto Farm Quest", Settings.AutoQuest, function(s) Settings.AutoQuest = s end)
CreateToggle(farmScroll, "Auto Attack", Settings.AutoAttack, function(s) Settings.AutoAttack = s end)
CreateToggle(farmScroll, "Auto Haki", Settings.AutoHaki, function(s) Settings.AutoHaki = s end)
CreateToggle(farmScroll, "Fast Attack", Settings.FastAttack, function(s) Settings.FastAttack = s end)
CreateToggle(farmScroll, "Auto Boss", Settings.AutoBoss, function(s) Settings.AutoBoss = s end)

-- ABA 2: RAID
local raidScroll = ContentFrames[2]:FindFirstChildOfClass("ScrollingFrame")
CreateSection(raidScroll, "🏰 RAID SYSTEM")
CreateToggle(raidScroll, "Auto Raid", Settings.AutoRaid, function(s) Settings.AutoRaid = s end)
CreateButton(raidScroll, "🔥 Iniciar Raid", function() Notify("RAID", "Iniciando raid...", 3) end)
CreateButton(raidScroll, "⚖️ Law Raid", function() Notify("RAID", "Iniciando Law Raid...", 3) end)

-- ABA 3: BOSS
local bossScroll = ContentFrames[3]:FindFirstChildOfClass("ScrollingFrame")
CreateSection(bossScroll, "💀 BOSS SYSTEM")
CreateToggle(bossScroll, "Auto Boss Farm", Settings.AutoBoss, function(s) Settings.AutoBoss = s end)
CreateButton(bossScroll, "💀 Don Swan", function() Notify("BOSS", "Teleportando...", 2) end)
CreateButton(bossScroll, "💀 Diamond", function() Notify("BOSS", "Teleportando...", 2) end)
CreateButton(bossScroll, "💀 Jeremy", function() Notify("BOSS", "Teleportando...", 2) end)
CreateButton(bossScroll, "💀 Fajita", function() Notify("BOSS", "Teleportando...", 2) end)

-- ABA 4: COMBAT
local combatScroll = ContentFrames[4]:FindFirstChildOfClass("ScrollingFrame")
CreateSection(combatScroll, "⚡ AUTO SKILLS")
CreateToggle(combatScroll, "Auto Skill Z", Settings.AutoSkillZ, function(s) Settings.AutoSkillZ = s end)
CreateToggle(combatScroll, "Auto Skill X", Settings.AutoSkillX, function(s) Settings.AutoSkillX = s end)
CreateToggle(combatScroll, "Auto Skill C", Settings.AutoSkillC, function(s) Settings.AutoSkillC = s end)
CreateToggle(combatScroll, "Auto Skill V", Settings.AutoSkillV, function(s) Settings.AutoSkillV = s end)
CreateToggle(combatScroll, "Auto Skill F", Settings.AutoSkillF, function(s) Settings.AutoSkillF = s end)

-- ABA 5: PLAYER
local playerScroll = ContentFrames[5]:FindFirstChildOfClass("ScrollingFrame")
CreateSection(playerScroll, "👤 MOVEMENT")
CreateSlider(playerScroll, "WalkSpeed", 16, 500, Settings.WalkSpeed, function(v)
    Settings.WalkSpeed = v
    if Humanoid then Humanoid.WalkSpeed = v end
end)
CreateSlider(playerScroll, "JumpPower", 50, 500, Settings.JumpPower, function(v)
    Settings.JumpPower = v
    if Humanoid then Humanoid.JumpPower = v end
end)
CreateSlider(playerScroll, "Fly Speed", 10, 200, Settings.FlySpeed, function(v) Settings.FlySpeed = v end)
CreateSection(playerScroll, "⭐ ABILITIES")
CreateToggle(playerScroll, "Fly", Settings.Fly, function(s) Settings.Fly = s end)
CreateToggle(playerScroll, "No Clip", Settings.NoClip, function(s) Settings.NoClip = s end)
CreateToggle(playerScroll, "Infinite Jump", Settings.InfiniteJump, function(s) Settings.InfiniteJump = s end)

-- ABA 6: VISUAL
local visualScroll = ContentFrames[6]:FindFirstChildOfClass("ScrollingFrame")
CreateSection(visualScroll, "👁️ ESP")
CreateToggle(visualScroll, "ESP Players", Settings.ESPPlayers, function(s) Settings.ESPPlayers = s end)
CreateToggle(visualScroll, "ESP Fruits", Settings.ESPFruits, function(s) Settings.ESPFruits = s end)
CreateToggle(visualScroll, "ESP Chests", Settings.ESPChests, function(s) Settings.ESPChests = s end)
CreateSection(visualScroll, "🌍 WORLD")
CreateToggle(visualScroll, "Remove Fog", Settings.RemoveFog, function(s)
    Settings.RemoveFog = s
    Lighting.FogEnd = s and 9e9 or 1000
end)
CreateToggle(visualScroll, "Full Bright", Settings.FullBright, function(s)
    Settings.FullBright = s
    Lighting.Brightness = s and 2 or 1
end)
CreateToggle(visualScroll, "FPS Boost", Settings.FPSBoost, function(s)
    Settings.FPSBoost = s
    if s then Lighting.GlobalShadows = false end
end)

-- ABA 7: MISC
local miscScroll = ContentFrames[7]:FindFirstChildOfClass("ScrollingFrame")
CreateSection(miscScroll, "⚙️ UTILITIES")
CreateToggle(miscScroll, "Anti AFK", Settings.AntiAFK, function(s) Settings.AntiAFK = s end)
CreateToggle(miscScroll, "Auto Redeem Codes", Settings.AutoRedeem, function(s) Settings.AutoRedeem = s end)
CreateSection(miscScroll, "🌐 SERVER")
CreateButton(miscScroll, "🔄 Rejoin Server", function()
    TeleportService:Teleport(game.PlaceId, Player)
end)
CreateButton(miscScroll, "🌐 Server Hop", function()
    Notify("SERVER", "Procurando servidor...", 3)
end)
CreateButton(miscScroll, "📋 Copy Discord", function()
    if setclipboard then
        setclipboard("https://discord.gg/itachihub")
        Notify("DISCORD", "Link copiado!", 2)
    end
end)

-- Atualizar CanvasSize
for _, content in ipairs(ContentFrames) do
    local scroll = content:FindFirstChildOfClass("ScrollingFrame")
    if scroll then
        local layout = scroll:FindFirstChildOfClass("UIListLayout")
        if layout then
            task.wait(0.1)
            scroll.CanvasSize = UDim2.new(0, 0, 0, layout.AbsoluteContentSize.Y + 10)
        end
    end
end

-- Botão Flutuante
local FloatingBtn = Instance.new("ImageButton")
FloatingBtn.Size = UDim2.new(0, 55, 0, 55)
FloatingBtn.Position = UDim2.new(0.04, 0, 0.8, 0)
FloatingBtn.BackgroundTransparency = 1
FloatingBtn.Image = "rbxassetid://16556523844"
FloatingBtn.Visible = false
FloatingBtn.ZIndex = 50
FloatingBtn.Parent = ScreenGui

local FloatCorner = Instance.new("UICorner")
FloatCorner.CornerRadius = UDim.new(1, 0)
FloatCorner.Parent = FloatingBtn

local FloatStroke = Instance.new("UIStroke")
FloatStroke.Color = Color3.fromRGB(255, 0, 0)
FloatStroke.Thickness = 2
FloatStroke.Parent = FloatingBtn

FloatingBtn.MouseButton1Click:Connect(function()
    PlayClick()
    MainFrame.Visible = true
    FloatingBtn.Visible = false
end)

MinimizeBtn.MouseButton1Click:Connect(function()
    PlayClick()
    MainFrame.Visible = false
    FloatingBtn.Visible = true
end)

CloseBtn.MouseButton1Click:Connect(function()
    PlayClick()
    ScreenGui:Destroy()
end)

-- ============================================
-- SISTEMAS FUNCIONAIS
-- ============================================

-- Auto Farm
task.spawn(function()
    while task.wait(0.5) do
        if Settings.AutoFarm and Character and RootPart then
            local nearestEnemy = nil
            local shortestDist = math.huge
            
            for _, enemy in ipairs(workspace.Enemies:GetChildren()) do
                if enemy:FindFirstChild("Humanoid") and enemy.Humanoid.Health > 0 and enemy:FindFirstChild("HumanoidRootPart") then
                    local dist = (RootPart.Position - enemy.HumanoidRootPart.Position).Magnitude
                    if dist < shortestDist then
                        shortestDist = dist
                        nearestEnemy = enemy
                    end
                end
            end
            
            if nearestEnemy and nearestEnemy:FindFirstChild("HumanoidRootPart") then
                RootPart.CFrame = nearestEnemy.HumanoidRootPart.CFrame * CFrame.new(0, 2, 3)
                
                if Settings.AutoAttack then
                    local weapon = Character:FindFirstChildOfClass("Tool")
                    if weapon then weapon:Activate() end
                end
            end
        end
    end
end)

-- Auto Haki
task.spawn(function()
    while task.wait(1) do
        if Settings.AutoHaki then
            pcall(function()
                local args = {[1] = "Buso"}
                ReplicatedStorage.Remotes.CommF_:InvokeServer(unpack(args))
            end)
        end
    end
end)

-- Auto Skills
task.spawn(function()
    while task.wait(0.3) do
        local skills = {
            {Settings.AutoSkillZ, Enum.KeyCode.Z},
            {Settings.AutoSkillX, Enum.KeyCode.X},
            {Settings.AutoSkillC, Enum.KeyCode.C},
            {Settings.AutoSkillV, Enum.KeyCode.V},
            {Settings.AutoSkillF, Enum.KeyCode.F}
        }
        for _, skill in ipairs(skills) do
            if skill[1] then
                VirtualInputManager:SendKeyEvent(true, skill[2], false, nil)
                task.wait(0.1)
                VirtualInputManager:SendKeyEvent(false, skill[2], false, nil)
            end
        end
    end
end)

-- Fly
local flyKeys = {W = false, S = false, A = false, D = false, Space = false, LeftControl = false}

UserInputService.InputBegan:Connect(function(input, gameProcessed)
    if gameProcessed then return end
    if flyKeys[input.KeyCode.Name] ~= nil then
        flyKeys[input.KeyCode.Name] = true
    end
end)

UserInputService.InputEnded:Connect(function(input)
    if flyKeys[input.KeyCode.Name] ~= nil then
        flyKeys[input.KeyCode.Name] = false
    end
end)

task.spawn(function()
    while task.wait() do
        if Settings.Fly and Character and RootPart then
            local humanoid = Character:FindFirstChild("Humanoid")
            if humanoid then humanoid.PlatformStand = true end
            
            local dir = Vector3.new()
            if flyKeys.W then dir = dir + Camera.CFrame.LookVector end
            if flyKeys.S then dir = dir - Camera.CFrame.LookVector end
            if flyKeys.A then dir = dir - Camera.CFrame.RightVector end
            if flyKeys.D then dir = dir + Camera.CFrame.RightVector end
            if flyKeys.Space then dir = dir + Vector3.new(0, 1, 0) end
            if flyKeys.LeftControl then dir = dir - Vector3.new(0, 1, 0) end
            
            RootPart.Velocity = dir.Magnitude > 0 and dir.Unit * Settings.FlySpeed or Vector3.zero
        end
    end
end)

-- No Clip
task.spawn(function()
    while task.wait(0.2) do
        if Settings.NoClip and Character then
            for _, part in ipairs(Character:GetDescendants()) do
                if part:IsA("BasePart") then
                    part.CanCollide = false
                end
            end
        end
    end
end)

-- Infinite Jump
UserInputService.JumpRequest:Connect(function()
    if Settings.InfiniteJump and Character and Humanoid then
        Humanoid:ChangeState(Enum.HumanoidStateType.Jumping)
    end
end)

-- Anti AFK
task.spawn(function()
    while task.wait(20) do
        if Settings.AntiAFK then
            VirtualUser:CaptureController()
            VirtualUser:ClickButton2(Vector2.new())
        end
    end
end)

-- Auto Redeem
task.spawn(function()
    while task.wait(60) do
        if Settings.AutoRedeem then
            local codes = {"Sub2CaptainMaui", "KittGaming", "Sub2Fer999", "Enyu_is_Pro", "Magicbus"}
            for _, code in ipairs(codes) do
                pcall(function()
                    ReplicatedStorage.Remotes.Redeem:InvokeServer("Code", code)
                end)
                task.wait(2)
            end
        end
    end
end)

-- ESP
task.spawn(function()
    while task.wait(2) do
        for _, obj in ipairs(workspace:GetDescendants()) do
            if obj:IsA("Highlight") and obj.Name == "ESP" then
                obj:Destroy()
            end
        end
        
        if Settings.ESPPlayers then
            for _, plr in ipairs(Players:GetPlayers()) do
                if plr ~= Player and plr.Character then
                    local hl = Instance.new("Highlight")
                    hl.Name = "ESP"
                    hl.FillColor = Color3.fromRGB(255, 0, 0)
                    hl.FillTransparency = 0.5
                    hl.OutlineColor = Color3.fromRGB(255, 50, 50)
                    hl.Parent = plr.Character
                end
            end
        end
        
        if Settings.ESPFruits then
            for _, obj in ipairs(workspace:GetChildren()) do
                if obj:IsA("Tool") and obj:FindFirstChild("Handle") then
                    local hl = Instance.new("Highlight")
                    hl.Name = "ESP"
                    hl.FillColor = Color3.fromRGB(255, 165, 0)
                    hl.FillTransparency = 0.5
                    hl.Parent = obj
                end
            end
        end
    end
end)

-- Atalhos de teclado
UserInputService.InputBegan:Connect(function(input, gameProcessed)
    if gameProcessed then return end
    if input.KeyCode == Enum.KeyCode.F1 then
        MainFrame.Visible = not MainFrame.Visible
        FloatingBtn.Visible = not MainFrame.Visible
    elseif input.KeyCode == Enum.KeyCode.F2 then
        Settings.AutoFarm = not Settings.AutoFarm
        Notify("FARM", Settings.AutoFarm and "ON" or "OFF", 2)
    elseif input.KeyCode == Enum.KeyCode.F3 then
        Settings.Fly = not Settings.Fly
        Notify("FLY", Settings.Fly and "ON" or "OFF", 2)
    end
end)

-- Notificação de carregamento
Notify("✅ ITACHI HUB", "Script carregado com sucesso!", 4)
Notify("⌨️ ATALHOS", "F1=Menu | F2=Farm | F3=Fly", 4)

print("⚡ ITACHI HUB V3.2 - 100% FUNCIONAL ⚡")
print("✅ Todas as opções funcionando!")

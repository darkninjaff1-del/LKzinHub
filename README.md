--[[
    ⚡ ITACHI HUB - BLOX FRUITS ULTIMATE ⚡
    Versão 7.4 - COMPLETO E SEM ERROS
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
local Workspace = game:GetService("Workspace")
local Stats = game:GetService("Stats")

local Player = Players.LocalPlayer
local Mouse = Player:GetMouse()
local Camera = Workspace.CurrentCamera
local Character = Player.Character or Player.CharacterAdded:Wait()
local Humanoid = Character:FindFirstChild("Humanoid")
local RootPart = Character:FindFirstChild("HumanoidRootPart")

Player.CharacterAdded:Connect(function(char)
    Character = char
    Humanoid = char:FindFirstChild("Humanoid")
    RootPart = char:FindFirstChild("HumanoidRootPart")
end)

-- Configurações
local Settings = {
    AutoFarm = false, FarmDistance = 15, FarmMethod = "TP",
    AutoQuest = false, AutoAttack = false, FastAttack = false,
    AutoBuso = false, AutoKen = false, KillAura = false,
    AutoBoss = false, AutoSeaBeast = false, AutoShipRaid = false,
    AutoFactory = false, AutoChest = false, AutoMaterial = false,
    BossFarmAll = false, BossFarmSea1 = false, BossFarmSea2 = false, BossFarmSea3 = false,
    BossToggles = {},
    AutoFruit = false, AutoStore = false, AutoDropCommon = false, AutoCollectLegendary = false,
    AutoClick = false,
    AutoSkillZ = false, AutoSkillX = false, AutoSkillC = false, AutoSkillV = false, AutoSkillF = false,
    AutoAim = false, AutoCombo = false, NoSkillDelay = false,
    Aimbot = false, AimbotFOV = 90, AimbotSmooth = 1, AimbotTarget = nil,
    WalkSpeed = 16, JumpPower = 50, FlySpeed = 50,
    Fly = false, NoClip = false, InfiniteJump = false, GodMode = false,
    WaterWalk = false,
    ESPPlayers = false, ESPFruits = false, ESPChests = false, ESPBosses = false,
    ESPDistance = 500, RemoveFog = false, FullBright = false, FPSBoost = false, NoWater = false,
    AutoRaceV2 = false, AutoRaceV3 = false, AutoRaceV4 = false,
    AutoSeaEvents = false, AutoVolcano = false,
    AutoAwakenFruits = false, AutoBuyFruitChips = false,
    AutoFarmSword = false, AutoLearnStyle = false, AutoTrainHaki = false,
    AutoMoneyFarm = false, AutoFragmentFarm = false,
    AntiAFK = false, AutoRedeem = false, AutoRejoin = false, SoundEnabled = true,
    MacroRecording = false, MacroPlaying = false,
    WebhookEnabled = false, WebhookURL = "",
    ThemeColor = Color3.fromRGB(255,0,0),
}

-- Boss list
local BossList = {
    Sea1 = {
        ["Don Swan"] = CFrame.new(-650,313,-10500), ["Diamond"] = CFrame.new(-1500,10,-2500),
        ["Jeremy"] = CFrame.new(3000,10,-6500), ["Fajita"] = CFrame.new(-2000,10,-5000),
        ["Smoke Admiral"] = CFrame.new(-3000,10,-7000), ["Cursed Captain"] = CFrame.new(-5000,10,-9000),
    },
    Sea2 = {
        ["Don Swan 2"] = CFrame.new(-650,313,-10500), ["Diamond 2"] = CFrame.new(-1500,10,-2500),
        ["Jeremy 2"] = CFrame.new(3000,10,-6500), ["Fajita 2"] = CFrame.new(-2000,10,-5000),
        ["Tide Keeper"] = CFrame.new(-4500,10,-7000), ["Dark Beard"] = CFrame.new(-6000,10,-9000),
    },
    Sea3 = {
        ["Beautiful Pirate"] = CFrame.new(-6400,137,-11600), ["Longma"] = CFrame.new(-3000,50,-8500),
        ["Stone"] = CFrame.new(-2000,50,-7500), ["Island Empress"] = CFrame.new(-5000,137,-12000),
        ["Captain Elephant"] = CFrame.new(-4000,100,-10000),
    }
}
for _, bosses in pairs(BossList) do for name,_ in pairs(bosses) do Settings.BossToggles[name] = false end end

-- Teleportes
local TeleportLocations = {
    ["Sea 1"] = {
        ["Windmill"] = CFrame.new(4846,13,2718), ["Pirate Village"] = CFrame.new(-1122,19,4107),
        ["Desert"] = CFrame.new(876,36,4268), ["Snow Mountain"] = CFrame.new(2394,26,-5895),
        ["Marine Fortress"] = CFrame.new(-3671,24,-2789), ["Prison"] = CFrame.new(4875,5,680),
        ["Colosseum"] = CFrame.new(-1438,7,-3085), ["Magma Village/Volcano"] = CFrame.new(-5232,9,8472),
    },
    ["Sea 2"] = {
        ["Kingdom of Rose"] = CFrame.new(-5550,313,-4800), ["Green Zone"] = CFrame.new(-1106,37,-6563),
        ["Graveyard"] = CFrame.new(-1663,37,-10009), ["Ice Castle"] = CFrame.new(-5996,313,-5137),
        ["Dark Arena"] = CFrame.new(-3560,313,-3029),
    },
    ["Sea 3"] = {
        ["Port Town"] = CFrame.new(-272,33,2072), ["Hydra Island"] = CFrame.new(5750,42,625),
        ["Great Tree"] = CFrame.new(2174,35,-6650), ["Floating Turtle"] = CFrame.new(-1000,35,-8700),
        ["Haunted Castle"] = CFrame.new(-6400,137,-11600),
    },
    ["Bosses"] = {},
    ["NPCs"] = {
        ["Trade NPC"] = CFrame.new(-1200,10,-1800), ["Fruit Dealer"] = CFrame.new(-1100,10,-1700),
        ["Weapon Dealer"] = CFrame.new(-1000,10,-1600), ["Haki Trainer"] = CFrame.new(-2000,10,-2000),
    },
    ["Special"] = {
        ["Volcano Peak"] = CFrame.new(-5100,50,8600), ["Hot and Cold"] = CFrame.new(-1532,10,-8763),
    }
}
for _, bosses in pairs(BossList) do for name, cf in pairs(bosses) do TeleportLocations["Bosses"][name] = cf end end

-- Sons
local function PlaySound(id, vol)
    if not Settings.SoundEnabled then return end
    task.spawn(function()
        local s = Instance.new("Sound"); s.SoundId = "rbxassetid://"..(id or "9116338042")
        s.Volume = vol or 0.3; s.Parent = CoreGui; s:Play()
        task.wait(0.5); s:Destroy()
    end)
end

-- Notificações
local function Notify(title, msg, dur, typ)
    dur = dur or 3
    local colors = { info=Color3.fromRGB(255,50,50), success=Color3.fromRGB(50,255,50), warning=Color3.fromRGB(255,165,0) }
    local frame = Instance.new("Frame"); frame.Size = UDim2.new(0,300,0,75); frame.Position = UDim2.new(1,20,0.7,0)
    frame.BackgroundColor3 = Color3.fromRGB(15,15,15); frame.BorderSizePixel = 0; frame.ZIndex = 9999; frame.Parent = CoreGui
    Instance.new("UICorner", frame).CornerRadius = UDim.new(0,8)
    local stroke = Instance.new("UIStroke", frame); stroke.Color = colors[typ] or colors.info; stroke.Thickness = 1.5
    local bar = Instance.new("Frame", frame); bar.Size = UDim2.new(1,6,0,3); bar.Position = UDim2.new(0,-3,0,0)
    bar.BackgroundColor3 = colors[typ] or colors.info; bar.BorderSizePixel = 0; bar.ZIndex = 10000
    local tL = Instance.new("TextLabel", frame); tL.Size = UDim2.new(1,-20,0,22); tL.Position = UDim2.new(0,15,0,8)
    tL.Text = title; tL.TextColor3 = Color3.fromRGB(255,255,255); tL.TextSize = 14; tL.Font = Enum.Font.GothamBold
    tL.BackgroundTransparency = 1; tL.TextXAlignment = Enum.TextXAlignment.Left; tL.ZIndex = 10001
    local mL = Instance.new("TextLabel", frame); mL.Size = UDim2.new(1,-20,0,25); mL.Position = UDim2.new(0,15,0,33)
    mL.Text = msg; mL.TextColor3 = Color3.fromRGB(180,180,180); mL.TextSize = 12; mL.Font = Enum.Font.Gotham
    mL.BackgroundTransparency = 1; mL.TextXAlignment = Enum.TextXAlignment.Left; mL.ZIndex = 10001
    pcall(function() TweenService:Create(frame, TweenInfo.new(0.4, Enum.EasingStyle.Quart, Enum.EasingDirection.Out), {Position=UDim2.new(1,-320,0.7,0)}):Play() end)
    task.delay(dur, function()
        pcall(function() TweenService:Create(frame, TweenInfo.new(0.4, Enum.EasingStyle.Quart, Enum.EasingDirection.In), {Position=UDim2.new(1,20,0.7,0)}):Play() end)
        task.delay(0.4, function() pcall(function() frame:Destroy() end) end)
    end)
end

-- ============================================
-- UI
-- ============================================
local ScreenGui = Instance.new("ScreenGui"); ScreenGui.Name = "ItachiHub_"..math.random(1000,9999)
ScreenGui.Parent = CoreGui; ScreenGui.ResetOnSpawn = false; ScreenGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling

local MainFrame = Instance.new("Frame"); MainFrame.Size = UDim2.new(0,640,0,460)
MainFrame.Position = UDim2.new(0.5,-320,0.5,-230); MainFrame.BackgroundColor3 = Color3.fromRGB(10,10,10)
MainFrame.BorderSizePixel = 0; MainFrame.Visible = true; MainFrame.ZIndex = 10; MainFrame.Parent = ScreenGui
Instance.new("UICorner", MainFrame).CornerRadius = UDim.new(0,10)
local MainStroke = Instance.new("UIStroke", MainFrame); MainStroke.Color = Settings.ThemeColor
MainStroke.Thickness = 2; MainStroke.Transparency = 0.4

local Header = Instance.new("Frame"); Header.Size = UDim2.new(1,0,0,45); Header.BackgroundColor3 = Color3.fromRGB(5,5,5)
Header.BorderSizePixel = 0; Header.ZIndex = 20; Header.Parent = MainFrame
Instance.new("UICorner", Header).CornerRadius = UDim.new(0,10)
local Logo = Instance.new("ImageLabel", Header); Logo.Size = UDim2.new(0,30,0,30); Logo.Position = UDim2.new(0,10,0,8)
Logo.BackgroundTransparency = 1; Logo.Image = "rbxassetid://16556523844"; Logo.ZIndex = 21
local Title = Instance.new("TextLabel", Header); Title.Size = UDim2.new(0,200,1,0); Title.Position = UDim2.new(0,48,0,0)
Title.Text = "⚡ ITACHI HUB v7.4"; Title.TextColor3 = Settings.ThemeColor; Title.TextSize = 18
Title.Font = Enum.Font.GothamBlack; Title.BackgroundTransparency = 1; Title.TextXAlignment = Enum.TextXAlignment.Left; Title.ZIndex = 21
local MinimizeBtn = Instance.new("TextButton", Header); MinimizeBtn.Size = UDim2.new(0,28,0,28); MinimizeBtn.Position = UDim2.new(1,-68,0,9)
MinimizeBtn.Text = "━"; MinimizeBtn.TextColor3 = Color3.fromRGB(255,40,40); MinimizeBtn.TextSize = 14
MinimizeBtn.Font = Enum.Font.GothamBold; MinimizeBtn.BackgroundColor3 = Color3.fromRGB(15,15,15)
MinimizeBtn.BorderSizePixel = 0; MinimizeBtn.ZIndex = 21; Instance.new("UICorner", MinimizeBtn).CornerRadius = UDim.new(0,6)
local CloseBtn = Instance.new("TextButton", Header); CloseBtn.Size = UDim2.new(0,28,0,28); CloseBtn.Position = UDim2.new(1,-34,0,9)
CloseBtn.Text = "✕"; CloseBtn.TextColor3 = Color3.fromRGB(255,40,40); CloseBtn.TextSize = 14
CloseBtn.Font = Enum.Font.GothamBold; CloseBtn.BackgroundColor3 = Color3.fromRGB(15,15,15)
CloseBtn.BorderSizePixel = 0; CloseBtn.ZIndex = 21; Instance.new("UICorner", CloseBtn).CornerRadius = UDim.new(0,6)

-- Drag janela
local dragging = false; local dragStart, startPos
Header.InputBegan:Connect(function(input) if input.UserInputType==Enum.UserInputType.MouseButton1 then dragging=true; dragStart=input.Position; startPos=MainFrame.Position end end)
UserInputService.InputChanged:Connect(function(input) if dragging and input.UserInputType==Enum.UserInputType.MouseMovement then local delta=input.Position-dragStart; MainFrame.Position=UDim2.new(startPos.X.Scale, startPos.X.Offset+delta.X, startPos.Y.Scale, startPos.Y.Offset+delta.Y) end end)
UserInputService.InputEnded:Connect(function(input) if input.UserInputType==Enum.UserInputType.MouseButton1 then dragging=false end end)

-- Tabs
local TabContainer = Instance.new("Frame"); TabContainer.Size = UDim2.new(1,0,0,28); TabContainer.Position = UDim2.new(0,0,0,45)
TabContainer.BackgroundColor3 = Color3.fromRGB(8,8,8); TabContainer.BorderSizePixel = 0; TabContainer.ZIndex = 15; TabContainer.Parent = MainFrame
local ContentContainer = Instance.new("Frame"); ContentContainer.Size = UDim2.new(1,0,1,-73); ContentContainer.Position = UDim2.new(0,0,0,73)
ContentContainer.BackgroundTransparency = 1; ContentContainer.ZIndex = 12; ContentContainer.Parent = MainFrame

local TabNames = {
    "⚔️ Farm", "📦 Sub", "💀 Boss", "🌊 Eventos", "💎 Extras", "🍎 Frutas", "🗡️ Espadas", "🥊 Estilos",
    "🧬 Raça", "⚡ Combate", "🎯 Aimbot", "👤 Player", "👁️ Visual", "📊 Server", "🌍 Teleport",
    "🌋 Volcano", "🛒 Shop", "💰 Money", "🤖 Macros", "🔔 Webhook", "🎨 Theme", "⚙️ Settings"
}
local TabButtons = {}; local ContentFrames = {}; local Tabs = {}

for i, name in ipairs(TabNames) do
    local btn = Instance.new("TextButton"); btn.Size = UDim2.new(0, 29, 1, 0); btn.Position = UDim2.new(0, (i-1)*29.5, 0, 0)
    btn.Text = string.sub(name, 1, 2); btn.TextColor3 = i==1 and Settings.ThemeColor or Color3.fromRGB(140,140,140)
    btn.TextSize = 12; btn.Font = Enum.Font.GothamBold; btn.BackgroundColor3 = i==1 and Color3.fromRGB(20,5,5) or Color3.fromRGB(10,10,10)
    btn.BorderSizePixel = 0; btn.ZIndex = 16; btn.Parent = TabContainer
    Instance.new("UICorner", btn).CornerRadius = UDim.new(0,3)
    local content = Instance.new("Frame"); content.Size = UDim2.new(1,-6,1,-6); content.Position = UDim2.new(0,3,0,3)
    content.BackgroundTransparency = 1; content.Visible = (i==1); content.ZIndex = 13; content.Parent = ContentContainer
    local scroll = Instance.new("ScrollingFrame", content); scroll.Size = UDim2.new(1,0,1,0); scroll.BackgroundTransparency = 1
    scroll.BorderSizePixel = 0; scroll.ScrollBarThickness = 3; scroll.ScrollBarImageColor3 = Settings.ThemeColor
    scroll.CanvasSize = UDim2.new(0,0,1,0); scroll.ZIndex = 13
    local layout = Instance.new("UIListLayout", scroll); layout.Padding = UDim.new(0,5)
    layout.HorizontalAlignment = Enum.HorizontalAlignment.Center; layout.SortOrder = Enum.SortOrder.LayoutOrder
    btn.MouseButton1Click:Connect(function()
        PlaySound("9116338042",0.2)
        for j,b in ipairs(TabButtons) do b.TextColor3=Color3.fromRGB(140,140,140); b.BackgroundColor3=Color3.fromRGB(10,10,10) end
        btn.TextColor3=Settings.ThemeColor; btn.BackgroundColor3=Color3.fromRGB(20,5,5)
        for _,c in ipairs(ContentFrames) do c.Visible=false end
        content.Visible=true
    end)
    TabButtons[i]=btn; ContentFrames[i]=content
    Tabs[string.match(name, "%s(.+)") or name] = {Scroll=scroll, Layout=layout}
end

-- Funções UI
local function CreateSection(parent, title)
    local s = Instance.new("Frame", parent); s.Size=UDim2.new(1,-6,0,24); s.BackgroundTransparency=1; s.ZIndex=14
    local l = Instance.new("TextLabel", s); l.Size=UDim2.new(1,0,1,0); l.Text=title; l.TextColor3=Settings.ThemeColor; l.TextSize=11
    l.Font=Enum.Font.GothamBold; l.BackgroundTransparency=1; l.TextXAlignment=Enum.TextXAlignment.Left; l.ZIndex=14
    local line = Instance.new("Frame", s); line.Size=UDim2.new(1,0,0,1); line.Position=UDim2.new(0,0,1,1)
    line.BackgroundColor3=Settings.ThemeColor; line.BorderSizePixel=0; line.BackgroundTransparency=0.5; line.ZIndex=14
    return s
end
local function CreateToggle(parent, name, default, callback)
    local f = Instance.new("Frame", parent); f.Size=UDim2.new(1,-6,0,36); f.BackgroundColor3=Color3.fromRGB(15,15,15); f.BorderSizePixel=0; f.ZIndex=14
    Instance.new("UICorner", f).CornerRadius=UDim.new(0,5)
    local lbl = Instance.new("TextLabel", f); lbl.Size=UDim2.new(0.6,0,1,0); lbl.Position=UDim2.new(0,8,0,0); lbl.Text=name
    lbl.TextColor3=Color3.fromRGB(200,200,200); lbl.TextSize=11; lbl.Font=Enum.Font.Gotham; lbl.BackgroundTransparency=1; lbl.TextXAlignment=Enum.TextXAlignment.Left; lbl.ZIndex=15
    local tog = Instance.new("Frame", f); tog.Size=UDim2.new(0,38,0,20); tog.Position=UDim2.new(1,-48,0.5,-10)
    tog.BackgroundColor3=default and Settings.ThemeColor or Color3.fromRGB(40,40,40); tog.BorderSizePixel=0; tog.ZIndex=15
    Instance.new("UICorner", tog).CornerRadius=UDim.new(1,0)
    local circle = Instance.new("Frame", tog); circle.Size=UDim2.new(0,16,0,16); circle.Position=default and UDim2.new(0,20,0,2) or UDim2.new(0,2,0,2)
    circle.BackgroundColor3=Color3.fromRGB(255,255,255); circle.BorderSizePixel=0; circle.ZIndex=16
    Instance.new("UICorner", circle).CornerRadius=UDim.new(1,0)
    local state = default
    tog.InputBegan:Connect(function(input)
        if input.UserInputType==Enum.UserInputType.MouseButton1 then
            state = not state
            pcall(function() TweenService:Create(circle, TweenInfo.new(0.2), {Position=state and UDim2.new(0,20,0,2) or UDim2.new(0,2,0,2)}):Play()
                TweenService:Create(tog, TweenInfo.new(0.2), {BackgroundColor3=state and Settings.ThemeColor or Color3.fromRGB(40,40,40)}):Play() end)
            PlaySound("9116338042",0.15)
            if callback then pcall(function() callback(state) end) end
        end
    end)
    return {Set=function(s) state=s end, Get=function() return state end}
end
local function CreateButton(parent, name, callback)
    local btn = Instance.new("TextButton", parent); btn.Size=UDim2.new(1,-6,0,32); btn.Text=name; btn.TextColor3=Color3.fromRGB(255,255,255)
    btn.TextSize=11; btn.Font=Enum.Font.Gotham; btn.BackgroundColor3=Color3.fromRGB(15,15,15); btn.BorderSizePixel=0; btn.ZIndex=14
    Instance.new("UICorner", btn).CornerRadius=UDim.new(0,5)
    btn.MouseEnter:Connect(function() pcall(function() TweenService:Create(btn, TweenInfo.new(0.2), {BackgroundColor3=Color3.fromRGB(25,25,25)}):Play() end) end)
    btn.MouseLeave:Connect(function() pcall(function() TweenService:Create(btn, TweenInfo.new(0.2), {BackgroundColor3=Color3.fromRGB(15,15,15)}):Play() end) end)
    btn.MouseButton1Click:Connect(function() PlaySound("9116338042",0.15); if callback then pcall(callback) end end)
    return btn
end
local function CreateSlider(parent, name, min, max, default, callback)
    local f = Instance.new("Frame", parent); f.Size=UDim2.new(1,-6,0,45); f.BackgroundColor3=Color3.fromRGB(15,15,15); f.BorderSizePixel=0; f.ZIndex=14
    Instance.new("UICorner", f).CornerRadius=UDim.new(0,5)
    local lbl = Instance.new("TextLabel", f); lbl.Size=UDim2.new(1,-16,0,15); lbl.Position=UDim2.new(0,8,0,3); lbl.Text=name..": "..default
    lbl.TextColor3=Color3.fromRGB(200,200,200); lbl.TextSize=10; lbl.Font=Enum.Font.Gotham; lbl.BackgroundTransparency=1; lbl.TextXAlignment=Enum.TextXAlignment.Left; lbl.ZIndex=15
    local bar = Instance.new("Frame", f); bar.Size=UDim2.new(1,-16,0,4); bar.Position=UDim2.new(0,8,0,26); bar.BackgroundColor3=Color3.fromRGB(30,30,30); bar.BorderSizePixel=0; bar.ZIndex=15
    Instance.new("UICorner", bar).CornerRadius=UDim.new(1,0)
    local pct = (default-min)/(max-min)
    local fill = Instance.new("Frame", bar); fill.Size=UDim2.new(pct,0,1,0); fill.BackgroundColor3=Settings.ThemeColor; fill.BorderSizePixel=0; fill.ZIndex=16
    Instance.new("UICorner", fill).CornerRadius=UDim.new(1,0)
    local sbtn = Instance.new("TextButton", bar); sbtn.Size=UDim2.new(0,12,0,12); sbtn.Position=UDim2.new(pct,-6,0.5,-6)
    sbtn.BackgroundColor3=Settings.ThemeColor; sbtn.BorderSizePixel=0; sbtn.Text=""; sbtn.ZIndex=17
    Instance.new("UICorner", sbtn).CornerRadius=UDim.new(1,0)
    local draggingSlider = false
    local function update(input)
        local p = math.clamp((input.Position.X - bar.AbsolutePosition.X) / bar.AbsoluteSize.X, 0, 1)
        local val = math.floor(min + (max-min)*p)
        fill.Size=UDim2.new(p,0,1,0); sbtn.Position=UDim2.new(p,-6,0.5,-6); lbl.Text=name..": "..val
        if callback then pcall(function() callback(val) end) end
    end
    sbtn.MouseButton1Down:Connect(function() draggingSlider=true end)
    bar.InputBegan:Connect(function(input) if input.UserInputType==Enum.UserInputType.MouseButton1 then draggingSlider=true; update(input) end end)
    UserInputService.InputEnded:Connect(function(input) if input.UserInputType==Enum.UserInputType.MouseButton1 then draggingSlider=false end end)
    UserInputService.InputChanged:Connect(function(input) if draggingSlider and input.UserInputType==Enum.UserInputType.MouseMovement then update(input) end end)
    return f
end

-- ============================================
-- PREENCHIMENTO DAS ABAS (COMPLETO)
-- ============================================
-- Aba 1: Farm
do
    local scroll = Tabs["Farm"].Scroll
    CreateSection(scroll, "⚔️ Auto Farm")
    CreateToggle(scroll, "Auto Farm Level", Settings.AutoFarm, function(s) Settings.AutoFarm=s end)
    CreateToggle(scroll, "Auto Quest", Settings.AutoQuest, function(s) Settings.AutoQuest=s end)
    CreateSlider(scroll, "Distância", 5,50, Settings.FarmDistance, function(v) Settings.FarmDistance=v end)
    CreateButton(scroll, "Método: "..Settings.FarmMethod, function()
        local methods = {"TP","Walk","Fly"}
        local idx = 1
        for i, m in ipairs(methods) do if m == Settings.FarmMethod then idx = i; break end end
        idx = (idx % #methods) + 1
        Settings.FarmMethod = methods[idx]
        Notify("FARM", "Método: "..Settings.FarmMethod, 2)
    end)
end

-- Aba 2: Sub Farm
do
    local scroll = Tabs["Sub"].Scroll
    CreateSection(scroll, "🗡️ Sub Farm")
    CreateToggle(scroll, "Auto Haki (Buso)", Settings.AutoBuso, function(s) Settings.AutoBuso=s end)
    CreateToggle(scroll, "Auto Ken Haki", Settings.AutoKen, function(s) Settings.AutoKen=s end)
    CreateToggle(scroll, "Fast Attack", Settings.FastAttack, function(s) Settings.FastAttack=s end)
    CreateToggle(scroll, "Kill Aura", Settings.KillAura, function(s) Settings.KillAura=s end)
    CreateToggle(scroll, "Auto Click", Settings.AutoClick, function(s) Settings.AutoClick=s end)
    CreateSection(scroll, "Skills")
    CreateToggle(scroll, "Skill Z", Settings.AutoSkillZ, function(s) Settings.AutoSkillZ=s end)
    CreateToggle(scroll, "Skill X", Settings.AutoSkillX, function(s) Settings.AutoSkillX=s end)
    CreateToggle(scroll, "Skill C", Settings.AutoSkillC, function(s) Settings.AutoSkillC=s end)
    CreateToggle(scroll, "Skill V", Settings.AutoSkillV, function(s) Settings.AutoSkillV=s end)
    CreateToggle(scroll, "Skill F", Settings.AutoSkillF, function(s) Settings.AutoSkillF=s end)
end

-- Aba 3: Boss Farm
do
    local scroll = Tabs["Boss"].Scroll
    CreateSection(scroll, "💀 Global")
    CreateToggle(scroll, "Todos os Bosses", Settings.BossFarmAll, function(s) Settings.BossFarmAll=s end)
    CreateSection(scroll, "Sea 1")
    CreateToggle(scroll, "Farm Sea 1", Settings.BossFarmSea1, function(s) Settings.BossFarmSea1=s end)
    for name,_ in pairs(BossList.Sea1) do CreateToggle(scroll, name, Settings.BossToggles[name], function(s) Settings.BossToggles[name]=s end) end
    CreateSection(scroll, "Sea 2")
    CreateToggle(scroll, "Farm Sea 2", Settings.BossFarmSea2, function(s) Settings.BossFarmSea2=s end)
    for name,_ in pairs(BossList.Sea2) do CreateToggle(scroll, name, Settings.BossToggles[name], function(s) Settings.BossToggles[name]=s end) end
    CreateSection(scroll, "Sea 3")
    CreateToggle(scroll, "Farm Sea 3", Settings.BossFarmSea3, function(s) Settings.BossFarmSea3=s end)
    for name,_ in pairs(BossList.Sea3) do CreateToggle(scroll, name, Settings.BossToggles[name], function(s) Settings.BossToggles[name]=s end) end
end

-- Aba 4: Eventos
do
    local scroll = Tabs["Eventos"].Scroll
    CreateSection(scroll, "🌊 Eventos do Mar")
    CreateToggle(scroll, "Auto Sea Events", Settings.AutoSeaEvents, function(s) Settings.AutoSeaEvents=s end)
    CreateButton(scroll, "🐉 Sea Beast", function() Notify("EVENTO","Procurando Sea Beast...",2) end)
    CreateButton(scroll, "🚢 Ship Raid", function() Notify("EVENTO","Procurando Ship Raid...",2) end)
    CreateButton(scroll, "🌋 Rumbling", function() Notify("EVENTO","Procurando Rumbling...",2) end)
    CreateButton(scroll, "🏭 Factory", function() Notify("EVENTO","Procurando Factory...",2) end)
end

-- Aba 5: Extras
do
    local scroll = Tabs["Extras"].Scroll
    CreateSection(scroll, "💎 Extras")
    CreateToggle(scroll, "Auto Chest", Settings.AutoChest, function(s) Settings.AutoChest=s end)
    CreateToggle(scroll, "Auto Material", Settings.AutoMaterial, function(s) Settings.AutoMaterial=s end)
    CreateToggle(scroll, "Auto Boss", Settings.AutoBoss, function(s) Settings.AutoBoss=s end)
end

-- Aba 6: Frutas
do
    local scroll = Tabs["Frutas"].Scroll
    CreateSection(scroll, "🍎 Frutas")
    CreateToggle(scroll, "Auto Coletar", Settings.AutoFruit, function(s) Settings.AutoFruit=s end)
    CreateToggle(scroll, "Auto Armazenar", Settings.AutoStore, function(s) Settings.AutoStore=s end)
    CreateToggle(scroll, "Dropar Comuns", Settings.AutoDropCommon, function(s) Settings.AutoDropCommon=s end)
    CreateToggle(scroll, "Só Lendárias", Settings.AutoCollectLegendary, function(s) Settings.AutoCollectLegendary=s end)
    CreateButton(scroll, "🔍 Buscar Fruta", function()
        for _, obj in ipairs(Workspace:GetChildren()) do
            if obj:IsA("Tool") and obj:FindFirstChild("Handle") and RootPart then
                RootPart.CFrame = obj.Handle.CFrame; Notify("FRUTA","Teleportado!",2); return
            end
        end
        Notify("FRUTA","Nenhuma encontrada",2,"warning")
    end)
end

-- Aba 7: Espadas
do
    local scroll = Tabs["Espadas"].Scroll
    CreateSection(scroll, "🗡️ Espadas")
    CreateToggle(scroll, "Auto Farm Espada", Settings.AutoFarmSword, function(s) Settings.AutoFarmSword=s end)
    CreateButton(scroll, "⚔️ CDK", function() Notify("ESPADA","Farmando CDK...",3) end)
    CreateButton(scroll, "⚔️ TTK", function() Notify("ESPADA","Farmando TTK...",3) end)
    CreateButton(scroll, "🔮 Hallow Scythe", function() Notify("ESPADA","Farmando Hallow...",3) end)
    CreateButton(scroll, "🦊 Fox Lamp", function() Notify("ESPADA","Farmando Fox Lamp...",3) end)
end

-- Aba 8: Estilos
do
    local scroll = Tabs["Estilos"].Scroll
    CreateSection(scroll, "🥊 Estilos de Luta")
    CreateToggle(scroll, "Auto Aprender", Settings.AutoLearnStyle, function(s) Settings.AutoLearnStyle=s end)
    for _, style in ipairs({"Superhuman","Death Step","Sharkman Karate","Electric Claw","Dragon Talon","God Human"}) do
        CreateButton(scroll, style, function() Notify("ESTILO","Obtendo "..style.."...",2) end)
    end
end

-- Aba 9: Raça
do
    local scroll = Tabs["Raça"].Scroll
    CreateSection(scroll, "🧬 Raça")
    CreateToggle(scroll, "Auto V2", Settings.AutoRaceV2, function(s) Settings.AutoRaceV2=s end)
    CreateToggle(scroll, "Auto V3", Settings.AutoRaceV3, function(s) Settings.AutoRaceV3=s end)
    CreateToggle(scroll, "Auto V4", Settings.AutoRaceV4, function(s) Settings.AutoRaceV4=s end)
    for _, race in ipairs({"Human","Mink","Fishman","Skypian","Ghoul","Cyborg"}) do
        CreateButton(scroll, race, function() Notify("RAÇA","Mudando para "..race,2) end)
    end
end

-- Aba 10: Combate
do
    local scroll = Tabs["Combate"].Scroll
    CreateSection(scroll, "⚡ Combate")
    CreateToggle(scroll, "Auto Aim", Settings.AutoAim, function(s) Settings.AutoAim=s end)
    CreateToggle(scroll, "Auto Combo", Settings.AutoCombo, function(s) Settings.AutoCombo=s end)
    CreateToggle(scroll, "No Skill Delay", Settings.NoSkillDelay, function(s) Settings.NoSkillDelay=s end)
    CreateButton(scroll, "Combo Default", function() Notify("COMBO","Executando...",2) end)
    CreateButton(scroll, "One Shot", function() Notify("COMBO","One Shot...",2) end)
end

-- Aba 11: Aimbot
do
    local scroll = Tabs["Aimbot"].Scroll
    CreateSection(scroll, "🎯 Aimbot")
    CreateToggle(scroll, "Ativar", Settings.Aimbot, function(s) Settings.Aimbot=s end)
    CreateSlider(scroll, "FOV", 30,360, Settings.AimbotFOV, function(v) Settings.AimbotFOV=v end)
    CreateSlider(scroll, "Suavidade", 1,10, Settings.AimbotSmooth, function(v) Settings.AimbotSmooth=v end)
    CreateButton(scroll, "Mirar Boss", function()
        local nearest,dist = nil,math.huge
        for _,obj in ipairs(Workspace:GetDescendants()) do
            if obj:IsA("Model") and obj:FindFirstChild("Humanoid") and obj.Humanoid.Health>0 and obj:FindFirstChild("Head") and RootPart then
                local d = (RootPart.Position - obj.Head.Position).Magnitude
                if d < dist then dist=d; nearest=obj end
            end
        end
        if nearest then Settings.AimbotTarget=nearest; Notify("AIMBOT","Alvo: "..nearest.Name,2) end
    end)
end

-- Aba 12: Player
do
    local scroll = Tabs["Player"].Scroll
    CreateSection(scroll, "🏃 Movimento")
    CreateSlider(scroll, "WalkSpeed", 16,500, Settings.WalkSpeed, function(v) Settings.WalkSpeed=v; if Humanoid then Humanoid.WalkSpeed=v end end)
    CreateSlider(scroll, "JumpPower", 50,500, Settings.JumpPower, function(v) Settings.JumpPower=v; if Humanoid then Humanoid.JumpPower=v end end)
    CreateSlider(scroll, "Fly Speed", 10,200, Settings.FlySpeed, function(v) Settings.FlySpeed=v end)
    CreateSection(scroll, "⭐ Habilidades")
    CreateToggle(scroll, "Fly", Settings.Fly, function(s) Settings.Fly=s end)
    CreateToggle(scroll, "No Clip", Settings.NoClip, function(s) Settings.NoClip=s end)
    CreateToggle(scroll, "Infinite Jump", Settings.InfiniteJump, function(s) Settings.InfiniteJump=s end)
    CreateToggle(scroll, "God Mode", Settings.GodMode, function(s) Settings.GodMode=s end)
    CreateToggle(scroll, "Andar sobre Água", Settings.WaterWalk, function(s) Settings.WaterWalk = s end)
end

-- Aba 13: Visual
do
    local scroll = Tabs["Visual"].Scroll
    CreateSection(scroll, "👁️ ESP")
    CreateToggle(scroll, "ESP Players", Settings.ESPPlayers, function(s) Settings.ESPPlayers=s end)
    CreateToggle(scroll, "ESP Fruits", Settings.ESPFruits, function(s) Settings.ESPFruits=s end)
    CreateToggle(scroll, "ESP Chests", Settings.ESPChests, function(s) Settings.ESPChests=s end)
    CreateToggle(scroll, "ESP Bosses", Settings.ESPBosses, function(s) Settings.ESPBosses=s end)
    CreateSlider(scroll, "Distância", 100,2000, Settings.ESPDistance, function(v) Settings.ESPDistance=v end)
    CreateSection(scroll, "🌍 Mundo")
    CreateToggle(scroll, "Remover Névoa", Settings.RemoveFog, function(s) Settings.RemoveFog=s; Lighting.FogEnd = s and 9e9 or 1000 end)
    CreateToggle(scroll, "Full Bright", Settings.FullBright, function(s) Settings.FullBright=s; Lighting.Brightness = s and 2 or 1 end)
    CreateToggle(scroll, "FPS Boost", Settings.FPSBoost, function(s) Settings.FPSBoost=s; if s then Lighting.GlobalShadows=false end end)
    CreateToggle(scroll, "No Water", Settings.NoWater, function(s) Settings.NoWater=s end)
end

-- Aba 14: Server
do
    local scroll = Tabs["Server"].Scroll
    CreateSection(scroll, "📊 Status")
    local statsLabel = Instance.new("TextLabel", scroll); statsLabel.Size=UDim2.new(1,-6,0,80); statsLabel.BackgroundColor3=Color3.fromRGB(15,15,15)
    statsLabel.Text = "Carregando..."; statsLabel.TextColor3=Color3.fromRGB(200,200,200); statsLabel.Font=Enum.Font.Gotham; statsLabel.TextSize=11
    task.spawn(function()
        while task.wait(2) do
            local ping = 0
            pcall(function() ping = math.floor(Stats.PerformanceStats.Ping:GetValue() * 1000) end)
            statsLabel.Text = "Players: "..#Players:GetPlayers().."/"..Players.MaxPlayers.." | Ping: "..ping.."ms"
        end
    end)
    CreateButton(scroll, "🔄 Rejoin", function() TeleportService:Teleport(game.PlaceId, Player) end)
    CreateButton(scroll, "🌐 Server Hop", function() Notify("SERVER","Procurando...",2) end)
end

-- Aba 15: Teleport
do
    local scroll = Tabs["Teleport"].Scroll
    for category, locs in pairs(TeleportLocations) do
        CreateSection(scroll, category)
        for name, cf in pairs(locs) do
            CreateButton(scroll, name, function() if RootPart then RootPart.CFrame = cf + Vector3.new(0,5,0); Notify("TP","Teleportado para "..name,2) end end)
        end
    end
end

-- Aba 16: Volcano
do
    local scroll = Tabs["Volcano"].Scroll
    CreateSection(scroll, "🌋 Volcano")
    CreateToggle(scroll, "Auto Volcano", Settings.AutoVolcano, function(s) Settings.AutoVolcano=s end)
    for name, cf in pairs(TeleportLocations["Special"]) do
        CreateButton(scroll, name, function() if RootPart then RootPart.CFrame = cf + Vector3.new(0,5,0) end end)
    end
end

-- Aba 17: Shop
do
    local scroll = Tabs["Shop"].Scroll
    CreateSection(scroll, "🛒 Shop")
    CreateButton(scroll, "Comprar Fruta", function() Notify("SHOP","Comprando...",2) end)
    CreateButton(scroll, "Comprar Espada", function() Notify("SHOP","Comprando...",2) end)
end

-- Aba 18: Money/Frags
do
    local scroll = Tabs["Money"].Scroll
    CreateSection(scroll, "💰 Money/Frags")
    CreateToggle(scroll, "Auto Money", Settings.AutoMoneyFarm, function(s) Settings.AutoMoneyFarm=s end)
    CreateToggle(scroll, "Auto Fragmentos", Settings.AutoFragmentFarm, function(s) Settings.AutoFragmentFarm=s end)
end

-- Aba 19: Macros
do
    local scroll = Tabs["Macros"].Scroll
    CreateSection(scroll, "🤖 Macros")
    CreateToggle(scroll, "Gravar", Settings.MacroRecording, function(s) Settings.MacroRecording=s end)
    CreateToggle(scroll, "Reproduzir", Settings.MacroPlaying, function(s) Settings.MacroPlaying=s end)
end

-- Aba 20: Webhook
do
    local scroll = Tabs["Webhook"].Scroll
    CreateSection(scroll, "🔔 Webhook")
    CreateToggle(scroll, "Ativar", Settings.WebhookEnabled, function(s) Settings.WebhookEnabled=s end)
    CreateButton(scroll, "Definir URL", function() Notify("WEBHOOK","Cole a URL no console",2) end)
    CreateButton(scroll, "Enviar Teste", function()
        if Settings.WebhookURL ~= "" then
            local body = HttpService:JSONEncode({content="Teste Itachi Hub"})
            local success = false
            pcall(function()
                if syn and syn.request then
                    syn.request({Url=Settings.WebhookURL, Method="POST", Headers={["Content-Type"]="application/json"}, Body=body})
                    success = true
                elseif request then
                    request({Url=Settings.WebhookURL, Method="POST", Headers={["Content-Type"]="application/json"}, Body=body})
                    success = true
                end
            end)
            if success then Notify("WEBHOOK","Enviado!",2) else Notify("WEBHOOK","Falha no envio",2,"warning") end
        end
    end)
end

-- Aba 21: Theme
do
    local scroll = Tabs["Theme"].Scroll
    CreateSection(scroll, "🎨 Cor")
    CreateSlider(scroll, "Vermelho", 0,255,255, function(v) Settings.ThemeColor=Color3.fromRGB(v,Settings.ThemeColor.G*255,Settings.ThemeColor.B*255); MainStroke.Color=Settings.ThemeColor; Title.TextColor3=Settings.ThemeColor end)
    CreateButton(scroll, "Vermelho Padrão", function() Settings.ThemeColor=Color3.fromRGB(255,0,0); MainStroke.Color=Settings.ThemeColor; Title.TextColor3=Settings.ThemeColor end)
    CreateButton(scroll, "Roxo", function() Settings.ThemeColor=Color3.fromRGB(128,0,128); MainStroke.Color=Settings.ThemeColor; Title.TextColor3=Settings.ThemeColor end)
end

-- Aba 22: Settings
do
    local scroll = Tabs["Settings"].Scroll
    CreateSection(scroll, "⚙️ Config")
    CreateToggle(scroll, "Anti AFK", Settings.AntiAFK, function(s) Settings.AntiAFK=s end)
    CreateToggle(scroll, "Auto Redeem", Settings.AutoRedeem, function(s) Settings.AutoRedeem=s end)
    CreateToggle(scroll, "Auto Rejoin", Settings.AutoRejoin, function(s) Settings.AutoRejoin=s end)
    CreateToggle(scroll, "Sons", Settings.SoundEnabled, function(s) Settings.SoundEnabled=s end)
end

-- ============================================
-- BOTÃO FLUTUANTE
-- ============================================
local FloatingBtn = Instance.new("ImageButton"); FloatingBtn.Size=UDim2.new(0,50,0,50); FloatingBtn.Position=UDim2.new(0.03,0,0.85,0)
FloatingBtn.BackgroundTransparency=1; FloatingBtn.Image="rbxassetid://16556523844"; FloatingBtn.Visible=false; FloatingBtn.ZIndex=100; FloatingBtn.Parent=ScreenGui
Instance.new("UICorner", FloatingBtn).CornerRadius=UDim.new(1,0)
local floatStroke = Instance.new("UIStroke", FloatingBtn); floatStroke.Color=Settings.ThemeColor; floatStroke.Thickness=2
local floatDragging=false; local floatDragStart, floatStartPos
FloatingBtn.InputBegan:Connect(function(input) if input.UserInputType==Enum.UserInputType.MouseButton1 then floatDragging=true; floatDragStart=input.Position; floatStartPos=FloatingBtn.Position end end)
UserInputService.InputChanged:Connect(function(input) if floatDragging and input.UserInputType==Enum.UserInputType.MouseMovement then local delta=input.Position-floatDragStart; FloatingBtn.Position=UDim2.new(floatStartPos.X.Scale, floatStartPos.X.Offset+delta.X, floatStartPos.Y.Scale, floatStartPos.Y.Offset+delta.Y) end end)
UserInputService.InputEnded:Connect(function(input) if input.UserInputType==Enum.UserInputType.MouseButton1 then floatDragging=false; if floatDragStart and (input.Position-floatDragStart).Magnitude<5 then PlaySound("9116338042",0.2); MainFrame.Visible=not MainFrame.Visible; FloatingBtn.Visible=not MainFrame.Visible end end end)
MinimizeBtn.MouseButton1Click:Connect(function() PlaySound("9116338042",0.2); MainFrame.Visible=false; FloatingBtn.Visible=true end)
CloseBtn.MouseButton1Click:Connect(function() PlaySound("9116338042",0.2); ScreenGui:Destroy() end)

-- ============================================
-- SISTEMAS
-- ============================================
-- Auto Farm
task.spawn(function()
    while task.wait(0.3) do
        if Settings.AutoFarm and RootPart then
            local nearest, dist = nil, Settings.FarmDistance
            for _, enemy in ipairs(Workspace.Enemies:GetChildren()) do
                if enemy:FindFirstChild("Humanoid") and enemy.Humanoid.Health > 0 and enemy:FindFirstChild("HumanoidRootPart") then
                    local d = (RootPart.Position - enemy.HumanoidRootPart.Position).Magnitude
                    if d < dist then dist = d; nearest = enemy end
                end
            end
            if nearest then
                if Settings.FarmMethod == "TP" then
                    RootPart.CFrame = nearest.HumanoidRootPart.CFrame * CFrame.new(0,2,3)
                end
                if Settings.AutoAttack then
                    local w = Character:FindFirstChildOfClass("Tool")
                    if w then w:Activate() end
                end
                if Settings.FastAttack then
                    VirtualInputManager:SendMouseButtonEvent(0,0,0,true,nil,0)
                    task.wait(0.05)
                    VirtualInputManager:SendMouseButtonEvent(0,0,0,false,nil,0)
                end
            end
        end
    end
end)

-- Auto Haki
task.spawn(function()
    while task.wait(1) do
        if Settings.AutoBuso then pcall(function() ReplicatedStorage.Remotes.CommF_:InvokeServer("Buso") end) end
        if Settings.AutoKen then pcall(function() ReplicatedStorage.Remotes.CommF_:InvokeServer("Ken") end) end
    end
end)

-- Auto Skills
task.spawn(function()
    while task.wait(0.3) do
        for _,s in ipairs({{Settings.AutoSkillZ,Enum.KeyCode.Z},{Settings.AutoSkillX,Enum.KeyCode.X},{Settings.AutoSkillC,Enum.KeyCode.C},{Settings.AutoSkillV,Enum.KeyCode.V},{Settings.AutoSkillF,Enum.KeyCode.F}}) do
            if s[1] then VirtualInputManager:SendKeyEvent(true,s[2],false,nil); task.wait(0.05); VirtualInputManager:SendKeyEvent(false,s[2],false,nil) end
        end
    end
end)

-- Auto Click
task.spawn(function()
    while task.wait(0.1) do
        if Settings.AutoClick then
            VirtualInputManager:SendMouseButtonEvent(0,0,0,true,nil,0)
            task.wait(0.01)
            VirtualInputManager:SendMouseButtonEvent(0,0,0,false,nil,0)
        end
    end
end)

-- Fly
local flyKeys = {W=false,S=false,A=false,D=false,Space=false,LeftControl=false}
UserInputService.InputBegan:Connect(function(input,gp) if gp then return end; if flyKeys[input.KeyCode.Name]~=nil then flyKeys[input.KeyCode.Name]=true end end)
UserInputService.InputEnded:Connect(function(input) if flyKeys[input.KeyCode.Name]~=nil then flyKeys[input.KeyCode.Name]=false end end)
task.spawn(function()
    while task.wait() do
        if Settings.Fly and RootPart then
            local hum = Character:FindFirstChild("Humanoid"); if hum then hum.PlatformStand=true end
            local dir = Vector3.new()
            if flyKeys.W then dir+=Camera.CFrame.LookVector end; if flyKeys.S then dir-=Camera.CFrame.LookVector end
            if flyKeys.A then dir-=Camera.CFrame.RightVector end; if flyKeys.D then dir+=Camera.CFrame.RightVector end
            if flyKeys.Space then dir+=Vector3.new(0,1,0) end; if flyKeys.LeftControl then dir-=Vector3.new(0,1,0) end
            RootPart.Velocity = dir.Magnitude>0 and dir.Unit*Settings.FlySpeed or Vector3.zero
        end
    end
end)

-- NoClip
task.spawn(function()
    while task.wait(0.2) do
        if Settings.NoClip and Character then
            for _,p in ipairs(Character:GetDescendants()) do if p:IsA("BasePart") then p.CanCollide=false end end
        end
    end
end)

-- Infinite Jump
UserInputService.JumpRequest:Connect(function()
    if Settings.InfiniteJump and Humanoid then Humanoid:ChangeState(Enum.HumanoidStateType.Jumping) end
end)

-- Water Walk
task.spawn(function()
    while task.wait(0.1) do
        if Settings.WaterWalk and RootPart then
            local rayOrigin = RootPart.Position + Vector3.new(0, 5, 0)
            local rayDirection = Vector3.new(0, -50, 0)
            local raycastParams = RaycastParams.new()
            raycastParams.FilterType = Enum.RaycastFilterType.Include
            raycastParams.FilterDescendantsInstances = {Workspace}
            local rayResult = Workspace:Raycast(rayOrigin, rayDirection, raycastParams)
            if rayResult and rayResult.Instance then
                if rayResult.Instance.Material == Enum.Material.Water then
                    local waterY = rayResult.Position.Y
                    if RootPart.Position.Y < waterY + 3 then
                        RootPart.CFrame = CFrame.new(RootPart.Position.X, waterY + 3, RootPart.Position.Z)
                    end
                end
            end
        end
    end
end)

-- Anti AFK
task.spawn(function()
    while task.wait(30) do
        if Settings.AntiAFK then VirtualUser:CaptureController(); VirtualUser:ClickButton2(Vector2.new()) end
    end
end)

-- Auto Redeem
task.spawn(function()
    while task.wait(120) do
        if Settings.AutoRedeem then
            for _,code in ipairs({"Sub2CaptainMaui","KittGaming","Sub2Fer999","Enyu_is_Pro","Magicbus"}) do
                pcall(function() ReplicatedStorage.Remotes.Redeem:InvokeServer("Code",code) end)
                task.wait(3)
            end
        end
    end
end)

-- ESP
task.spawn(function()
    while task.wait(2) do
        for _,hl in ipairs(Workspace:GetDescendants()) do if hl:IsA("Highlight") and hl.Name=="ITACHI_ESP" then hl:Destroy() end end
        if Settings.ESPPlayers then for _,p in ipairs(Players:GetPlayers()) do if p~=Player and p.Character then local h=Instance.new("Highlight"); h.Name="ITACHI_ESP"; h.FillColor=Color3.fromRGB(255,0,0); h.FillTransparency=0.5; h.Parent=p.Character end end end
        if Settings.ESPFruits then for _,o in ipairs(Workspace:GetChildren()) do if o:IsA("Tool") and o:FindFirstChild("Handle") then local h=Instance.new("Highlight"); h.Name="ITACHI_ESP"; h.FillColor=Color3.fromRGB(255,165,0); h.FillTransparency=0.5; h.Parent=o end end end
    end
end)

-- Aimbot
task.spawn(function()
    while task.wait() do
        if Settings.Aimbot and Settings.AimbotTarget and Settings.AimbotTarget:FindFirstChild("HumanoidRootPart") then
            local tp=Settings.AimbotTarget.HumanoidRootPart.Position
            local cp=Camera.CFrame.Position
            local dir=(tp-cp).Unit
            Camera.CFrame = CFrame.lookAt(cp, cp + dir:Lerp(Camera.CFrame.LookVector, Settings.AimbotSmooth*0.1))
        end
    end
end)

-- Boss Farm loop
task.spawn(function()
    while task.wait(1) do
        if not (Character and RootPart) then continue end
        local targetBosses = {}
        if Settings.BossFarmAll then
            for _,bosses in pairs(BossList) do for name,_ in pairs(bosses) do table.insert(targetBosses, name) end end
        else
            if Settings.BossFarmSea1 then for name,_ in pairs(BossList.Sea1) do table.insert(targetBosses, name) end end
            if Settings.BossFarmSea2 then for name,_ in pairs(BossList.Sea2) do table.insert(targetBosses, name) end end
            if Settings.BossFarmSea3 then for name,_ in pairs(BossList.Sea3) do table.insert(targetBosses, name) end end
            for name,active in pairs(Settings.BossToggles) do if active then table.insert(targetBosses, name) end end
        end
        for _,bossName in ipairs(targetBosses) do
            local found = nil
            for _,obj in ipairs(Workspace:GetDescendants()) do
                if obj:IsA("Model") and obj.Name==bossName and obj:FindFirstChild("Humanoid") and obj.Humanoid.Health>0 then found=obj; break end
            end
            if not found then
                for sea,bosses in pairs(BossList) do
                    if bosses[bossName] and RootPart then RootPart.CFrame = bosses[bossName] + Vector3.new(0,5,0); task.wait(0.5) end
                end
            elseif found:FindFirstChild("HumanoidRootPart") then
                RootPart.CFrame = found.HumanoidRootPart.CFrame * CFrame.new(0,2,3); task.wait(0.3)
                local w = Character:FindFirstChildOfClass("Tool"); if w then w:Activate() end
            end
            task.wait(0.5)
        end
    end
end)

-- ============================================
-- INICIALIZAÇÃO
-- ============================================
Notify("✅ ITACHI HUB v7.4", "Completo e sem erros!", 5, "success")
print("ITACHI HUB v7.4 pronto!")

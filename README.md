-- ==========================================
-- COLOQUE ESTE BLOCO NO TOPO ABSOLUTO DO SCRIPT
-- ==========================================
local jogosPermitidos = {
    83590755974448,
    99001115434148,
    133451168835128,
    125299131646017,
}

local function verificarJogo()
    for _, id in ipairs(jogosPermitidos) do
        if game.PlaceId == id then
            return true
        end
    end
    return false
end

if not verificarJogo() then
    warn("[Anti-Leak/Restrição] Este script não foi feito para rodar neste jogo!")
    return
end

-- ======================================================== --
--  I'M MENU // AIMBOT & SILENT AIM FUNCIONAIS + SEM MORTOS --
-- ======================================================== --

local CoreGui = game:GetService("CoreGui")
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local TweenService = game:GetService("TweenService")
local Lighting = game:GetService("Lighting")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local Camera = workspace.CurrentCamera
local LocalPlayer = Players.LocalPlayer

-- Remove interfaces anteriores se houver
if CoreGui:FindFirstChild("IMMenuUI") then CoreGui.IMMenuUI:Destroy() end
if CoreGui:FindFirstChild("IMLoadingUI") then CoreGui.IMLoadingUI:Destroy() end
if CoreGui:FindFirstChild("IMFpsUI") then CoreGui.IMFpsUI:Destroy() end

local isMobile = UserInputService.TouchEnabled and not UserInputService.KeyboardEnabled

-- ==========================================
-- TELA DE CARREGAMENTO (ESTILO PRETO & BRANCO)
-- ==========================================
local LoadGui = Instance.new("ScreenGui")
LoadGui.Name = "IMLoadingUI"
LoadGui.Parent = CoreGui
LoadGui.ResetOnSpawn = false

local LoadFrame = Instance.new("Frame", LoadGui)
LoadFrame.Size = isMobile and UDim2.new(0, 520, 0, 280) or UDim2.new(0, 760, 0, 420)
LoadFrame.Position = UDim2.new(0.5, LoadFrame.Size.X.Offset / -2, 0.5, LoadFrame.Size.Y.Offset / -2)
LoadFrame.BackgroundColor3 = Color3.fromRGB(10, 10, 10)
LoadFrame.BorderSizePixel = 0
Instance.new("UICorner", LoadFrame).CornerRadius = UDim.new(0, 12)

local LoadStroke = Instance.new("UIStroke", LoadFrame)
LoadStroke.Color = Color3.fromRGB(60, 60, 60)
LoadStroke.Thickness = 1.5

local LoadGlow = Instance.new("Frame", LoadFrame)
LoadGlow.Size = UDim2.new(1, 0, 0, 2)
LoadGlow.Position = UDim2.new(0, 0, 0, 0)
LoadGlow.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
LoadGlow.BackgroundTransparency = 0.5
LoadGlow.BorderSizePixel = 0
Instance.new("UICorner", LoadGlow).CornerRadius = UDim.new(1, 0)

local LoadTitleContainer = Instance.new("Frame", LoadFrame)
LoadTitleContainer.Size = UDim2.new(0, 300, 0, 80)
LoadTitleContainer.Position = UDim2.new(0.5, -150, 0.32, -40)
LoadTitleContainer.BackgroundTransparency = 1

local LoadTitle1 = Instance.new("TextLabel", LoadTitleContainer)
LoadTitle1.Size = UDim2.new(1, 0, 0, 35)
LoadTitle1.BackgroundTransparency = 1
LoadTitle1.Text = "I'M"
LoadTitle1.TextColor3 = Color3.fromRGB(255, 255, 255)
LoadTitle1.TextSize = 32
LoadTitle1.Font = Enum.Font.GothamBold

local LoadTitle2 = Instance.new("TextLabel", LoadTitleContainer)
LoadTitle2.Size = UDim2.new(1, 0, 0, 35)
LoadTitle2.Position = UDim2.new(0, 0, 0, 32)
LoadTitle2.BackgroundTransparency = 1
LoadTitle2.Text = "MENU"
LoadTitle2.TextColor3 = Color3.fromRGB(170, 170, 170)
LoadTitle2.TextSize = 24
LoadTitle2.Font = Enum.Font.GothamBold

local LoadSub = Instance.new("TextLabel", LoadFrame)
LoadSub.Size = UDim2.new(1, 0, 0, 25)
LoadSub.Position = UDim2.new(0, 0, 0.58, 0)
LoadSub.BackgroundTransparency = 1
LoadSub.Text = "Carregando módulos e segurança..."
LoadSub.TextColor3 = Color3.fromRGB(180, 180, 180)
LoadSub.TextSize = 12
LoadSub.Font = Enum.Font.GothamMedium

local BarBg = Instance.new("Frame", LoadFrame)
BarBg.Size = UDim2.new(0, 420, 0, 8)
BarBg.Position = UDim2.new(0.5, -210, 0.73, 0)
BarBg.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
BarBg.BorderSizePixel = 0
Instance.new("UICorner", BarBg).CornerRadius = UDim.new(1, 0)

local BarStroke = Instance.new("UIStroke", BarBg)
BarStroke.Color = Color3.fromRGB(45, 45, 45)
BarStroke.Thickness = 1

local BarFill = Instance.new("Frame", BarBg)
BarFill.Size = UDim2.new(0, 0, 1, 0)
BarFill.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
BarFill.BorderSizePixel = 0
Instance.new("UICorner", BarFill).CornerRadius = UDim.new(1, 0)

local StartMenuMain = false

task.spawn(function()
    TweenService:Create(BarFill, TweenInfo.new(3, Enum.EasingStyle.Quart, Enum.EasingDirection.Out), {Size = UDim2.new(1, 0, 1, 0)}):Play()
    task.wait(3.2)
    LoadSub.Text = "Sucesso! Inicializando interface..."
    LoadSub.TextColor3 = Color3.fromRGB(255, 255, 255)
    task.wait(0.8)
    TweenService:Create(LoadFrame, TweenInfo.new(0.4, Enum.EasingStyle.Back, Enum.EasingDirection.In), {Size = UDim2.new(0, 0, 0, 0), Position = UDim2.new(0.5, 0, 0.5, 0)}):Play()
    task.wait(0.4)
    LoadGui:Destroy()
    StartMenuMain = true
end)

repeat task.wait() until StartMenuMain

-- ==========================================
-- CONFIGURAÇÕES GERAIS
-- ==========================================
getgenv().Settings = {
    ShowFov = false,
    SilentAimActive = false,
    AimbotActive = false,
    WallCheckEnabled = false,
    AimTarget = "Head",
    FovRadius = 120,
    AimSmoothing = 5,
    
    Boxes = false,
    Tracers = false,
    HealthBar = false,
    Skeleton = false,
    HeadCircle = false,
    ESPStuds = false,
    MaxDistance = 300,
    TracerColor = Color3.fromRGB(255, 255, 255),
    BoxColor = Color3.fromRGB(255, 255, 255),
    SkeletonColor = Color3.fromRGB(255, 255, 255),
    HealthBgColor = Color3.fromRGB(30, 30, 30),
    HealthGoodColor = Color3.fromRGB(0, 255, 0),
    HealthLowColor = Color3.fromRGB(255, 50, 50),
    TextColor = Color3.fromRGB(255, 255, 255),

    InvisibleActive = false,
    AntiReportActive = true,
    AntiBanActive = true,
    FpsUnlockActive = false,
    PotatoModeActive = false,
    
    FullBrightActive = false,
    AntiAfkActive = false,
    NoFogActive = false,
    
    SpinBotActive = false,
    SpinSpeed = 50,

    MobileBtn = false,
    BindPanel = true,
    FpsWidget = false
}

local ToggleStates = {}
local RegisteredKeyBinds = {}
local OriginalLighting = {
    Ambient = Lighting.Ambient,
    OutdoorAmbient = Lighting.OutdoorAmbient,
    Brightness = Lighting.Brightness,
    ClockTime = Lighting.ClockTime,
    FogEnd = Lighting.FogEnd
}

pcall(function() setfpscap(360) end)

local OpenKey = Enum.KeyCode.Insert
local ListeningForKey = false
local MenuVisible = true
local IsMinimized = false

local Blur = Instance.new("BlurEffect", Lighting)
Blur.Size = 15
Blur.Enabled = true

local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "IMMenuUI"
ScreenGui.Parent = CoreGui
ScreenGui.ResetOnSpawn = false

local menuSize = isMobile and UDim2.new(0, 560, 0, 320) or UDim2.new(0, 920, 0, 520)

-- Janela Principal
local MainFrame = Instance.new("Frame", ScreenGui)
MainFrame.Size = menuSize
MainFrame.Position = UDim2.new(0.5, menuSize.X.Offset / -2, 0.5, menuSize.Y.Offset / -2)
MainFrame.BackgroundColor3 = Color3.fromRGB(10, 10, 10)
MainFrame.BorderSizePixel = 0
Instance.new("UICorner", MainFrame).CornerRadius = UDim.new(0, 8)

local MainStroke = Instance.new("UIStroke", MainFrame)
MainStroke.Color = Color3.fromRGB(40, 40, 40)
MainStroke.Thickness = 1

local LogoContainer = Instance.new("Frame", MainFrame)
LogoContainer.Size = UDim2.new(0, 180, 0, 65)
LogoContainer.BackgroundTransparency = 1

local LogoText1 = Instance.new("TextLabel", LogoContainer)
LogoText1.Size = UDim2.new(1, 0, 0, 22)
LogoText1.Position = UDim2.new(0, 18, 0, 15)
LogoText1.BackgroundTransparency = 1
LogoText1.Text = "I'M"
LogoText1.TextColor3 = Color3.fromRGB(255, 255, 255)
LogoText1.TextSize = 17
LogoText1.Font = Enum.Font.GothamBold
LogoText1.TextXAlignment = Enum.TextXAlignment.Left

local LogoText2 = Instance.new("TextLabel", LogoContainer)
LogoText2.Size = UDim2.new(1, 0, 0, 20)
LogoText2.Position = UDim2.new(0, 18, 0, 32)
LogoText2.BackgroundTransparency = 1
LogoText2.Text = "MENU"
LogoText2.TextColor3 = Color3.fromRGB(255, 255, 255)
LogoText2.TextSize = 15
LogoText2.Font = Enum.Font.GothamBold
LogoText2.TextXAlignment = Enum.TextXAlignment.Left

local CloseBtn = Instance.new("TextButton", MainFrame)
CloseBtn.Size = UDim2.new(0, 30, 0, 30)
CloseBtn.Position = UDim2.new(1, -35, 0, 15)
CloseBtn.Text = "×"
CloseBtn.TextColor3 = Color3.fromRGB(200, 200, 200)
CloseBtn.BackgroundTransparency = 1
CloseBtn.TextSize = 22
CloseBtn.Font = Enum.Font.GothamBold

local MinimizeBtn = Instance.new("TextButton", MainFrame)
MinimizeBtn.Size = UDim2.new(0, 30, 0, 30)
MinimizeBtn.Position = UDim2.new(1, -65, 0, 15)
MinimizeBtn.Text = "−"
MinimizeBtn.TextColor3 = Color3.fromRGB(200, 200, 200)
MinimizeBtn.BackgroundTransparency = 1
MinimizeBtn.TextSize = 22
MinimizeBtn.Font = Enum.Font.GothamBold

local Sidebar = Instance.new("Frame", MainFrame)
Sidebar.Size = UDim2.new(0, 180, 1, -65)
Sidebar.Position = UDim2.new(0, 0, 0, 65)
Sidebar.BackgroundTransparency = 1

local SidebarLayout = Instance.new("UIListLayout", Sidebar)
SidebarLayout.SortOrder = Enum.SortOrder.LayoutOrder
SidebarLayout.Padding = UDim.new(0, 10)

local ContentArea = Instance.new("Frame", MainFrame)
ContentArea.Size = UDim2.new(1, -195, 1, -20)
ContentArea.Position = UDim2.new(0, 185, 0, 10)
ContentArea.BackgroundColor3 = Color3.fromRGB(14, 14, 14)
ContentArea.BorderSizePixel = 0
Instance.new("UICorner", ContentArea).CornerRadius = UDim.new(0, 6)

local Pages = {}
local function CreatePage(name)
    local page = Instance.new("ScrollingFrame", ContentArea)
    page.Size = UDim2.new(1, -20, 1, -20)
    page.Position = UDim2.new(0, 10, 0, 10)
    page.BackgroundTransparency = 1
    page.Visible = false
    page.CanvasSize = UDim2.new(0, 0, 0, 0)
    page.AutomaticCanvasSize = Enum.AutomaticSize.Y
    page.ScrollBarThickness = 3
    Pages[name] = page
    return page
end

local CombatPage = CreatePage("Combat")
local VisualsPage = CreatePage("Visuais")
local PlayerPage = CreatePage("Player")
local SettingsPage = CreatePage("Settings")

for _, pageName in ipairs({"Combat", "Visuais", "Player", "Settings"}) do
    local layout = Instance.new("UIListLayout", Pages[pageName])
    layout.SortOrder = Enum.SortOrder.LayoutOrder
    layout.Padding = UDim.new(0, 6)
end

local function AddTabButton(name, targetPage, iconId)
    local btn = Instance.new("TextButton", Sidebar)
    btn.Size = UDim2.new(1, -20, 0, 35)
    btn.Position = UDim2.new(0, 10, 0, 0)
    btn.BackgroundTransparency = 1
    btn.Text = "          " .. name
    btn.TextColor3 = Color3.fromRGB(150, 150, 150)
    btn.TextSize = 13
    btn.Font = Enum.Font.GothamMedium
    btn.TextXAlignment = Enum.TextXAlignment.Left

    local icon = Instance.new("ImageLabel", btn)
    icon.Size = UDim2.new(0, 18, 0, 18)
    icon.Position = UDim2.new(0, 6, 0.5, -9)
    icon.BackgroundTransparency = 1
    icon.Image = iconId
    icon.ImageColor3 = Color3.fromRGB(150, 150, 150)

    local activeIndicator = Instance.new("Frame", btn)
    activeIndicator.Size = UDim2.new(0, 3, 0, 20)
    activeIndicator.Position = UDim2.new(0, -6, 0.5, -10)
    activeIndicator.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
    activeIndicator.BorderSizePixel = 0
    activeIndicator.Visible = false
    Instance.new("UICorner", activeIndicator).CornerRadius = UDim.new(1, 0)
    
    btn.MouseButton1Click:Connect(function()
        for _, p in pairs(Pages) do p.Visible = false end
        for _, b in pairs(Sidebar:GetChildren()) do
            if b:IsA("TextButton") then
                b.TextColor3 = Color3.fromRGB(150, 150, 150)
                local ico = b:FindFirstChildOfClass("ImageLabel")
                if ico then ico.ImageColor3 = Color3.fromRGB(150, 150, 150) end
                local ind = b:FindFirstChildOfClass("Frame")
                if ind then ind.Visible = false end
            end
        end
        targetPage.Visible = true
        btn.TextColor3 = Color3.fromRGB(255, 255, 255)
        icon.ImageColor3 = Color3.fromRGB(255, 255, 255)
        activeIndicator.Visible = true
    end)
end

AddTabButton("Combat", CombatPage, "rbxassetid://7733765307")
AddTabButton("Visuais", VisualsPage, "rbxassetid://7733774602")
AddTabButton("Player", PlayerPage, "rbxassetid://7743871002")
AddTabButton("Settings", SettingsPage, "rbxassetid://7733954760")
CombatPage.Visible = true
Sidebar:GetChildren()[2]:FindFirstChildOfClass("Frame").Visible = true

-- Arrastar Menu Principal
local draggingMain, dragStartMain, startPosMain
LogoContainer.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
        draggingMain = true
        dragStartMain = input.Position
        startPosMain = MainFrame.Position
        input.Changed:Connect(function()
            if input.UserInputState == Enum.UserInputState.End then draggingMain = false end
        end)
    end
end)

UserInputService.InputChanged:Connect(function(input)
    if draggingMain and (input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch) then
        local delta = input.Position - dragStartMain
        MainFrame.Position = UDim2.new(startPosMain.X.Scale, startPosMain.X.Offset + delta.X, startPosMain.Y.Scale, startPosMain.Y.Offset + delta.Y)
    end
end)

MinimizeBtn.MouseButton1Click:Connect(function()
    IsMinimized = not IsMinimized
    ContentArea.Visible = not IsMinimized
    Sidebar.Visible = not IsMinimized
    MainFrame:TweenSize(IsMinimized and UDim2.new(0, menuSize.X.Offset, 0, 65) or menuSize, "Out", "Quart", 0.3, true)
    Blur.Size = IsMinimized and 0 or 15
    MinimizeBtn.Text = IsMinimized and "+" or "−"
end)

CloseBtn.MouseButton1Click:Connect(function()
    ScreenGui:Destroy()
    Blur:Destroy()
end)

local function ToggleMenu()
    MenuVisible = not MenuVisible
    MainFrame.Visible = MenuVisible
    Blur.Size = MenuVisible and 15 or 0
end

-- Botão Flutuante Mobile
local FloatingBtn = Instance.new("ImageButton", ScreenGui)
FloatingBtn.Size = UDim2.new(0, 48, 0, 48)
FloatingBtn.Position = UDim2.new(0, 15, 0.5, -24)
FloatingBtn.BackgroundColor3 = Color3.fromRGB(15, 15, 15)
FloatingBtn.Visible = false
FloatingBtn.Image = "rbxassetid://6034293975"
Instance.new("UICorner", FloatingBtn).CornerRadius = UDim.new(1, 0)
local FloatStroke = Instance.new("UIStroke", FloatingBtn)
FloatStroke.Color = Color3.fromRGB(255, 255, 255)
FloatStroke.Thickness = 1.5

local draggingMob, dragStartMob, startPosMob
FloatingBtn.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
        draggingMob = true
        dragStartMob = input.Position
        startPosMob = FloatingBtn.Position
        input.Changed:Connect(function()
            if input.UserInputState == Enum.UserInputState.End then draggingMob = false end
        end)
    end
end)

UserInputService.InputChanged:Connect(function(input)
    if draggingMob and (input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch) then
        local delta = input.Position - dragStartMob
        FloatingBtn.Position = UDim2.new(startPosMob.X.Scale, startPosMob.X.Offset + delta.X, startPosMob.Y.Scale, startPosMob.Y.Offset + delta.Y)
    end
end)
FloatingBtn.MouseButton1Click:Connect(ToggleMenu)

-- Painel de Binds
local BindsFrame = Instance.new("Frame", ScreenGui)
BindsFrame.Size = UDim2.new(0, 210, 0, 32)
BindsFrame.Position = UDim2.new(0.02, 0, 0.25, 0)
BindsFrame.BackgroundColor3 = Color3.fromRGB(12, 12, 12)
BindsFrame.BackgroundTransparency = 0.2
BindsFrame.BorderSizePixel = 0
BindsFrame.ClipsDescendants = true
Instance.new("UICorner", BindsFrame).CornerRadius = UDim.new(0, 6)

local BindsTopBar = Instance.new("Frame", BindsFrame)
BindsTopBar.Size = UDim2.new(1, 0, 0, 32)
BindsTopBar.BackgroundColor3 = Color3.fromRGB(10, 10, 10)
BindsTopBar.BorderSizePixel = 0
Instance.new("UICorner", BindsTopBar).CornerRadius = UDim.new(0, 6)

local draggingBinds, dragStartBinds, startPosBinds
BindsTopBar.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
        draggingBinds = true
        dragStartBinds = input.Position
        startPosBinds = BindsFrame.Position
        input.Changed:Connect(function()
            if input.UserInputState == Enum.UserInputState.End then draggingBinds = false end
        end)
    end
end)

UserInputService.InputChanged:Connect(function(input)
    if draggingBinds and (input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch) then
        local delta = input.Position - dragStartBinds
        BindsFrame.Position = UDim2.new(startPosBinds.X.Scale, startPosBinds.X.Offset + delta.X, startPosBinds.Y.Scale, startPosBinds.Y.Offset + delta.Y)
    end
end)

local KeyboardIcon = Instance.new("TextLabel", BindsTopBar)
KeyboardIcon.Size = UDim2.new(0, 24, 0, 18)
KeyboardIcon.Position = UDim2.new(0, 10, 0.5, -9)
KeyboardIcon.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
KeyboardIcon.Text = "⌨"
KeyboardIcon.TextColor3 = Color3.fromRGB(255, 255, 255)
KeyboardIcon.TextSize = 11
KeyboardIcon.Font = Enum.Font.GothamBold
Instance.new("UICorner", KeyboardIcon).CornerRadius = UDim.new(0, 3)

local BindsTitle = Instance.new("TextLabel", BindsTopBar)
BindsTitle.Size = UDim2.new(1, -45, 1, 0)
BindsTitle.Position = UDim2.new(0, 42, 0, 0)
BindsTitle.BackgroundTransparency = 1
BindsTitle.Text = "Binds"
BindsTitle.TextColor3 = Color3.fromRGB(240, 240, 240)
BindsTitle.TextSize = 13
BindsTitle.Font = Enum.Font.GothamBold
BindsTitle.TextXAlignment = Enum.TextXAlignment.Left

local BindsListContainer = Instance.new("Frame", BindsFrame)
BindsListContainer.Size = UDim2.new(1, 0, 1, -32)
BindsListContainer.Position = UDim2.new(0, 0, 0, 32)
BindsListContainer.BackgroundColor3 = Color3.fromRGB(16, 16, 16)
BindsListContainer.BackgroundTransparency = 0.4
BindsListContainer.BorderSizePixel = 0

local BindsLayout = Instance.new("UIListLayout", BindsListContainer)
BindsLayout.SortOrder = Enum.SortOrder.LayoutOrder

local function UpdateBindsSize()
    local count = 0
    for _, _ in pairs(BindsListContainer:GetChildren()) do
        if _ ~= BindsLayout then count = count + 1 end
    end
    BindsFrame.Size = UDim2.new(0, 210, 0, 32 + (count * 24))
end

local ActiveBindsUI = {}

local function AddToggleOption(parentContainer, labelText, defaultKey, settingKey, callback)
    local optionFrame = Instance.new("Frame", parentContainer)
    optionFrame.Size = UDim2.new(1, 0, 0, 36)
    optionFrame.BackgroundTransparency = 1

    local label = Instance.new("TextLabel", optionFrame)
    label.Size = UDim2.new(0.65, 0, 1, 0)
    label.Position = UDim2.new(0, 5, 0, 0)
    label.BackgroundTransparency = 1
    label.Text = labelText
    label.TextColor3 = Color3.fromRGB(200, 200, 200)
    label.TextSize = 12
    label.Font = Enum.Font.Gotham
    label.TextXAlignment = Enum.TextXAlignment.Left

    local keyButton = Instance.new("TextButton", optionFrame)
    keyButton.Size = UDim2.new(0, 26, 0, 20)
    keyButton.Position = UDim2.new(1, -90, 0.5, -10)
    keyButton.BackgroundColor3 = Color3.fromRGB(22, 22, 22)
    keyButton.Text = "⌨"
    keyButton.TextColor3 = Color3.fromRGB(255, 255, 255)
    keyButton.TextSize = 11
    keyButton.Font = Enum.Font.GothamBold
    Instance.new("UICorner", keyButton).CornerRadius = UDim.new(0, 4)

    local toggleBtn = Instance.new("TextButton", optionFrame)
    toggleBtn.Size = UDim2.new(0, 38, 0, 20)
    toggleBtn.Position = UDim2.new(1, -45, 0.5, -10)
    toggleBtn.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
    toggleBtn.Text = ""
    Instance.new("UICorner", toggleBtn).CornerRadius = UDim.new(1, 0)

    local circle = Instance.new("Frame", toggleBtn)
    circle.Size = UDim2.new(0, 16, 0, 16)
    circle.Position = UDim2.new(0, 2, 0.5, -8)
    circle.BackgroundColor3 = Color3.fromRGB(150, 150, 150)
    Instance.new("UICorner", circle).CornerRadius = UDim.new(1, 0)

    local activeState = getgenv().Settings[settingKey] or false
    local currentKey = defaultKey
    local waitingForKey = false

    ToggleStates[settingKey] = {
        Set = function(newState)
            activeState = newState
            getgenv().Settings[settingKey] = activeState
            
            if activeState then
                circle:TweenPosition(UDim2.new(1, -18, 0.5, -8), Enum.EasingDirection.Out, Enum.EasingStyle.Quad, 0.15, true)
                circle.BackgroundColor3 = Color3.fromRGB(10, 10, 10)
                toggleBtn.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
            else
                circle:TweenPosition(UDim2.new(0, 2, 0.5, -8), Enum.EasingDirection.Out, Enum.EasingStyle.Quad, 0.15, true)
                circle.BackgroundColor3 = Color3.fromRGB(150, 150, 150)
                toggleBtn.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
            end
            
            if callback then callback(activeState) end

            if activeState and currentKey ~= Enum.KeyCode.None then
                if not ActiveBindsUI[labelText] then
                    local bindEntry = Instance.new("TextLabel", BindsListContainer)
                    bindEntry.Size = UDim2.new(1, 0, 0, 24)
                    bindEntry.BackgroundTransparency = 1
                    bindEntry.Text = "  • " .. labelText .. "  [" .. currentKey.Name .. "]"
                    bindEntry.TextColor3 = Color3.fromRGB(200, 200, 200)
                    bindEntry.TextSize = 11
                    bindEntry.Font = Enum.Font.Gotham
                    bindEntry.TextXAlignment = Enum.TextXAlignment.Left
                    ActiveBindsUI[labelText] = bindEntry
                    UpdateBindsSize()
                end
            else
                if ActiveBindsUI[labelText] then
                    ActiveBindsUI[labelText]:Destroy()
                    ActiveBindsUI[labelText] = nil
                    UpdateBindsSize()
                end
            end
        end,
        GetState = function() return activeState end,
        GetKey = function() return currentKey end
    }

    ToggleStates[settingKey].Set(activeState)

    keyButton.MouseButton1Click:Connect(function()
        if waitingForKey then return end
        waitingForKey = true
        keyButton.Text = "..."
        local conn
        conn = UserInputService.InputBegan:Connect(function(input)
            if input.UserInputType == Enum.UserInputType.Keyboard then
                currentKey = input.KeyCode
                keyButton.Text = "⌨"
                waitingForKey = false
                conn:Disconnect()
                
                RegisteredKeyBinds[currentKey] = settingKey

                if ToggleStates[settingKey].GetState() and ActiveBindsUI[labelText] then
                    ActiveBindsUI[labelText].Text = "  • " .. labelText .. "  [" .. currentKey.Name .. "]"
                end
            end
        end)
    end)

    if currentKey ~= Enum.KeyCode.None then
        RegisteredKeyBinds[currentKey] = settingKey
    end

    toggleBtn.MouseButton1Click:Connect(function() 
        ToggleStates[settingKey].Set(not ToggleStates[settingKey].GetState()) 
    end)
end

UserInputService.InputBegan:Connect(function(input, gpe)
    if not gpe and input.UserInputType == Enum.UserInputType.Keyboard then
        if input.KeyCode == OpenKey then
            ToggleMenu()
        else
            local targetSetting = RegisteredKeyBinds[input.KeyCode]
            if targetSetting and ToggleStates[targetSetting] then
                local currentState = ToggleStates[targetSetting].GetState()
                ToggleStates[targetSetting].Set(not currentState)
            end
        end
    end
end)

local function AddSliderOption(parentContainer, labelText, min, max, default, suffix, settingKey, callback)
    local sliderFrame = Instance.new("Frame", parentContainer)
    sliderFrame.Size = UDim2.new(1, 0, 0, 48)
    sliderFrame.BackgroundTransparency = 1

    local label = Instance.new("TextLabel", sliderFrame)
    label.Size = UDim2.new(0.65, 0, 0, 20)
    label.Position = UDim2.new(0, 5, 0, 0)
    label.BackgroundTransparency = 1
    label.Text = labelText
    label.TextColor3 = Color3.fromRGB(200, 200, 200)
    label.TextSize = 12
    label.Font = Enum.Font.Gotham
    label.TextXAlignment = Enum.TextXAlignment.Left

    local valLabel = Instance.new("TextLabel", sliderFrame)
    valLabel.Size = UDim2.new(0.35, -5, 0, 20)
    valLabel.Position = UDim2.new(0.65, 0, 0, 0)
    valLabel.BackgroundTransparency = 1
    valLabel.Text = tostring(default) .. " " .. suffix
    valLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
    valLabel.TextSize = 12
    valLabel.Font = Enum.Font.GothamBold
    valLabel.TextXAlignment = Enum.TextXAlignment.Right

    local bgBar = Instance.new("TextButton", sliderFrame)
    bgBar.Size = UDim2.new(1, -10, 0, 6)
    bgBar.Position = UDim2.new(0, 5, 0, 28)
    bgBar.BackgroundColor3 = Color3.fromRGB(22, 22, 22)
    bgBar.Text = ""
    bgBar.AutoButtonColor = false
    Instance.new("UICorner", bgBar).CornerRadius = UDim.new(1, 0)

    local fillBar = Instance.new("Frame", bgBar)
    fillBar.Size = UDim2.new((default - min) / (max - min), 0, 1, 0)
    fillBar.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
    fillBar.BorderSizePixel = 0
    Instance.new("UICorner", fillBar).CornerRadius = UDim.new(1, 0)

    local dragging = false
    local function updateSlider(input)
        local pos = math.clamp((input.Position.X - bgBar.AbsolutePosition.X) / bgBar.AbsoluteSize.X, 0, 1)
        local val = math.floor(min + (max - min) * pos)
        fillBar.Size = UDim2.new(pos, 0, 1, 0)
        valLabel.Text = tostring(val) .. " " .. suffix
        getgenv().Settings[settingKey] = val
        if callback then callback(val) end
    end

    bgBar.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
            dragging = true
            updateSlider(input)
            input.Changed:Connect(function()
                if input.UserInputState == Enum.UserInputState.End then dragging = false end
            end)
        end
    end)

    UserInputService.InputChanged:Connect(function(input)
        if dragging and (input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch) then
            updateSlider(input)
        end
    end)
end

local function AddSectionHeader(parentContainer, text)
    local label = Instance.new("TextLabel", parentContainer)
    label.Size = UDim2.new(1, 0, 0, 26)
    label.BackgroundTransparency = 1
    label.Text = "  " .. string.upper(text)
    label.TextColor3 = Color3.fromRGB(255, 255, 255)
    label.TextSize = 11
    label.Font = Enum.Font.GothamBold
    label.TextXAlignment = Enum.TextXAlignment.Left
end

-- ==========================================
-- CONTEÚDO DAS ABAS
-- ==========================================
local CombatLeft = Instance.new("ScrollingFrame", CombatPage)
CombatLeft.Size = UDim2.new(0.48, 0, 1, 0)
CombatLeft.BackgroundTransparency = 1
CombatLeft.CanvasSize = UDim2.new(0, 0, 0, 0)
CombatLeft.AutomaticCanvasSize = Enum.AutomaticSize.Y
CombatLeft.ScrollBarThickness = 2
local CLeftLayout = Instance.new("UIListLayout", CombatLeft)
CLeftLayout.SortOrder = Enum.SortOrder.LayoutOrder
CLeftLayout.Padding = UDim.new(0, 6)

local CombatRight = Instance.new("ScrollingFrame", CombatPage)
CombatRight.Size = UDim2.new(0.48, 0, 1, 0)
CombatRight.Position = UDim2.new(0.52, 0, 0, 0)
CombatRight.BackgroundTransparency = 1
CombatRight.CanvasSize = UDim2.new(0, 0, 0, 0)
CombatRight.AutomaticCanvasSize = Enum.AutomaticSize.Y
CombatRight.ScrollBarThickness = 2
local CRightLayout = Instance.new("UIListLayout", CombatRight)
CRightLayout.SortOrder = Enum.SortOrder.LayoutOrder
CRightLayout.Padding = UDim.new(0, 6)

-- Combat Esquerda
AddSectionHeader(CombatLeft, "Combat Systems")
AddToggleOption(CombatLeft, "Show FOV Circle", Enum.KeyCode.None, "ShowFov", function(v) 
    getgenv().Settings.ShowFov = v 
    if fovCircle then fovCircle.Visible = v end
end)
AddSliderOption(CombatLeft, "FOV Radius", 10, 800, getgenv().Settings.FovRadius, "px", "FovRadius", function(v) 
    getgenv().Settings.FovRadius = v 
    if fovCircle then fovCircle.Radius = v end
end)
AddSliderOption(CombatLeft, "Aim Smoothing", 1, 20, getgenv().Settings.AimSmoothing, "x", "AimSmoothing", function(v)
    getgenv().Settings.AimSmoothing = v
end)
AddToggleOption(CombatLeft, "Silent Aim (Redir)", Enum.KeyCode.None, "SilentAimActive", function(v) getgenv().Settings.SilentAimActive = v end)
AddToggleOption(CombatLeft, "Aimbot", Enum.KeyCode.None, "AimbotActive", function(v) getgenv().Settings.AimbotActive = v end)
AddToggleOption(CombatLeft, "Wall Check", Enum.KeyCode.None, "WallCheckEnabled", function(v) getgenv().Settings.WallCheckEnabled = v end)

AddSectionHeader(CombatLeft, "Player Utility")
AddToggleOption(CombatLeft, "Toggle Invisible", Enum.KeyCode.None, "InvisibleActive", function(state)
    local char = LocalPlayer.Character
    if char then
        for _, part in pairs(char:GetDescendants()) do
            if part:IsA("BasePart") or part:IsA("Decal") then
                if part.Name ~= "HumanoidRootPart" then part.Transparency = state and 1 or 0 end
            end
        end
    end
end)

-- Combat Direita (Teleporte)
AddSectionHeader(CombatRight, "Instant Teleport Panel")
local TpInput = Instance.new("TextBox", CombatRight)
TpInput.Size = UDim2.new(1, 0, 0, 32)
TpInput.PlaceholderText = "Type Player Name..."
TpInput.Text = ""
TpInput.BackgroundColor3 = Color3.fromRGB(22, 22, 22)
TpInput.TextColor3 = Color3.fromRGB(255, 255, 255)
TpInput.Font = Enum.Font.Gotham
TpInput.TextSize = 11
Instance.new("UICorner", TpInput).CornerRadius = UDim.new(0, 4)

local TpBtn = Instance.new("TextButton", CombatRight)
TpBtn.Size = UDim2.new(1, 0, 0, 32)
TpBtn.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
TpBtn.Text = "TELEPORT"
TpBtn.TextColor3 = Color3.fromRGB(10, 10, 10)
TpBtn.Font = Enum.Font.GothamBold
TpBtn.TextSize = 10
Instance.new("UICorner", TpBtn).CornerRadius = UDim.new(0, 4)

local function GetPlayerFromName(text)
    text = string.lower(text)
    text = string.gsub(text, "^%s*(.-)%s*$", "%1")
    if text ~= "" then
        for _, p in pairs(Players:GetPlayers()) do
            if p ~= LocalPlayer and (string.sub(string.lower(p.Name), 1, string.len(text)) == text or string.sub(string.lower(p.DisplayName), 1, string.len(text)) == text) then return p end
        end
    end
    return nil
end

TpBtn.MouseButton1Click:Connect(function()
    local target = GetPlayerFromName(TpInput.Text)
    if target and target.Character and target.Character:FindFirstChild("HumanoidRootPart") and LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart") then
        LocalPlayer.Character.HumanoidRootPart.CFrame = target.Character.HumanoidRootPart.CFrame * CFrame.new(0, 2, 0)
    end
end)

-- ABA: Visuais (ESP)
AddToggleOption(VisualsPage, "Boxes", Enum.KeyCode.None, "Boxes", function(state) getgenv().Settings.Boxes = state end)
AddToggleOption(VisualsPage, "Tracers", Enum.KeyCode.None, "Tracers", function(state) getgenv().Settings.Tracers = state end)
AddToggleOption(VisualsPage, "Barra de Vida", Enum.KeyCode.None, "HealthBar", function(state) getgenv().Settings.HealthBar = state end)
AddToggleOption(VisualsPage, "Esqueleto", Enum.KeyCode.None, "Skeleton", function(state) getgenv().Settings.Skeleton = state end)
AddToggleOption(VisualsPage, "Círculo na Cabeça", Enum.KeyCode.None, "HeadCircle", function(state) getgenv().Settings.HeadCircle = state end)
AddToggleOption(VisualsPage, "ESP Studs", Enum.KeyCode.None, "ESPStuds", function(state) getgenv().Settings.ESPStuds = state end)
AddSliderOption(VisualsPage, "Distância Máxima ESP", 50, 1000, 300, "Studs", "MaxDistance", function(val) getgenv().Settings.MaxDistance = val end)

-- ABA: Player (Boost, Otimizador, Anti-Afk, SpinBot)
AddSectionHeader(PlayerPage, "Performance & Optimizer")
local savedMaterials = {}
AddToggleOption(PlayerPage, "Potato PC Mode", Enum.KeyCode.None, "PotatoModeActive", function(state)
    if state then
        pcall(function()
            Lighting.GlobalShadows = false
            Lighting.FogEnd = 999999
            for _, obj in pairs(workspace:GetDescendants()) do
                if obj:IsA("BasePart") and not obj:IsA("Terrain") then
                    savedMaterials[obj] = {Material = obj.Material, Color = obj.Color}
                    obj.Material = Enum.Material.SmoothPlastic
                elseif obj:IsA("Texture") or obj:IsA("Decal") then obj.Transparency = 1
                elseif obj:IsA("PostEffect") or obj:IsA("Atmosphere") or obj:IsA("Sky") then obj.Parent = Lighting end
            end
        end)
    else
        pcall(function()
            Lighting.GlobalShadows = true
            for obj, data in pairs(savedMaterials) do if obj and obj.Parent then obj.Material = data.Material obj.Color = data.Color end end
            for _, obj in pairs(workspace:GetDescendants()) do if obj:IsA("Texture") or obj:IsA("Decal") then obj.Transparency = 0 end end
            savedMaterials = {}
        end)
    end
end)
AddToggleOption(PlayerPage, "Unlock FPS Cap", Enum.KeyCode.None, "FpsUnlockActive")

AddSectionHeader(PlayerPage, "Visual & Lighting Frame")
AddToggleOption(PlayerPage, "Full Bright / No Dark", Enum.KeyCode.None, "FullBrightActive", function(state)
    pcall(function()
        if state then
            Lighting.Ambient = Color3.fromRGB(255, 255, 255)
            Lighting.OutdoorAmbient = Color3.fromRGB(255, 255, 255)
            Lighting.Brightness = 2 Lighting.ClockTime = 14
        else
            Lighting.Ambient = OriginalLighting.Ambient Lighting.OutdoorAmbient = OriginalLighting.OutdoorAmbient
            Lighting.Brightness = OriginalLighting.Brightness Lighting.ClockTime = OriginalLighting.ClockTime
        end
    end)
end)
AddToggleOption(PlayerPage, "No Fog / Clear World", Enum.KeyCode.None, "NoFogActive", function(state)
    pcall(function() Lighting.FogEnd = state and 999999 or OriginalLighting.FogEnd end)
end)

AddSectionHeader(PlayerPage, "Troll Utilities")
AddToggleOption(PlayerPage, "Anti-AFK Disconnect", Enum.KeyCode.None, "AntiAfkActive")
AddToggleOption(PlayerPage, "Spin Bot Speed", Enum.KeyCode.None, "SpinBotActive")
AddSliderOption(PlayerPage, "Spin Speed Value", 10, 200, 50, "sp", "SpinSpeed", function(val) getgenv().Settings.SpinSpeed = val end)

-- ABA: Settings
AddToggleOption(SettingsPage, "Bolinha Flutuante", Enum.KeyCode.None, "MobileBtn", function(state) FloatingBtn.Visible = state end)
AddToggleOption(SettingsPage, "Painel de Binds", Enum.KeyCode.None, "BindPanel", function(state) BindsFrame.Visible = state end)

local settingsFrame = Instance.new("Frame", SettingsPage)
settingsFrame.Size = UDim2.new(1, 0, 0, 36)
settingsFrame.BackgroundTransparency = 1
local settingsLabel = Instance.new("TextLabel", settingsFrame)
settingsLabel.Size = UDim2.new(0.65, 0, 1, 0)
settingsLabel.Position = UDim2.new(0, 5, 0, 0)
settingsLabel.BackgroundTransparency = 1
settingsLabel.Text = "Atalho do Painel"
settingsLabel.TextColor3 = Color3.fromRGB(200, 200, 200)
settingsLabel.TextSize = 12
settingsLabel.Font = Enum.Font.Gotham
settingsLabel.TextXAlignment = Enum.TextXAlignment.Left

local keyButton = Instance.new("TextButton", settingsFrame)
keyButton.Size = UDim2.new(0, 65, 0, 22)
keyButton.Position = UDim2.new(1, -75, 0.5, -11)
keyButton.BackgroundColor3 = Color3.fromRGB(22, 22, 22)
keyButton.Text = OpenKey.Name
keyButton.TextColor3 = Color3.fromRGB(255, 255, 255)
keyButton.TextSize = 11
keyButton.Font = Enum.Font.GothamBold
Instance.new("UICorner", keyButton).CornerRadius = UDim.new(0, 4)

keyButton.MouseButton1Click:Connect(function()
    if ListeningForKey then return end
    ListeningForKey = true
    keyButton.Text = "..."
    local conn
    conn = UserInputService.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.Keyboard then
            OpenKey = input.KeyCode
            keyButton.Text = OpenKey.Name
            ListeningForKey = false
            conn:Disconnect()
        end
    end)
end)

-- ==========================================
-- MOTORES DE FUNCIONALIDADE
-- ==========================================

RunService.Heartbeat:Connect(function()
    local myChar = LocalPlayer.Character
    local myHrd = myChar and myChar:FindFirstChild("HumanoidRootPart")
    if myChar and myHrd then
        if getgenv().Settings.SpinBotActive then
            myHrd.CFrame = myHrd.CFrame * CFrame.Angles(0, math.rad(getgenv().Settings.SpinSpeed), 0)
        end
    end
end)

local VirtualUser = game:GetService("VirtualUser")
LocalPlayer.Idled:Connect(function()
    if getgenv().Settings.AntiAfkActive then
        VirtualUser:Button2Down(Vector2.new(0,0), Camera.CFrame)
        task.wait(1)
        VirtualUser:Button2Up(Vector2.new(0,0), Camera.CFrame)
    end
end)

-- === FUNÇÃO DE ALVO OTIMIZADA (IGNORA MORTOS E GRUDA CORRETAMENTE) ===
local function getNearestTarget()
    local target = nil
    local maxDist = math.huge
    local char = LocalPlayer.Character
    if not char or not char:FindFirstChild("HumanoidRootPart") then return nil end
    
    local mouse = UserInputService:GetMouseLocation()
    
    for _, plr in ipairs(Players:GetPlayers()) do
        if plr ~= LocalPlayer then
            local pChar = plr.Character
            if pChar then
                local humanoid = pChar:FindFirstChildOfClass("Humanoid")
                local head = pChar:FindFirstChild("Head")
                local rootPart = pChar:FindFirstChild("HumanoidRootPart")
                
                -- Checagem rigorosa de vida para ignorar quem morreu, mas permitindo grudar nos vivos
                if humanoid and head and rootPart and humanoid.Health > 0 then
                    if getgenv().Settings.WallCheckEnabled then
                        local rayParams = RaycastParams.new()
                        rayParams.FilterType = Enum.RaycastFilterType.Exclude
                        rayParams.FilterDescendantsInstances = {LocalPlayer.Character, pChar}
                        local ray = workspace:Raycast(Camera.CFrame.Position, (head.Position - Camera.CFrame.Position), rayParams)
                        if ray then continue end
                    end

                    local screenPos, onScreen = Camera:WorldToViewportPoint(head.Position)
                    if onScreen then
                        local dist = (mouse - Vector2.new(screenPos.X, screenPos.Y)).Magnitude
                        if dist <= getgenv().Settings.FovRadius and dist < maxDist then
                            maxDist = dist
                            target = head
                        end
                    end
                end
            end
        end
    end
    return target
end

-- === SILENT AIM ===
local Caster = require(ReplicatedStorage:WaitForChild("ZexisShared"):WaitForChild("Modules"):WaitForChild("Caster"))
if getgenv().ShotHookCaster then
    pcall(function() getgenv().ShotHookCaster:Restore() end)
end

local oldCast = Caster.Cast
Caster.Cast = function(p1, p2, p3, p4, p5, p6)
    if getgenv().Settings.SilentAimActive then
        local targetHead = getNearestTarget()
        if targetHead then
            p6 = targetHead.Position
        end
    end
    return oldCast(p1, p2, p3, p4, p5, p6)
end
getgenv().ShotHookCaster = { Restore = function() Caster.Cast = oldCast end }

-- === AIMBOT (CELULAR / CAMERA LOCK) ===
RunService.RenderStepped:Connect(function()
    local myChar = LocalPlayer.Character
    if not myChar or not myChar:FindFirstChild("Humanoid") or myChar.Humanoid.Health <= 0 then return end
    
    if getgenv().Settings.AimbotActive then
        local targetHead = getNearestTarget()
        if targetHead then
            local step = math.max(getgenv().Settings.AimSmoothing, 1)
            local currentCF = Camera.CFrame
            local targetCF = CFrame.new(currentCF.Position, targetHead.Position)
            Camera.CFrame = currentCF:Lerp(targetCF, 1 / step)
        end
    end
end)

-- === FOV CIRCLE BRANCO ===
local fovCircle = Drawing.new("Circle")
fovCircle.Radius = getgenv().Settings.FovRadius
fovCircle.Thickness = 2
fovCircle.Color = Color3.fromRGB(255, 255, 255)
fovCircle.Filled = false
fovCircle.Visible = false
fovCircle.Transparency = 1
fovCircle.Position = Vector2.new(Camera.ViewportSize.X / 2, Camera.ViewportSize.Y / 2)

RunService.RenderStepped:Connect(function()
    fovCircle.Position = Vector2.new(Camera.ViewportSize.X / 2, Camera.ViewportSize.Y / 2)
    fovCircle.Radius = getgenv().Settings.FovRadius
    fovCircle.Visible = getgenv().Settings.ShowFov
end)

-- ==========================================
-- MOTOR ESP COMPLETO
-- ==========================================
local BONE_PAIRS = {
    {"Head", "UpperTorso"}, {"UpperTorso", "LowerTorso"},
    {"UpperTorso", "LeftUpperArm"}, {"LeftUpperArm", "LeftLowerArm"}, {"LeftLowerArm", "LeftHand"},
    {"UpperTorso", "RightUpperArm"}, {"RightUpperArm", "RightLowerArm"}, {"RightLowerArm", "RightHand"},
    {"LowerTorso", "LeftUpperLeg"}, {"LeftUpperLeg", "LeftLowerLeg"}, {"LeftLowerLeg", "LeftFoot"},
    {"LowerTorso", "RightUpperLeg"}, {"RightUpperLeg", "RightLowerLeg"}, {"RightLowerLeg", "RightFoot"}
}
local R6_BONE_PAIRS = {
    {"Head", "Torso"}, {"Torso", "Left Arm"}, {"Torso", "Right Arm"},
    {"Torso", "Left Leg"}, {"Torso", "Right Leg"}
}

local function getCharacterBounds(character)
    local minX, minY = math.huge, math.huge
    local maxX, maxY = -math.huge, -math.huge
    local onScreenAny = false

    for _, part in ipairs(character:GetChildren()) do
        if part:IsA("BasePart") then
            local cframe, size = part.CFrame, part.Size
            local halfSize = size / 2

            local vertices = {
                cframe * Vector3.new(-halfSize.X, -halfSize.Y, -halfSize.Z),
                cframe * Vector3.new(halfSize.X, -halfSize.Y, -halfSize.Z),
                cframe * Vector3.new(-halfSize.X, halfSize.Y, -halfSize.Z),
                cframe * Vector3.new(halfSize.X, halfSize.Y, -halfSize.Z),
                cframe * Vector3.new(-halfSize.X, -halfSize.Y, halfSize.Z),
                cframe * Vector3.new(halfSize.X, -halfSize.Y, halfSize.Z),
                cframe * Vector3.new(-halfSize.X, halfSize.Y, halfSize.Z),
                cframe * Vector3.new(halfSize.X, halfSize.Y, halfSize.Z)
            }

            for _, vertex in ipairs(vertices) do
                local screenPos, onScreen = Camera:WorldToViewportPoint(vertex)
                if onScreen then
                    onScreenAny = true
                    if screenPos.X < minX then minX = screenPos.X end
                    if screenPos.X > maxX then maxX = screenPos.X end
                    if screenPos.Y < minY then minY = screenPos.Y end
                    if screenPos.Y > maxY then maxY = screenPos.Y end
                end
            end
        end
    end

    return onScreenAny, Vector2.new(minX, minY), Vector2.new(maxX, maxY)
end

local function createESP(player)
    if player == LocalPlayer then return end

    local tracer = Drawing.new("Line")
    local box = Drawing.new("Square")
    local healthBarBg = Drawing.new("Line")
    local healthBar = Drawing.new("Line")
    local distanceText = Drawing.new("Text")
    local headCircle = Drawing.new("Circle")

    tracer.Color = getgenv().Settings.TracerColor; tracer.Thickness = 1
    box.Color = getgenv().Settings.BoxColor; box.Thickness = 1; box.Filled = false
    healthBarBg.Thickness = 2; healthBarBg.Color = getgenv().Settings.HealthBgColor
    healthBar.Thickness = 2
    
    distanceText.Color = getgenv().Settings.TextColor; distanceText.Size = 14; distanceText.Center = true; distanceText.Outline = true
    headCircle.Color = getgenv().Settings.SkeletonColor; headCircle.Thickness = 1; headCircle.Filled = false; headCircle.NumSides = 12

    local skeletonLines = {}
    for i = 1, #BONE_PAIRS do
        local line = Drawing.new("Line")
        line.Color = getgenv().Settings.SkeletonColor; line.Thickness = 1; line.Visible = false
        table.insert(skeletonLines, line)
    end

    local function setAllVisibility(visible)
        tracer.Visible = visible and getgenv().Settings.Tracers
        box.Visible = visible and getgenv().Settings.Boxes
        healthBarBg.Visible = visible and getgenv().Settings.HealthBar
        healthBar.Visible = visible and getgenv().Settings.HealthBar
        distanceText.Visible = visible and getgenv().Settings.ESPStuds
        headCircle.Visible = visible and getgenv().Settings.HeadCircle
        for _, line in ipairs(skeletonLines) do line.Visible = visible and getgenv().Settings.Skeleton end
    end

    local connection
    connection = RunService.RenderStepped:Connect(function()
        local character = player.Character
        local myCharacter = LocalPlayer.Character
        
        if not character or not character:IsDescendantOf(workspace) or not myCharacter then
            setAllVisibility(false)
            if not player:IsDescendantOf(Players) then
                tracer:Remove(); box:Remove(); healthBarBg:Remove(); healthBar:Remove()
                distanceText:Remove(); headCircle:Remove()
                for _, line in ipairs(skeletonLines) do line:Remove() end
                connection:Disconnect()
            end
            return
        end

        local rootPart = character:FindFirstChild("HumanoidRootPart")
        local myRootPart = myCharacter:FindFirstChild("HumanoidRootPart")
        local humanoid = character:FindFirstChildOfClass("Humanoid")
        
        if rootPart and myRootPart and humanoid and humanoid.Health > 0 then
            local distance = (myRootPart.Position - rootPart.Position).Magnitude
            if distance > getgenv().Settings.MaxDistance then
                setAllVisibility(false)
                return
            end

            local isValid, topLeft, bottomRight = getCharacterBounds(character)
            if isValid then
                local boxWidth = bottomRight.X - topLeft.X
                local boxHeight = bottomRight.Y - topLeft.Y

                box.Size = Vector2.new(boxWidth + 4, boxHeight + 4)
                box.Position = Vector2.new(topLeft.X - 2, topLeft.Y - 2)
                box.Visible = getgenv().Settings.Boxes

                tracer.From = Vector2.new(Camera.ViewportSize.X / 2, Camera.ViewportSize.Y)
                tracer.To = Vector2.new(topLeft.X + (boxWidth / 2), bottomRight.Y + 2)
                tracer.Visible = getgenv().Settings.Tracers

                local healthPercentage = humanoid.Health / humanoid.MaxHealth
                local healthBarX = topLeft.X - 6
                
                healthBarBg.From = Vector2.new(healthBarX, bottomRight.Y + 2)
                healthBarBg.To = Vector2.new(healthBarX, topLeft.Y - 2)
                healthBarBg.Visible = getgenv().Settings.HealthBar

                local healthHeight = (boxHeight + 4) * healthPercentage
                healthBar.From = Vector2.new(healthBarX, bottomRight.Y + 2)
                healthBar.To = Vector2.new(healthBarX, (bottomRight.Y + 2) - healthHeight)
                healthBar.Color = getgenv().Settings.HealthLowColor:Lerp(getgenv().Settings.HealthGoodColor, healthPercentage)
                healthBar.Visible = getgenv().Settings.HealthBar

                distanceText.Text = string.format("[%d Studs]", math.floor(distance))
                distanceText.Position = Vector2.new(topLeft.X + (boxWidth / 2), bottomRight.Y + 6)
                distanceText.Visible = getgenv().Settings.ESPStuds

                local isR15 = (humanoid.RigType == Enum.HumanoidRigType.R15)
                local activeBones = isR15 and BONE_PAIRS or R6_BONE_PAIRS

                if getgenv().Settings.Skeleton then
                    for i, bonePair in ipairs(activeBones) do
                        local p1 = character:FindFirstChild(bonePair[1])
                        local p2 = character:FindFirstChild(bonePair[2])
                        local line = skeletonLines[i]

                        if p1 and p2 and line then
                            local pos1, os1 = Camera:WorldToViewportPoint(p1.Position)
                            local pos2, os2 = Camera:WorldToViewportPoint(p2.Position)

                            if os1 and os2 then
                                line.From = Vector2.new(pos1.X, pos1.Y)
                                line.To = Vector2.new(pos2.X, pos2.Y)
                                line.Visible = true
                            else
                                line.Visible = false
                            end
                        elseif line then
                            line.Visible = false
                        end
                    end
                else
                    for _, line in ipairs(skeletonLines) do line.Visible = false end
                end

                local headPart = character:FindFirstChild("Head")
                if headPart and getgenv().Settings.HeadCircle then
                    local headPos, headOnScreen = Camera:WorldToViewportPoint(headPart.Position)
                    if headOnScreen then
                        headCircle.Position = Vector2.new(headPos.X, headPos.Y)
                        headCircle.Radius = math.clamp(boxWidth / 7, 2, 15)
                        headCircle.Visible = true
                    else
                        headCircle.Visible = false
                    end
                else
                    headCircle.Visible = false
                end
            else
                setAllVisibility(false)
            end
        else
            setAllVisibility(false)
        end
    end)
end

for _, player in ipairs(Players:GetPlayers()) do createESP(player) end
Players.PlayerAdded:Connect(createESP)

print("I'M Menu Atualizado: Aimbot e Silent Aim Otimizados com Sucesso!")

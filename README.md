--[[
    Gross Hub UI Library - Versão Profissional
    Inspirada em design moderno com funcionalidades completas
]]

local Library = {}
Library.__index = Library

-- Serviços
local TweenService = game:GetService("TweenService")
local UserInputService = game:GetService("UserInputService")
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")

local Player = Players.LocalPlayer
local Mouse = Player:GetMouse()

-- Detectar plataforma
local IsMobile = UserInputService.TouchEnabled and not UserInputService.KeyboardEnabled

-- Configurações de Tema
local Theme = {
    Primary = Color3.fromRGB(139, 69, 255),
    Secondary = Color3.fromRGB(110, 50, 230),
    Background = Color3.fromRGB(15, 15, 20),
    Surface = Color3.fromRGB(20, 20, 28),
    SurfaceLight = Color3.fromRGB(28, 28, 38),
    Border = Color3.fromRGB(45, 45, 60),
    Text = Color3.fromRGB(255, 255, 255),
    TextDim = Color3.fromRGB(160, 160, 170),
    Success = Color3.fromRGB(87, 242, 135),
    Danger = Color3.fromRGB(237, 66, 69),
    Warning = Color3.fromRGB(255, 193, 7)
}

-- Função auxiliar de Tween
local function Tween(object, properties, duration)
    duration = duration or 0.3
    local tween = TweenService:Create(
        object,
        TweenInfo.new(duration, Enum.EasingStyle.Quad, Enum.EasingDirection.Out),
        properties
    )
    tween:Play()
    return tween
end

-- Criar ScreenGui principal
function Library:CreateWindow(title)
    local Window = {}
    
    -- ScreenGui
    local ScreenGui = Instance.new("ScreenGui")
    ScreenGui.Name = "GrossHubUI"
    ScreenGui.ResetOnSpawn = false
    ScreenGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
    
    if gethui then
        ScreenGui.Parent = gethui()
    elseif syn and syn.protect_gui then
        syn.protect_gui(ScreenGui)
        ScreenGui.Parent = game:GetService("CoreGui")
    else
        ScreenGui.Parent = game:GetService("CoreGui")
    end
    
    -- Frame Principal
    local MainFrame = Instance.new("Frame")
    MainFrame.Name = "MainFrame"
    MainFrame.Size = IsMobile and UDim2.new(0, 320, 0, 420) or UDim2.new(0, 550, 0, 450)
    MainFrame.Position = UDim2.new(0.5, 0, 1.5, 0)
    MainFrame.AnchorPoint = Vector2.new(0.5, 0.5)
    MainFrame.BackgroundColor3 = Theme.Background
    MainFrame.BorderSizePixel = 0
    MainFrame.ClipsDescendants = false
    MainFrame.Parent = ScreenGui
    
    local MainCorner = Instance.new("UICorner")
    MainCorner.CornerRadius = UDim.new(0, 12)
    MainCorner.Parent = MainFrame
    
    local MainStroke = Instance.new("UIStroke")
    MainStroke.Color = Theme.Border
    MainStroke.Thickness = 1
    MainStroke.Parent = MainFrame
    
    -- Sombra
    local Shadow = Instance.new("ImageLabel")
    Shadow.Name = "Shadow"
    Shadow.Size = UDim2.new(1, 40, 1, 40)
    Shadow.Position = UDim2.new(0.5, 0, 0.5, 0)
    Shadow.AnchorPoint = Vector2.new(0.5, 0.5)
    Shadow.BackgroundTransparency = 1
    Shadow.Image = "rbxassetid://6014261993"
    Shadow.ImageColor3 = Color3.fromRGB(0, 0, 0)
    Shadow.ImageTransparency = 0.4
    Shadow.ScaleType = Enum.ScaleType.Slice
    Shadow.SliceCenter = Rect.new(99, 99, 99, 99)
    Shadow.ZIndex = 0
    Shadow.Parent = MainFrame
    
    -- Header
    local Header = Instance.new("Frame")
    Header.Name = "Header"
    Header.Size = UDim2.new(1, 0, 0, 45)
    Header.BackgroundColor3 = Theme.Surface
    Header.BorderSizePixel = 0
    Header.Parent = MainFrame
    
    local HeaderCorner = Instance.new("UICorner")
    HeaderCorner.CornerRadius = UDim.new(0, 12)
    HeaderCorner.Parent = Header
    
    local HeaderFix = Instance.new("Frame")
    HeaderFix.Size = UDim2.new(1, 0, 0, 12)
    HeaderFix.Position = UDim2.new(0, 0, 1, -12)
    HeaderFix.BackgroundColor3 = Theme.Surface
    HeaderFix.BorderSizePixel = 0
    HeaderFix.Parent = Header
    
    -- Logo Circle (Gross)
    local LogoCircle = Instance.new("Frame")
    LogoCircle.Name = "LogoCircle"
    LogoCircle.Size = UDim2.new(0, 32, 0, 32)
    LogoCircle.Position = UDim2.new(0, 10, 0.5, 0)
    LogoCircle.AnchorPoint = Vector2.new(0, 0.5)
    LogoCircle.BackgroundColor3 = Theme.Primary
    LogoCircle.BorderSizePixel = 0
    LogoCircle.Parent = Header
    
    local LogoCorner = Instance.new("UICorner")
    LogoCorner.CornerRadius = UDim.new(1, 0)
    LogoCorner.Parent = LogoCircle
    
    local LogoGradient = Instance.new("UIGradient")
    LogoGradient.Color = ColorSequence.new{
        ColorSequenceKeypoint.new(0, Theme.Primary),
        ColorSequenceKeypoint.new(1, Theme.Secondary)
    }
    LogoGradient.Rotation = 45
    LogoGradient.Parent = LogoCircle
    
    local LogoText = Instance.new("TextLabel")
    LogoText.Size = UDim2.new(1, 0, 1, 0)
    LogoText.BackgroundTransparency = 1
    LogoText.Text = "G"
    LogoText.TextColor3 = Theme.Text
    LogoText.TextSize = 18
    LogoText.Font = Enum.Font.GothamBold
    LogoText.Parent = LogoCircle
    
    -- Título
    local Title = Instance.new("TextLabel")
    Title.Name = "Title"
    Title.Size = UDim2.new(1, -140, 1, 0)
    Title.Position = UDim2.new(0, 50, 0, 0)
    Title.BackgroundTransparency = 1
    Title.Text = title or "GROSS HUB"
    Title.TextColor3 = Theme.Text
    Title.TextSize = 16
    Title.Font = Enum.Font.GothamBold
    Title.TextXAlignment = Enum.TextXAlignment.Left
    Title.Parent = Header
    
    -- Botões Header
    local CloseButton = Instance.new("TextButton")
    CloseButton.Name = "CloseButton"
    CloseButton.Size = UDim2.new(0, 32, 0, 32)
    CloseButton.Position = UDim2.new(1, -38, 0.5, 0)
    CloseButton.AnchorPoint = Vector2.new(0, 0.5)
    CloseButton.BackgroundColor3 = Theme.Danger
    CloseButton.BackgroundTransparency = 0.9
    CloseButton.BorderSizePixel = 0
    CloseButton.Text = "✕"
    CloseButton.TextColor3 = Theme.Danger
    CloseButton.TextSize = 16
    CloseButton.Font = Enum.Font.GothamBold
    CloseButton.Parent = Header
    
    local CloseCorner = Instance.new("UICorner")
    CloseCorner.CornerRadius = UDim.new(0, 8)
    CloseCorner.Parent = CloseButton
    
    -- Content Area
    local ContentFrame = Instance.new("Frame")
    ContentFrame.Name = "ContentFrame"
    ContentFrame.Size = UDim2.new(1, 0, 1, -45)
    ContentFrame.Position = UDim2.new(0, 0, 0, 45)
    ContentFrame.BackgroundTransparency = 1
    ContentFrame.Parent = MainFrame
    
    -- Sidebar
    local Sidebar = Instance.new("Frame")
    Sidebar.Name = "Sidebar"
    Sidebar.Size = UDim2.new(0, 140, 1, -10)
    Sidebar.Position = UDim2.new(0, 5, 0, 5)
    Sidebar.BackgroundColor3 = Theme.Surface
    Sidebar.BorderSizePixel = 0
    Sidebar.Parent = ContentFrame
    
    local SidebarCorner = Instance.new("UICorner")
    SidebarCorner.CornerRadius = UDim.new(0, 10)
    SidebarCorner.Parent = Sidebar
    
    local SidebarStroke = Instance.new("UIStroke")
    SidebarStroke.Color = Theme.Border
    SidebarStroke.Thickness = 1
    SidebarStroke.Parent = Sidebar
    
    local SidebarList = Instance.new("UIListLayout")
    SidebarList.Padding = UDim.new(0, 5)
    SidebarList.SortOrder = Enum.SortOrder.LayoutOrder
    SidebarList.Parent = Sidebar
    
    local SidebarPadding = Instance.new("UIPadding")
    SidebarPadding.PaddingTop = UDim.new(0, 8)
    SidebarPadding.PaddingBottom = UDim.new(0, 8)
    SidebarPadding.PaddingLeft = UDim.new(0, 8)
    SidebarPadding.PaddingRight = UDim.new(0, 8)
    SidebarPadding.Parent = Sidebar
    
    -- Container de Páginas
    local PageContainer = Instance.new("Frame")
    PageContainer.Name = "PageContainer"
    PageContainer.Size = UDim2.new(1, -155, 1, -10)
    PageContainer.Position = UDim2.new(0, 150, 0, 5)
    PageContainer.BackgroundTransparency = 1
    PageContainer.ClipsDescendants = true
    PageContainer.Parent = ContentFrame
    
    -- Sistema de Drag
    local dragging, dragInput, dragStart, startPos
    
    local function update(input)
        local delta = input.Position - dragStart
        Tween(MainFrame, {
            Position = UDim2.new(
                startPos.X.Scale,
                startPos.X.Offset + delta.X,
                startPos.Y.Scale,
                startPos.Y.Offset + delta.Y
            )
        }, 0.1)
    end
    
    Header.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or 
           input.UserInputType == Enum.UserInputType.Touch then
            dragging = true
            dragStart = input.Position
            startPos = MainFrame.Position
            
            input.Changed:Connect(function()
                if input.UserInputState == Enum.UserInputState.End then
                    dragging = false
                end
            end)
        end
    end)
    
    Header.InputChanged:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseMovement or 
           input.UserInputType == Enum.UserInputType.Touch then
            dragInput = input
        end
    end)
    
    UserInputService.InputChanged:Connect(function(input)
        if dragging and input == dragInput then
            update(input)
        end
    end)
    
    -- Fechar
    Window.IsOpen = false
    
    function Window:Toggle()
        Window.IsOpen = not Window.IsOpen
        
        if Window.IsOpen then
            Tween(MainFrame, {Position = UDim2.new(0.5, 0, 0.5, 0)}, 0.4)
        else
            Tween(MainFrame, {Position = UDim2.new(0.5, 0, 1.5, 0)}, 0.4)
        end
    end
    
    CloseButton.MouseEnter:Connect(function()
        Tween(CloseButton, {BackgroundTransparency = 0.7}, 0.2)
    end)
    
    CloseButton.MouseLeave:Connect(function()
        Tween(CloseButton, {BackgroundTransparency = 0.9}, 0.2)
    end)
    
    CloseButton.MouseButton1Click:Connect(function()
        Window:Toggle()
    end)
    
    -- Abrir automaticamente
    task.wait(0.1)
    Window:Toggle()
    
    Window.Tabs = {}
    Window.CurrentTab = nil
    Window.Sidebar = Sidebar
    Window.PageContainer = PageContainer
    
    return Window
end

-- Criar Tab
function Library:AddTab(Window, tabName)
    local Tab = {}
    
    -- Botão da Tab
    local TabButton = Instance.new("TextButton")
    TabButton.Name = tabName
    TabButton.Size = UDim2.new(1, 0, 0, 35)
    TabButton.BackgroundColor3 = Theme.SurfaceLight
    TabButton.BackgroundTransparency = 1
    TabButton.BorderSizePixel = 0
    TabButton.Text = tabName
    TabButton.TextColor3 = Theme.TextDim
    TabButton.TextSize = 13
    TabButton.Font = Enum.Font.Gotham
    TabButton.Parent = Window.Sidebar
    
    local TabCorner = Instance.new("UICorner")
    TabCorner.CornerRadius = UDim.new(0, 8)
    TabCorner.Parent = TabButton
    
    -- Página da Tab
    local TabPage = Instance.new("ScrollingFrame")
    TabPage.Name = tabName .. "Page"
    TabPage.Size = UDim2.new(1, 0, 1, 0)
    TabPage.BackgroundTransparency = 1
    TabPage.BorderSizePixel = 0
    TabPage.ScrollBarThickness = 4
    TabPage.ScrollBarImageColor3 = Theme.Primary
    TabPage.CanvasSize = UDim2.new(0, 0, 0, 0)
    TabPage.Visible = false
    TabPage.Parent = Window.PageContainer
    
    local TabList = Instance.new("UIListLayout")
    TabList.Padding = UDim.new(0, 8)
    TabList.SortOrder = Enum.SortOrder.LayoutOrder
    TabList.Parent = TabPage
    
    local TabPadding = Instance.new("UIPadding")
    TabPadding.PaddingTop = UDim.new(0, 8)
    TabPadding.PaddingLeft = UDim.new(0, 8)
    TabPadding.PaddingRight = UDim.new(0, 8)
    TabPadding.PaddingBottom = UDim.new(0, 8)
    TabPadding.Parent = TabPage
    
    TabList:GetPropertyChangedSignal("AbsoluteContentSize"):Connect(function()
        TabPage.CanvasSize = UDim2.new(0, 0, 0, TabList.AbsoluteContentSize.Y + 16)
    end)
    
    -- Função para selecionar tab
    function Tab:Select()
        for _, tab in pairs(Window.Tabs) do
            tab.Button.BackgroundTransparency = 1
            tab.Button.TextColor3 = Theme.TextDim
            tab.Page.Visible = false
        end
        
        TabButton.BackgroundTransparency = 0
        TabButton.TextColor3 = Theme.Primary
        TabPage.Visible = true
        Window.CurrentTab = Tab
    end
    
    TabButton.MouseButton1Click:Connect(function()
        Tab:Select()
    end)
    
    TabButton.MouseEnter:Connect(function()
        if Window.CurrentTab ~= Tab then
            Tween(TabButton, {BackgroundTransparency = 0.5}, 0.2)
        end
    end)
    
    TabButton.MouseLeave:Connect(function()
        if Window.CurrentTab ~= Tab then
            Tween(TabButton, {BackgroundTransparency = 1}, 0.2)
        end
    end)
    
    Tab.Button = TabButton
    Tab.Page = TabPage
    table.insert(Window.Tabs, Tab)
    
    if #Window.Tabs == 1 then
        Tab:Select()
    end
    
    return Tab
end

-- Adicionar Toggle
function Library:AddToggle(Tab, text, default, callback)
    local Toggle = Instance.new("Frame")
    Toggle.Name = "Toggle"
    Toggle.Size = UDim2.new(1, 0, 0, 40)
    Toggle.BackgroundColor3 = Theme.Surface
    Toggle.BorderSizePixel = 0
    Toggle.Parent = Tab.Page
    
    local ToggleCorner = Instance.new("UICorner")
    ToggleCorner.CornerRadius = UDim.new(0, 8)
    ToggleCorner.Parent = Toggle
    
    local ToggleStroke = Instance.new("UIStroke")
    ToggleStroke.Color = Theme.Border
    ToggleStroke.Thickness = 1
    ToggleStroke.Parent = Toggle
    
    local ToggleLabel = Instance.new("TextLabel")
    ToggleLabel.Size = UDim2.new(1, -60, 1, 0)
    ToggleLabel.Position = UDim2.new(0, 12, 0, 0)
    ToggleLabel.BackgroundTransparency = 1
    ToggleLabel.Text = text
    ToggleLabel.TextColor3 = Theme.Text
    ToggleLabel.TextSize = 13
    ToggleLabel.Font = Enum.Font.Gotham
    ToggleLabel.TextXAlignment = Enum.TextXAlignment.Left
    ToggleLabel.Parent = Toggle
    
    local ToggleButton = Instance.new("TextButton")
    ToggleButton.Size = UDim2.new(0, 44, 0, 24)
    ToggleButton.Position = UDim2.new(1, -52, 0.5, 0)
    ToggleButton.AnchorPoint = Vector2.new(0, 0.5)
    ToggleButton.BackgroundColor3 = Theme.Border
    ToggleButton.BorderSizePixel = 0
    ToggleButton.Text = ""
    ToggleButton.Parent = Toggle
    
    local ToggleButtonCorner = Instance.new("UICorner")
    ToggleButtonCorner.CornerRadius = UDim.new(1, 0)
    ToggleButtonCorner.Parent = ToggleButton
    
    local ToggleCircle = Instance.new("Frame")
    ToggleCircle.Size = UDim2.new(0, 20, 0, 20)
    ToggleCircle.Position = UDim2.new(0, 2, 0.5, 0)
    ToggleCircle.AnchorPoint = Vector2.new(0, 0.5)
    ToggleCircle.BackgroundColor3 = Theme.Text
    ToggleCircle.BorderSizePixel = 0
    ToggleCircle.Parent = ToggleButton
    
    local ToggleCircleCorner = Instance.new("UICorner")
    ToggleCircleCorner.CornerRadius = UDim.new(1, 0)
    ToggleCircleCorner.Parent = ToggleCircle
    
    local toggled = default or false
    
    local function UpdateToggle()
        if toggled then
            Tween(ToggleButton, {BackgroundColor3 = Theme.Primary})
            Tween(ToggleCircle, {Position = UDim2.new(1, -22, 0.5, 0)})
        else
            Tween(ToggleButton, {BackgroundColor3 = Theme.Border})
            Tween(ToggleCircle, {Position = UDim2.new(0, 2, 0.5, 0)})
        end
        
        if callback then
            callback(toggled)
        end
    end
    
    ToggleButton.MouseButton1Click:Connect(function()
        toggled = not toggled
        UpdateToggle()
    end)
    
    UpdateToggle()
    
    return {
        SetValue = function(value)
            toggled = value
            UpdateToggle()
        end
    }
end

-- Adicionar Slider
function Library:AddSlider(Tab, text, min, max, default, callback)
    local Slider = Instance.new("Frame")
    Slider.Name = "Slider"
    Slider.Size = UDim2.new(1, 0, 0, 50)
    Slider.BackgroundColor3 = Theme.Surface
    Slider.BorderSizePixel = 0
    Slider.Parent = Tab.Page
    
    local SliderCorner = Instance.new("UICorner")
    SliderCorner.CornerRadius = UDim.new(0, 8)
    SliderCorner.Parent = Slider
    
    local SliderStroke = Instance.new("UIStroke")
    SliderStroke.Color = Theme.Border
    SliderStroke.Thickness = 1
    SliderStroke.Parent = Slider
    
    local SliderLabel = Instance.new("TextLabel")
    SliderLabel.Size = UDim2.new(1, -24, 0, 20)
    SliderLabel.Position = UDim2.new(0, 12, 0, 5)
    SliderLabel.BackgroundTransparency = 1
    SliderLabel.Text = text
    SliderLabel.TextColor3 = Theme.Text
    SliderLabel.TextSize = 13
    SliderLabel.Font = Enum.Font.Gotham
    SliderLabel.TextXAlignment = Enum.TextXAlignment.Left
    SliderLabel.Parent = Slider
    
    local SliderValue = Instance.new("TextLabel")
    SliderValue.Size = UDim2.new(0, 40, 0, 20)
    SliderValue.Position = UDim2.new(1, -52, 0, 5)
    SliderValue.BackgroundTransparency = 1
    SliderValue.Text = tostring(default or min)
    SliderValue.TextColor3 = Theme.Primary
    SliderValue.TextSize = 12
    SliderValue.Font = Enum.Font.GothamBold
    SliderValue.TextXAlignment = Enum.TextXAlignment.Right
    SliderValue.Parent = Slider
    
    local SliderBar = Instance.new("Frame")
    SliderBar.Size = UDim2.new(1, -24, 0, 6)
    SliderBar.Position = UDim2.new(0, 12, 1, -14)
    SliderBar.BackgroundColor3 = Theme.Border
    SliderBar.BorderSizePixel = 0
    SliderBar.Parent = Slider
    
    local SliderBarCorner = Instance.new("UICorner")
    SliderBarCorner.CornerRadius = UDim.new(1, 0)
    SliderBarCorner.Parent = SliderBar
    
    local SliderFill = Instance.new("Frame")
    SliderFill.Size = UDim2.new(0, 0, 1, 0)
    SliderFill.BackgroundColor3 = Theme.Primary
    SliderFill.BorderSizePixel = 0
    SliderFill.Parent = SliderBar
    
    local SliderFillCorner = Instance.new("UICorner")
    SliderFillCorner.CornerRadius = UDim.new(1, 0)
    SliderFillCorner.Parent = SliderFill
    
    local dragging = false
    local value = default or min
    
    local function UpdateSlider(input)
        local pos = math.clamp((input.Position.X - SliderBar.AbsolutePosition.X) / SliderBar.AbsoluteSize.X, 0, 1)
        value = math.floor(min + (max - min) * pos)
        
        Tween(SliderFill, {Size = UDim2.new(pos, 0, 1, 0)}, 0.1)
        SliderValue.Text = tostring(value)
        
        if callback then
            callback(value)
        end
    end
    
    SliderBar.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or 
           input.UserInputType == Enum.UserInputType.Touch then
            dragging = true
            UpdateSlider(input)
        end
    end)
    
    SliderBar.InputEnded:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or 
           input.UserInputType == Enum.UserInputType.Touch then
            dragging = false
        end
    end)
    
    UserInputService.InputChanged:Connect(function(input)
        if dragging and (input.UserInputType == Enum.UserInputType.MouseMovement or 
           input.UserInputType == Enum.UserInputType.Touch) then
            UpdateSlider(input)
        end
    end)
    
    local initialPos = (value - min) / (max - min)
    SliderFill.Size = UDim2.new(initialPos, 0, 1, 0)
    
    return {
        SetValue = function(newValue)
            value = math.clamp(newValue, min, max)
            local pos = (value - min) / (max - min)
            Tween(SliderFill, {Size = UDim2.new(pos, 0, 1, 0)}, 0.1)
            SliderValue.Text = tostring(value)
        end
    }
end

-- Adicionar Button
function Library:AddButton(Tab, text, callback)
    local Button = Instance.new("TextButton")
    Button.Name = "Button"
    Button.Size = UDim2.new(1, 0, 0, 38)
    Button.BackgroundColor3 = Theme.Primary
    Button.BorderSizePixel = 0
    Button.Text = text
    Button.TextColor3 = Theme.Text
    Button.TextSize = 13
    Button.Font = Enum.Font.GothamBold
    Button.Parent = Tab.Page
    
    local ButtonCorner = Instance.new("UICorner")
    ButtonCorner.CornerRadius = UDim.new(0, 8)
    ButtonCorner.Parent = Button
    
    local ButtonGradient = Instance.new("UIGradient")
    ButtonGradient.Color = ColorSequence.new{
        ColorSequenceKeypoint.new(0, Theme.Primary),
        ColorSequenceKeypoint.new(1, Theme.Secondary)
    }
    ButtonGradient.Rotation = 45
    ButtonGradient.Parent = Button
    
    Button.MouseEnter:Connect(function()
        Tween(Button, {Size = UDim2.new(1, 0, 0, 40)}, 0.2)
    end)
    
    Button.MouseLeave:Connect(function()
        Tween(Button, {Size = UDim2.new(1, 0, 0, 38)}, 0.2)
    end)
    
    Button.MouseButton1Click:Connect(function()
        if callback then
            callback()
        end
    end)
end

-- Adicionar Keybind
function Library:AddKeybind(Tab, text, default, callback)
    local Keybind = Instance.new("Frame")
    Keybind.Name = "Keybind"
    Keybind.Size = UDim2.new(1, 0, 0, 40)
    Keybind.BackgroundColor3 = Theme.Surface
    Keybind.BorderSizePixel = 0
    Keybind.Parent = Tab.Page
    
    local KeybindCorner = Instance.new("UICorner")
    KeybindCorner.CornerRadius = UDim.new(0, 8)
    KeybindCorner.Parent = Keybind
    
    local KeybindStroke = Instance.new("UIStroke")
    KeybindStroke.Color = Theme.Border
    KeybindStroke.Thickness = 1
    KeybindStroke.Parent = Keybind
    
    local KeybindLabel = Instance.new("TextLabel")
    KeybindLabel.Size = UDim2.new(1, -90, 1, 0)
    KeybindLabel.Position = UDim2.new(0, 12, 0, 0)
    KeybindLabel.BackgroundTransparency = 1
    KeybindLabel.Text = text
    KeybindLabel.TextColor3 = Theme.Text
    KeybindLabel.TextSize = 13
    KeybindLabel.Font = Enum.Font.Gotham
    KeybindLabel.TextXAlignment = Enum.TextXAlignment.Left
    KeybindLabel.Parent = Keybind
    
    local KeybindButton = Instance.new("TextButton")
    KeybindButton.Size = UDim2.new(0, 70, 0, 28)
    KeybindButton.Position = UDim2.new(1, -78, 0.5, 0)
    KeybindButton.AnchorPoint = Vector2.new(0, 0.5)
    KeybindButton.BackgroundColor3 = Theme.SurfaceLight
    KeybindButton.BorderSizePixel = 0
    KeybindButton.Text = default and default.Name or "..."
    KeybindButton.TextColor3 = Theme.Text
    KeybindButton.TextSize = 11
    KeybindButton.Font = Enum.Font.GothamBold
    KeybindButton.Parent = Keybind
    
    local KeybindButtonCorner = Instance.new("UICorner")
    KeybindButtonCorner.CornerRadius = UDim.new(0, 6)
    KeybindButtonCorner.Parent = KeybindButton
    
    local currentKey = default
    local binding = false
    
    KeybindButton.MouseButton1Click:Connect(function()
        binding = true
        KeybindButton.Text = "..."
        Tween(KeybindButton, {BackgroundColor3 = Theme.Primary})
    end)
    
    UserInputService.InputBegan:Connect(function(input, gameProcessed)
        if binding and not gameProcessed then
            if input.UserInputType == Enum.UserInputType.Keyboard then
                currentKey = input.KeyCode
                KeybindButton.Text = input.KeyCode.Name
                binding = false
                Tween(KeybindButton, {BackgroundColor3 = Theme.SurfaceLight})
                
                if callback then
                    callback(currentKey)
                end
            end
        end
        
        if not binding and currentKey and input.KeyCode == currentKey then
            if callback then
                callback(currentKey)
            end
        end
    end)
    
    return {
        SetKey = function(key)
            currentKey = key
            KeybindButton.Text = key.Name
        end
    }
end

return Library

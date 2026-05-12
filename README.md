--[[
    Biblioteca de UI Profissional para Roblox
    Recursos: Menu draggable (PC/Mobile), Toggle, Slider, Button, Keybind
    Compatível com tela mobile
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

-- Configurações padrão
local Config = {
    MenuSize = UDim2.new(0, 320, 0, 400),
    MenuSizeMobile = UDim2.new(0, 280, 0, 350),
    CornerRadius = 12,
    AnimationSpeed = 0.3,
    Colors = {
        Background = Color3.fromRGB(25, 25, 30),
        Secondary = Color3.fromRGB(35, 35, 40),
        Accent = Color3.fromRGB(88, 101, 242),
        Text = Color3.fromRGB(255, 255, 255),
        SubText = Color3.fromRGB(180, 180, 190),
        Border = Color3.fromRGB(50, 50, 55),
        Success = Color3.fromRGB(87, 242, 135),
        Close = Color3.fromRGB(237, 66, 69)
    }
}

-- Detectar se é mobile
local IsMobile = UserInputService.TouchEnabled and not UserInputService.KeyboardEnabled

-- Função auxiliar para criar tweens
local function Tween(object, properties, duration)
    duration = duration or Config.AnimationSpeed
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
    ScreenGui.Name = "LibraryUI"
    ScreenGui.ResetOnSpawn = false
    ScreenGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
    ScreenGui.Parent = game.CoreGui
    
    -- Frame principal
    local MainFrame = Instance.new("Frame")
    MainFrame.Name = "MainFrame"
    MainFrame.Size = IsMobile and Config.MenuSizeMobile or Config.MenuSize
    MainFrame.Position = UDim2.new(0.5, 0, 1.5, 0) -- Começa fora da tela
    MainFrame.AnchorPoint = Vector2.new(0.5, 0.5)
    MainFrame.BackgroundColor3 = Config.Colors.Background
    MainFrame.BorderSizePixel = 0
    MainFrame.ClipsDescendants = true
    MainFrame.Parent = ScreenGui
    
    local MainCorner = Instance.new("UICorner")
    MainCorner.CornerRadius = UDim.new(0, Config.CornerRadius)
    MainCorner.Parent = MainFrame
    
    local MainStroke = Instance.new("UIStroke")
    MainStroke.Color = Config.Colors.Border
    MainStroke.Thickness = 1.5
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
    Shadow.ImageTransparency = 0.5
    Shadow.ScaleType = Enum.ScaleType.Slice
    Shadow.SliceCenter = Rect.new(99, 99, 99, 99)
    Shadow.ZIndex = 0
    Shadow.Parent = MainFrame
    
    -- Header
    local Header = Instance.new("Frame")
    Header.Name = "Header"
    Header.Size = UDim2.new(1, 0, 0, 45)
    Header.BackgroundColor3 = Config.Colors.Secondary
    Header.BorderSizePixel = 0
    Header.Parent = MainFrame
    
    local HeaderCorner = Instance.new("UICorner")
    HeaderCorner.CornerRadius = UDim.new(0, Config.CornerRadius)
    HeaderCorner.Parent = Header
    
    -- Fix para arredondar só o topo
    local HeaderFix = Instance.new("Frame")
    HeaderFix.Size = UDim2.new(1, 0, 0, 15)
    HeaderFix.Position = UDim2.new(0, 0, 1, -15)
    HeaderFix.BackgroundColor3 = Config.Colors.Secondary
    HeaderFix.BorderSizePixel = 0
    HeaderFix.Parent = Header
    
    -- Círculo decorativo (Gross)
    local GrossCircle = Instance.new("Frame")
    GrossCircle.Name = "Gross"
    GrossCircle.Size = UDim2.new(0, 28, 0, 28)
    GrossCircle.Position = UDim2.new(0, 12, 0.5, 0)
    GrossCircle.AnchorPoint = Vector2.new(0, 0.5)
    GrossCircle.BackgroundColor3 = Config.Colors.Accent
    GrossCircle.BorderSizePixel = 0
    GrossCircle.Parent = Header
    
    local GrossCorner = Instance.new("UICorner")
    GrossCorner.CornerRadius = UDim.new(1, 0)
    GrossCorner.Parent = GrossCircle
    
    local GrossGradient = Instance.new("UIGradient")
    GrossGradient.Color = ColorSequence.new{
        ColorSequenceKeypoint.new(0, Config.Colors.Accent),
        ColorSequenceKeypoint.new(1, Config.Colors.Success)
    }
    GrossGradient.Rotation = 45
    GrossGradient.Parent = GrossCircle
    
    -- Texto do círculo
    local GrossText = Instance.new("TextLabel")
    GrossText.Size = UDim2.new(1, 0, 1, 0)
    GrossText.BackgroundTransparency = 1
    GrossText.Text = "G"
    GrossText.TextColor3 = Config.Colors.Text
    GrossText.TextSize = 16
    GrossText.Font = Enum.Font.GothamBold
    GrossText.Parent = GrossCircle
    
    -- Título
    local Title = Instance.new("TextLabel")
    Title.Name = "Title"
    Title.Size = UDim2.new(1, -100, 1, 0)
    Title.Position = UDim2.new(0, 48, 0, 0)
    Title.BackgroundTransparency = 1
    Title.Text = title or "Menu"
    Title.TextColor3 = Config.Colors.Text
    Title.TextSize = 16
    Title.Font = Enum.Font.GothamBold
    Title.TextXAlignment = Enum.TextXAlignment.Left
    Title.Parent = Header
    
    -- Botão fechar (X)
    local CloseButton = Instance.new("TextButton")
    CloseButton.Name = "CloseButton"
    CloseButton.Size = UDim2.new(0, 35, 0, 35)
    CloseButton.Position = UDim2.new(1, -40, 0.5, 0)
    CloseButton.AnchorPoint = Vector2.new(0, 0.5)
    CloseButton.BackgroundColor3 = Config.Colors.Close
    CloseButton.BackgroundTransparency = 0.9
    CloseButton.BorderSizePixel = 0
    CloseButton.Text = "✕"
    CloseButton.TextColor3 = Config.Colors.Close
    CloseButton.TextSize = 18
    CloseButton.Font = Enum.Font.GothamBold
    CloseButton.Parent = Header
    
    local CloseCorner = Instance.new("UICorner")
    CloseCorner.CornerRadius = UDim.new(0, 8)
    CloseCorner.Parent = CloseButton
    
    -- Container de elementos
    local Container = Instance.new("ScrollingFrame")
    Container.Name = "Container"
    Container.Size = UDim2.new(1, -20, 1, -60)
    Container.Position = UDim2.new(0, 10, 0, 52)
    Container.BackgroundTransparency = 1
    Container.BorderSizePixel = 0
    Container.ScrollBarThickness = 4
    Container.ScrollBarImageColor3 = Config.Colors.Accent
    Container.CanvasSize = UDim2.new(0, 0, 0, 0)
    Container.Parent = MainFrame
    
    local ContainerLayout = Instance.new("UIListLayout")
    ContainerLayout.SortOrder = Enum.SortOrder.LayoutOrder
    ContainerLayout.Padding = UDim.new(0, 8)
    ContainerLayout.Parent = Container
    
    -- Atualizar canvas size automaticamente
    ContainerLayout:GetPropertyChangedSignal("AbsoluteContentSize"):Connect(function()
        Container.CanvasSize = UDim2.new(0, 0, 0, ContainerLayout.AbsoluteContentSize.Y + 10)
    end)
    
    -- Sistema de Drag (PC e Mobile)
    local dragging = false
    local dragInput, dragStart, startPos
    
    local function update(input)
        local delta = input.Position - dragStart
        Tween(MainFrame, {
            Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
        }, 0.1)
    end
    
    Header.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
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
        if input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch then
            dragInput = input
        end
    end)
    
    UserInputService.InputChanged:Connect(function(input)
        if dragging and input == dragInput then
            update(input)
        end
    end)
    
    -- Animação de entrada
    Window.IsOpen = false
    
    function Window:Toggle()
        Window.IsOpen = not Window.IsOpen
        
        if Window.IsOpen then
            Tween(MainFrame, {Position = UDim2.new(0.5, 0, 0.5, 0)})
        else
            Tween(MainFrame, {Position = UDim2.new(0.5, 0, 1.5, 0)})
        end
    end
    
    -- Botão fechar
    CloseButton.MouseEnter:Connect(function()
        Tween(CloseButton, {BackgroundTransparency = 0.7})
    end)
    
    CloseButton.MouseLeave:Connect(function()
        Tween(CloseButton, {BackgroundTransparency = 0.9})
    end)
    
    CloseButton.MouseButton1Click:Connect(function()
        Window:Toggle()
    end)
    
    -- Abrir menu automaticamente
    task.wait(0.1)
    Window:Toggle()
    
    -- Elementos
    function Window:AddToggle(text, default, callback)
        local Toggle = Instance.new("Frame")
        Toggle.Name = "Toggle"
        Toggle.Size = UDim2.new(1, 0, 0, 40)
        Toggle.BackgroundColor3 = Config.Colors.Secondary
        Toggle.BorderSizePixel = 0
        Toggle.Parent = Container
        
        local ToggleCorner = Instance.new("UICorner")
        ToggleCorner.CornerRadius = UDim.new(0, 8)
        ToggleCorner.Parent = Toggle
        
        local ToggleLabel = Instance.new("TextLabel")
        ToggleLabel.Size = UDim2.new(1, -55, 1, 0)
        ToggleLabel.Position = UDim2.new(0, 12, 0, 0)
        ToggleLabel.BackgroundTransparency = 1
        ToggleLabel.Text = text
        ToggleLabel.TextColor3 = Config.Colors.Text
        ToggleLabel.TextSize = 14
        ToggleLabel.Font = Enum.Font.Gotham
        ToggleLabel.TextXAlignment = Enum.TextXAlignment.Left
        ToggleLabel.Parent = Toggle
        
        local ToggleButton = Instance.new("TextButton")
        ToggleButton.Size = UDim2.new(0, 40, 0, 22)
        ToggleButton.Position = UDim2.new(1, -48, 0.5, 0)
        ToggleButton.AnchorPoint = Vector2.new(0, 0.5)
        ToggleButton.BackgroundColor3 = Config.Colors.Border
        ToggleButton.BorderSizePixel = 0
        ToggleButton.Text = ""
        ToggleButton.Parent = Toggle
        
        local ToggleButtonCorner = Instance.new("UICorner")
        ToggleButtonCorner.CornerRadius = UDim.new(1, 0)
        ToggleButtonCorner.Parent = ToggleButton
        
        local ToggleCircle = Instance.new("Frame")
        ToggleCircle.Size = UDim2.new(0, 18, 0, 18)
        ToggleCircle.Position = UDim2.new(0, 2, 0.5, 0)
        ToggleCircle.AnchorPoint = Vector2.new(0, 0.5)
        ToggleCircle.BackgroundColor3 = Config.Colors.Text
        ToggleCircle.BorderSizePixel = 0
        ToggleCircle.Parent = ToggleButton
        
        local ToggleCircleCorner = Instance.new("UICorner")
        ToggleCircleCorner.CornerRadius = UDim.new(1, 0)
        ToggleCircleCorner.Parent = ToggleCircle
        
        local toggled = default or false
        
        local function UpdateToggle()
            if toggled then
                Tween(ToggleButton, {BackgroundColor3 = Config.Colors.Accent})
                Tween(ToggleCircle, {Position = UDim2.new(1, -20, 0.5, 0)})
            else
                Tween(ToggleButton, {BackgroundColor3 = Config.Colors.Border})
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
    
    function Window:AddSlider(text, min, max, default, callback)
        local Slider = Instance.new("Frame")
        Slider.Name = "Slider"
        Slider.Size = UDim2.new(1, 0, 0, 50)
        Slider.BackgroundColor3 = Config.Colors.Secondary
        Slider.BorderSizePixel = 0
        Slider.Parent = Container
        
        local SliderCorner = Instance.new("UICorner")
        SliderCorner.CornerRadius = UDim.new(0, 8)
        SliderCorner.Parent = Slider
        
        local SliderLabel = Instance.new("TextLabel")
        SliderLabel.Size = UDim2.new(1, -24, 0, 20)
        SliderLabel.Position = UDim2.new(0, 12, 0, 5)
        SliderLabel.BackgroundTransparency = 1
        SliderLabel.Text = text
        SliderLabel.TextColor3 = Config.Colors.Text
        SliderLabel.TextSize = 14
        SliderLabel.Font = Enum.Font.Gotham
        SliderLabel.TextXAlignment = Enum.TextXAlignment.Left
        SliderLabel.Parent = Slider
        
        local SliderValue = Instance.new("TextLabel")
        SliderValue.Size = UDim2.new(0, 40, 0, 20)
        SliderValue.Position = UDim2.new(1, -52, 0, 5)
        SliderValue.BackgroundTransparency = 1
        SliderValue.Text = tostring(default or min)
        SliderValue.TextColor3 = Config.Colors.Accent
        SliderValue.TextSize = 13
        SliderValue.Font = Enum.Font.GothamBold
        SliderValue.TextXAlignment = Enum.TextXAlignment.Right
        SliderValue.Parent = Slider
        
        local SliderBar = Instance.new("Frame")
        SliderBar.Size = UDim2.new(1, -24, 0, 6)
        SliderBar.Position = UDim2.new(0, 12, 1, -14)
        SliderBar.BackgroundColor3 = Config.Colors.Border
        SliderBar.BorderSizePixel = 0
        SliderBar.Parent = Slider
        
        local SliderBarCorner = Instance.new("UICorner")
        SliderBarCorner.CornerRadius = UDim.new(1, 0)
        SliderBarCorner.Parent = SliderBar
        
        local SliderFill = Instance.new("Frame")
        SliderFill.Size = UDim2.new(0, 0, 1, 0)
        SliderFill.BackgroundColor3 = Config.Colors.Accent
        SliderFill.BorderSizePixel = 0
        SliderFill.Parent = SliderBar
        
        local SliderFillCorner = Instance.new("UICorner")
        SliderFillCorner.CornerRadius = UDim.new(1, 0)
        SliderFillCorner.Parent = SliderFill
        
        local SliderDot = Instance.new("Frame")
        SliderDot.Size = UDim2.new(0, 14, 0, 14)
        SliderDot.Position = UDim2.new(0, 0, 0.5, 0)
        SliderDot.AnchorPoint = Vector2.new(0.5, 0.5)
        SliderDot.BackgroundColor3 = Config.Colors.Text
        SliderDot.BorderSizePixel = 0
        SliderDot.Parent = SliderFill
        
        local SliderDotCorner = Instance.new("UICorner")
        SliderDotCorner.CornerRadius = UDim.new(1, 0)
        SliderDotCorner.Parent = SliderDot
        
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
            if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
                dragging = true
                UpdateSlider(input)
            end
        end)
        
        SliderBar.InputEnded:Connect(function(input)
            if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
                dragging = false
            end
        end)
        
        UserInputService.InputChanged:Connect(function(input)
            if dragging and (input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch) then
                UpdateSlider(input)
            end
        end)
        
        -- Setar valor inicial
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
    
    function Window:AddButton(text, callback)
        local Button = Instance.new("TextButton")
        Button.Name = "Button"
        Button.Size = UDim2.new(1, 0, 0, 38)
        Button.BackgroundColor3 = Config.Colors.Accent
        Button.BorderSizePixel = 0
        Button.Text = text
        Button.TextColor3 = Config.Colors.Text
        Button.TextSize = 14
        Button.Font = Enum.Font.GothamBold
        Button.Parent = Container
        
        local ButtonCorner = Instance.new("UICorner")
        ButtonCorner.CornerRadius = UDim.new(0, 8)
        ButtonCorner.Parent = Button
        
        Button.MouseEnter:Connect(function()
            Tween(Button, {BackgroundColor3 = Color3.fromRGB(
                Config.Colors.Accent.R * 255 + 20,
                Config.Colors.Accent.G * 255 + 20,
                Config.Colors.Accent.B * 255 + 20
            )})
        end)
        
        Button.MouseLeave:Connect(function()
            Tween(Button, {BackgroundColor3 = Config.Colors.Accent})
        end)
        
        Button.MouseButton1Click:Connect(function()
            if callback then
                callback()
            end
        end)
    end
    
    function Window:AddKeybind(text, default, callback)
        local Keybind = Instance.new("Frame")
        Keybind.Name = "Keybind"
        Keybind.Size = UDim2.new(1, 0, 0, 40)
        Keybind.BackgroundColor3 = Config.Colors.Secondary
        Keybind.BorderSizePixel = 0
        Keybind.Parent = Container
        
        local KeybindCorner = Instance.new("UICorner")
        KeybindCorner.CornerRadius = UDim.new(0, 8)
        KeybindCorner.Parent = Keybind
        
        local KeybindLabel = Instance.new("TextLabel")
        KeybindLabel.Size = UDim2.new(1, -80, 1, 0)
        KeybindLabel.Position = UDim2.new(0, 12, 0, 0)
        KeybindLabel.BackgroundTransparency = 1
        KeybindLabel.Text = text
        KeybindLabel.TextColor3 = Config.Colors.Text
        KeybindLabel.TextSize = 14
        KeybindLabel.Font = Enum.Font.Gotham
        KeybindLabel.TextXAlignment = Enum.TextXAlignment.Left
        KeybindLabel.Parent = Keybind
        
        local KeybindButton = Instance.new("TextButton")
        KeybindButton.Size = UDim2.new(0, 65, 0, 28)
        KeybindButton.Position = UDim2.new(1, -72, 0.5, 0)
        KeybindButton.AnchorPoint = Vector2.new(0, 0.5)
        KeybindButton.BackgroundColor3 = Config.Colors.Border
        KeybindButton.BorderSizePixel = 0
        KeybindButton.Text = default and default.Name or "..."
        KeybindButton.TextColor3 = Config.Colors.Text
        KeybindButton.TextSize = 12
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
            Tween(KeybindButton, {BackgroundColor3 = Config.Colors.Accent})
        end)
        
        UserInputService.InputBegan:Connect(function(input, gameProcessed)
            if binding and not gameProcessed then
                if input.UserInputType == Enum.UserInputType.Keyboard then
                    currentKey = input.KeyCode
                    KeybindButton.Text = input.KeyCode.Name
                    binding = false
                    Tween(KeybindButton, {BackgroundColor3 = Config.Colors.Border})
                    
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
    
    return Window
end

return Library

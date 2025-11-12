-- AetherUI - A UI Library Fluida, Moderna e Animada
-- Feita para desenvolvedores de jogos Roblox
-- Totalmente segura e dentro dos ToS

local AetherUI = {}
AetherUI.__index = AetherUI

local TweenService = game:GetService("TweenService")
local UserInputService = game:GetService("UserInputService")
local RunService = game:GetService("RunService")
local Players = game:GetService("Players")
local player = Players.LocalPlayer
local mouse = player:GetMouse()

-- Configurações globais
local Config = {
    AccentColor = Color3.fromRGB(0, 170, 255),
    Background = Color3.fromRGB(25, 25, 30),
    Secondary = Color3.fromRGB(35, 35, 40),
    Text = Color3.fromRGB(245, 245, 245),
    CornerRadius = UDim.new(0, 8),
    AnimationTime = 0.25
}

-- Função de animação suave
local function Tween(instance, properties, duration, easing)
    duration = duration or Config.AnimationTime
    easing = easing or Enum.EasingStyle.Quart
    local tween = TweenService:Create(instance, TweenInfo.new(duration, easing, Enum.EasingDirection.Out), properties)
    tween:Play()
    return tween
end

-- Notificação
function AetherUI:Notify(title, text, duration)
    duration = duration or 3
    local notif = Instance.new("Frame")
    local corner = Instance.new("UICorner")
    local titleLabel = Instance.new("TextLabel")
    local textLabel = Instance.new("TextLabel")
    local stroke = Instance.new("UIStroke")

    notif.Size = UDim2.new(0, 300, 0, 80)
    notif.Position = UDim2.new(1, -320, 1, -100)
    notif.BackgroundColor3 = Config.Background
    notif.BorderSizePixel = 0
    notif.Parent = player.PlayerGui

    corner.CornerRadius = Config.CornerRadius
    corner.Parent = notif

    stroke.Color = Config.AccentColor
    stroke.Thickness = 2
    stroke.Parent = notif

    titleLabel.Size = UDim2.new(1, -20, 0, 25)
    titleLabel.Position = UDim2.new(0, 10, 0, 8)
    titleLabel.BackgroundTransparency = 1
    titleLabel.Text = title
    titleLabel.TextColor3 = Config.AccentColor
    titleLabel.Font = Enum.Font.GothamBold
    titleLabel.TextSize = 16
    titleLabel.TextXAlignment = Enum.TextXAlignment.Left
    titleLabel.Parent = notif

    textLabel.Size = UDim2.new(1, -20, 0, 30)
    textLabel.Position = UDim2.new(0, 10, 0, 35)
    textLabel.BackgroundTransparency = 1
    textLabel.Text = text
    textLabel.TextColor3 = Config.Text
    textLabel.Font = Enum.Font.Gotham
    textLabel.TextSize = 14
    textLabel.TextXAlignment = Enum.TextXAlignment.Left
    textLabel.TextWrapped = true
    textLabel.Parent = notif

    -- Animação de entrada
    Tween(notif, {Position = UDim2.new(1, -320, 1, -100)}, 0.4, Enum.EasingStyle.Back)

    task.delay(duration, function()
        Tween(notif, {Position = UDim2.new(1, 20, 1, -100)}, 0.4, Enum.EasingStyle.Back):Completed:Connect(function()
            notif:Destroy()
        end)
    end)
end

-- Criar janela principal
function AetherUI:Window(options)
    options = options or {}
    local title = options.Title or "AetherUI"
    local size = options.Size or UDim2.new(0, 500, 0, 600)

    local screenGui = Instance.new("ScreenGui")
    screenGui.Parent = player.PlayerGui
    screenGui.Name = "AetherUI_" .. tick()

    local main = Instance.new("Frame")
    local corner = Instance.new("UICorner")
    local titleBar = Instance.new("Frame")
    local titleLabel = Instance.new("TextLabel")
    local closeBtn = Instance.new("TextButton")
    local container = Instance.new("Frame")
    local listLayout = Instance.new("UIListLayout")

    main.Size = size
    main.Position = UDim2.new(0.5, -size.X.Offset/2, 0.5, -size.Y.Offset/2)
    main.BackgroundColor3 = Config.Background
    main.BorderSizePixel = 0
    main.ClipsDescendants = true
    main.Parent = screenGui

    corner.CornerRadius = Config.CornerRadius
    corner.Parent = main

    titleBar.Size = UDim2.new(1, 0, 0, 40)
    titleBar.BackgroundColor3 = Config.Secondary
    titleBar.BorderSizePixel = 0
    titleBar.Parent = main

    titleLabel.Size = UDim2.new(1, -50, 1, 0)
    titleLabel.Position = UDim2.new(0, 15, 0, 0)
    titleLabel.BackgroundTransparency = 1
    titleLabel.Text = title
    titleLabel.TextColor3 = Config.Text
    titleLabel.Font = Enum.Font.GothamBold
    titleLabel.TextSize = 16
    titleLabel.TextXAlignment = Enum.TextXAlignment.Left
    titleLabel.Parent = titleBar

    closeBtn.Size = UDim2.new(0, 30, 0, 30)
    closeBtn.Position = UDim2.new(1, -35, 0, 5)
    closeBtn.BackgroundTransparency = 1
    closeBtn.Text = "×"
    closeBtn.TextColor3 = Color3.fromRGB(255, 100, 100)
    closeBtn.Font = Enum.Font.GothamBold
    closeBtn.TextSize = 20
    closeBtn.Parent = titleBar

    container.Size = UDim2.new(1, 0, 1, -40)
    container.Position = UDim2.new(0, 0, 0, 40)
    container.BackgroundTransparency = 1
    container.Parent = main

    listLayout.Padding = UDim.new(0, 8)
    listLayout.FillDirection = Enum.FillDirection.Vertical
    listLayout.HorizontalAlignment = Enum.HorizontalAlignment.Center
    listLayout.Parent = container

    -- Dragging
    local dragging = false
    local dragStart, startPos

    titleBar.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 then
            dragging = true
            dragStart = input.Position
            startPos = main.Position
        end
    end)

    titleBar.InputEnded:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 then
            dragging = false
        end
    end)

    UserInputService.InputChanged:Connect(function(input)
        if dragging and input.UserInputType == Enum.UserInputType.MouseMovement then
            local delta = input.Position - dragStart
            main.Position = UDim2.new(
                startPos.X.Scale, startPos.X.Offset + delta.X,
                startPos.Y.Scale, startPos.Y.Offset + delta.Y
            )
        end
    end)

    closeBtn.MouseButton1Click:Connect(function()
        Tween(main, {Size = UDim2.new(0, 0, 0, 0), Position = UDim2.new(0.5, 0, 0.5, 0)}, 0.3):Completed:Connect(function()
            screenGui:Destroy()
        end)
    end)

    local window = {ScreenGui = screenGui, Container = container}

    -- Tab System
    local tabs = {}
    local tabButtons = {}

    function window:Tab(name)
        local tabFrame = Instance.new("ScrollingFrame")
        local layout = Instance.new("UIListLayout")
        local padding = Instance.new("UIPadding")

        tabFrame.Size = UDim2.new(1, 0, 1, 0)
        tabFrame.BackgroundTransparency = 1
        tabFrame.ScrollBarThickness = 4
        tabFrame.Visible = false
        tabFrame.Parent = container

        layout.Padding = UDim.new(0, 6)
        layout.Parent = tabFrame

        padding.PaddingLeft = UDim.new(0, 12)
        padding.PaddingRight = UDim.new(0, 12)
        padding.PaddingTop = UDim.new(0, 8)
        padding.Parent = tabFrame

        local tabBtn = Instance.new("TextButton")
        tabBtn.Size = UDim2.new(0, 100, 0, 30)
        tabBtn.BackgroundColor3 = Config.Secondary
        tabBtn.Text = name
        tabBtn.TextColor3 = Config.Text
        tabBtn.Font = Enum.Font.Gotham
        tabBtn.TextSize = 14
        tabBtn.AutoButtonColor = false
        tabBtn.Parent = titleBar

        local btnCorner = Instance.new("UICorner")
        btnCorner.CornerRadius = UDim.new(0, 6)
        btnCorner.Parent = tabBtn

        local indicator = Instance.new("Frame")
        indicator.Size = UDim2.new(1, 0, 0, 2)
        indicator.Position = UDim2.new(0, 0, 1, -2)
        indicator.BackgroundColor3 = Config.AccentColor
        indicator.Parent = tabBtn
        indicator.Visible = false

        local tab = {
            Frame = tabFrame,
            Button = tabBtn,
            Indicator = indicator
        }

        table.insert(tabs, tab)
        table.insert(tabButtons, tabBtn)

        -- Layout dos botões
        for i, btn in ipairs(tabButtons) do
            btn.Position = UDim2.new(0, 15 + (i-1) * 110, 0, 5)
        end

        tabBtn.MouseButton1Click:Connect(function()
            for _, t in ipairs(tabs) do
                t.Frame.Visible = false
                Tween(t.Indicator, {Size = UDim2.new(0, 0, 0, 2)}, 0.2)
                t.Indicator.Visible = false
            end
            tab.Frame.Visible = true
            tab.Indicator.Visible = true
            Tween(tab.Indicator, {Size = UDim2.new(1, 0, 0, 2)}, 0.3)
            Tween(tabBtn, {BackgroundColor3 = Color3.fromRGB(45, 45, 50)}, 0.2)
            task.delay(0.2, function()
                Tween(tabBtn, {BackgroundColor3 = Config.Secondary}, 0.2)
            end)
        end)

        if #tabs == 1 then
            tabBtn.MouseButton1Click:Fire()
        end

        local elements = {}

        -- Toggle
        function tab:Toggle(text, callback, default)
            local toggle = Instance.new("Frame")
            local label = Instance.new("TextLabel")
            local box = Instance.new("Frame")
            local check = Instance.new("Frame")
            local corner1 = Instance.new("UICorner")
            local corner2 = Instance.new("UICorner")

            toggle.Size = UDim2.new(1, 0, 0, 36)
            toggle.BackgroundColor3 = Config.Secondary
            toggle.Parent = tabFrame

            corner1.CornerRadius = Config.CornerRadius
            corner1.Parent = toggle

            label.Size = UDim2.new(1, -60, 1, 0)
            label.Position = UDim2.new(0, 12, 0, 0)
            label.BackgroundTransparency = 1
            label.Text = text
            label.TextColor3 = Config.Text
            label.Font = Enum.Font.Gotham
            label.TextSize = 14
            label.TextXAlignment = Enum.TextXAlignment.Left
            label.Parent = toggle

            box.Size = UDim2.new(0, 40, 0, 20)
            box.Position = UDim2.new(1, -52, 0.5, -10)
            box.BackgroundColor3 = default and Config.AccentColor or Color3.fromRGB(60, 60, 70)
            box.Parent = toggle

            corner2.CornerRadius = UDim.new(0, 10)
            corner2.Parent = box

            check.Size = UDim2.new(0, 16, 0, 16)
            check.Position = default and UDim2.new(0, 20, 0.5, -8) or UDim2.new(0, 4, 0.5, -8)
            check.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
            check.Parent = box

            local checkCorner = Instance.new("UICorner")
            checkCorner.CornerRadius = UDim.new(0, 8)
            checkCorner.Parent = check

            local enabled = default or false
            toggle.MouseButton1Click:Connect(function()
                enabled = not enabled
                Tween(box, {BackgroundColor3 = enabled and Config.AccentColor or Color3.fromRGB(60, 60, 70)}, 0.2)
                Tween(check, {Position = enabled and UDim2.new(0, 20, 0.5, -8) or UDim2.new(0, 4, 0.5, -8)}, 0.2)
                if callback then callback(enabled) end
            end)

            elements[#elements+1] = toggle
            return toggle
        end

        -- Button
        function tab:Button(text, callback)
            local btn = Instance.new("TextButton")
            local corner = Instance.new("UICorner")

            btn.Size = UDim2.new(1, 0, 0, 36)
            btn.BackgroundColor3 = Config.AccentColor
            btn.Text = text
            btn.TextColor3 = Color3.fromRGB(255, 255, 255)
            btn.Font = Enum.Font.GothamBold
            btn.TextSize = 14
            btn.AutoButtonColor = false
            btn.Parent = tabFrame

            corner.CornerRadius = Config.CornerRadius
            corner.Parent = btn

            btn.MouseButton1Click:Connect(function()
                Tween(btn, {Size = UDim2.new(0.95, 0, 0, 36)}, 0.1):Completed:Connect(function()
                    Tween(btn, {Size = UDim2.new(1, 0, 0, 36)}, 0.1)
                end)
                if callback then callback() end
            end)

            btn.MouseEnter:Connect(function()
                Tween(btn, {BackgroundColor3 = Color3.fromRGB(0, 150, 230)}, 0.2)
            end)

            btn.MouseLeave:Connect(function()
                Tween(btn, {BackgroundColor3 = Config.AccentColor}, 0.2)
            end)

            return btn
        end

        -- Slider
        function tab:Slider(text, min, max, callback, default)
            local slider = Instance.new("Frame")
            local label = Instance.new("TextLabel")
            local valueLabel = Instance.new("TextLabel")
            local bar = Instance.new("Frame")
            local fill = Instance.new("Frame")
            local knob = Instance.new("Frame")

            slider.Size = UDim2.new(1, 0, 0, 50)
            slider.BackgroundColor3 = Config.Secondary
            slider.Parent = tabFrame

            local corner = Instance.new("UICorner")
            corner.CornerRadius = Config.CornerRadius
            corner.Parent = slider

            label.Size = UDim2.new(0.6, 0, 0, 20)
            label.Position = UDim2.new(0, 12, 0, 8)
            label.BackgroundTransparency = 1
            label.Text = text
            label.TextColor3 = Config.Text
            label.Font = Enum.Font.Gotham
            label.TextSize = 14
            label.TextXAlignment = Enum.TextXAlignment.Left
            label.Parent = slider

            valueLabel.Size = UDim2.new(0.3, 0, 0, 20)
            valueLabel.Position = UDim2.new(0.7, -12, 0, 8)
            valueLabel.BackgroundTransparency = 1
            valueLabel.Text = tostring(default or min)
            valueLabel.TextColor3 = Config.AccentColor
            valueLabel.Font = Enum.Font.GothamBold
            valueLabel.TextSize = 14
            valueLabel.TextXAlignment = Enum.TextXAlignment.Right
            valueLabel.Parent = slider

            bar.Size = UDim2.new(1, -24, 0, 6)
            bar.Position = UDim2.new(0, 12, 0, 32)
            bar.BackgroundColor3 = Color3.fromRGB(50, 50, 60)
            bar.Parent = slider

            local barCorner = Instance.new("UICorner")
            barCorner.CornerRadius = UDim.new(0, 3)
            barCorner.Parent = bar

            fill.Size = UDim2.new((default or min - min) / (max - min), 0, 1, 0)
            fill.BackgroundColor3 = Config.AccentColor
            fill.Parent = bar

            local fillCorner = Instance.new("UICorner")
            fillCorner.CornerRadius = UDim.new(0, 3)
            fillCorner.Parent = fill

            knob.Size = UDim2.new(0, 16, 0, 16)
            knob.Position = UDim2.new(fill.Size.X.Scale - 0.5, -8, 0.5, -8)
            knob.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
            knob.Parent = bar

            local knobCorner = Instance.new("UICorner")
            knobCorner.CornerRadius = UDim.new(1, 0)
            knobCorner.Parent = knob

            local dragging = false
            bar.InputBegan:Connect(function(input)
                if input.UserInputType == Enum.UserInputType.MouseButton1 then
                    dragging = true
                end
            end)

            bar.InputEnded:Connect(function(input)
                if input.UserInputType == Enum.UserInputType.MouseButton1 then
                    dragging = false
                end
            end)

            RunService.RenderStepped:Connect(function()
                if dragging then
                    local mouseX = mouse.X - bar.AbsolutePosition.X
                    local percent = math.clamp(mouseX / bar.AbsoluteSize.X, 0, 1)
                    local value = math.floor(min + (max - min) * percent)
                    fill.Size = UDim2.new(percent, 0, 1, 0)
                    knob.Position = UDim2.new(percent, -8, 0.5, -8)
                    valueLabel.Text = tostring(value)
                    if callback then callback(value) end
                end
            end)

            return slider
        end

        -- Color Picker
        function tab:ColorPicker(text, callback, default)
            local picker = Instance.new("Frame")
            local label = Instance.new("TextLabel")
            local colorBtn = Instance.new("TextButton")
            local hue = Instance.new("ImageLabel")
            local saturation = Instance.new("ImageLabel")
            local pickerKnob = Instance.new("Frame")

            picker.Size = UDim2.new(1, 0, 0, 140)
            picker.BackgroundColor3 = Config.Secondary
            picker.Parent = tabFrame

            local corner = Instance.new("UICorner")
            corner.CornerRadius = Config.CornerRadius
            corner.Parent = picker

            label.Size = UDim2.new(1, -100, 0, 30)
            label.Position = UDim2.new(0, 12, 0, 8)
            label.BackgroundTransparency = 1
            label.Text = text
            label.TextColor3 = Config.Text
            label.Font = Enum.Font.Gotham
            label.TextSize = 14
            label.TextXAlignment = Enum.TextXAlignment.Left
            label.Parent = picker

            colorBtn.Size = UDim2.new(0, 60, 0, 24)
            colorBtn.Position = UDim2.new(1, -72, 0, 8)
            colorBtn.BackgroundColor3 = default or Config.AccentColor
            colorBtn.Text = ""
            colorBtn.Parent = picker

            local btnCorner = Instance.new("UICorner")
            btnCorner.CornerRadius = UDim.new(0, 6)
            btnCorner.Parent = colorBtn

            -- Criar UI de picker (simplificado)
            -- Em produção, use imagens ou ImageLabels com gradientes
            -- Aqui usamos cores sólidas para exemplo

            local open = false
            colorBtn.MouseButton1Click:Connect(function()
                open = not open
                -- Implementar picker completo aqui
                AetherUI:Notify("Color Picker", "Em desenvolvimento!", 2)
            end)

            return picker
        end

        -- Textbox
        function tab:Textbox(text, placeholder, callback)
            local box = Instance.new("Frame")
            local label = Instance.new("TextLabel")
            local input = Instance.new("TextBox")

            box.Size = UDim2.new(1, 0, 0, 36)
            box.BackgroundColor3 = Config.Secondary
            box.Parent = tabFrame

            local corner = Instance.new("UICorner")
            corner.CornerRadius = Config.CornerRadius
            corner.Parent = box

            label.Size = UDim2.new(0.4, 0, 1, 0)
            label.Position = UDim2.new(0, 12, 0, 0)
            label.BackgroundTransparency = 1
            label.Text = text
            label.TextColor3 = Config.Text
            label.Font = Enum.Font.Gotham
            label.TextSize = 14
            label.TextXAlignment = Enum.TextXAlignment.Left
            label.Parent = box

            input.Size = UDim2.new(0.55, 0, 0, 24)
            input.Position = UDim2.new(0.45, 0, 0.5, -12)
            input.BackgroundColor3 = Color3.fromRGB(40, 40, 50)
            input.Text = ""
            input.PlaceholderText = placeholder
            input.TextColor3 = Config.Text
            input.Font = Enum.Font.Gotham
            input.TextSize = 14
            input.Parent = box

            local inputCorner = Instance.new("UICorner")
            inputCorner.CornerRadius = UDim.new(0, 6)
            inputCorner.Parent = input

            input.FocusLost:Connect(function(enter)
                if enter and callback then
                    callback(input.Text)
                end
            end)

            return box
        end

        -- Keybind
        function tab:Keybind(text, callback, default)
            local bind = Instance.new("Frame")
            local label = Instance.new("TextLabel")
            local keyBtn = Instance.new("TextButton")

            bind.Size = UDim2.new(1, 0, 0, 36)
            bind.BackgroundColor3 = Config.Secondary
            bind.Parent = tabFrame

            local corner = Instance.new("UICorner")
            corner.CornerRadius = Config.CornerRadius
            corner.Parent = bind

            label.Size = UDim2.new(0.6, 0, 1, 0)
            label.Position = UDim2.new(0, 12, 0, 0)
            label.BackgroundTransparency = 1
            label.Text = text
            label.TextColor3 = Config.Text
            label.Font = Enum.Font.Gotham
            label.TextSize = 14
            label.TextXAlignment = Enum.TextXAlignment.Left
            label.Parent = bind

            keyBtn.Size = UDim2.new(0, 80, 0, 24)
            keyBtn.Position = UDim2.new(1, -92, 0.5, -12)
            keyBtn.BackgroundColor3 = Color3.fromRGB(40, 40, 50)
            keyBtn.Text = default and default.Name or "None"
            keyBtn.TextColor3 = Config.Text
            keyBtn.Font = Enum.Font.Gotham
            keyBtn.TextSize = 14
            keyBtn.Parent = bind

            local btnCorner = Instance.new("UICorner")
            btnCorner.CornerRadius = UDim.new(0, 6)
            btnCorner.Parent = keyBtn

            local listening = false
            keyBtn.MouseButton1Click:Connect(function()
                listening = true
                keyBtn.Text = "..."
                AetherUI:Notify("Keybind", "Pressione uma tecla...", 3)
            end)

            UserInputService.InputBegan:Connect(function(input)
                if listening and input.KeyCode ~= Enum.KeyCode.Unknown then
                    listening = false
                    keyBtn.Text = input.KeyCode.Name
                    if callback then callback(input.KeyCode) end
                end
            end)

            return bind
        end

        tab.Elements = elements
        return tab
    end

    return window
end

return AetherUI

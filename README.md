-- AetherUI v2.0 - UI Library Fluida e Moderna
-- Cole isso no GitHub como AetherUI.lua

local AetherUI = {}
AetherUI.__index = AetherUI

print("AetherUI carregando...")

local TweenService = game:GetService("TweenService")
local UserInputService = game:GetService("UserInputService")
local RunService = game:GetService("RunService")
local Players = game:GetService("Players")
local player = Players.LocalPlayer
local mouse = player:GetMouse()

-- Configurações
local Config = {
    AccentColor = Color3.fromRGB(0, 170, 255),
    Background = Color3.fromRGB(25, 25, 30),
    Secondary = Color3.fromRGB(35, 35, 40),
    Text = Color3.fromRGB(245, 245, 245),
    CornerRadius = UDim.new(0, 8),
    AnimationTime = 0.25
}

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

    Tween(notif, {Position = UDim2.new(1, -320, 1, -100)}, 0.4, Enum.EasingStyle.Back)

    task.delay(duration, function()
        Tween(notif, {Position = UDim2.new(1, 20, 1, -100)}, 0.4, Enum.EasingStyle.Back):Completed:Connect(function()
            notif:Destroy()
        end)
    end)
end

-- Window
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
            main.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
        end
    end)

    closeBtn.MouseButton1Click:Connect(function()
        Tween(main, {Size = UDim2.new(0, 0, 0, 0)}, 0.3):Completed:Connect(function()
            screenGui:Destroy()
        end)
    end)

    local window = {ScreenGui = screenGui, Container = container}
    local tabs = {}

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
        indicator.Size = UDim2.new(0, 0, 0, 2)
        indicator.Position = UDim2.new(0, 0, 1, -2)
        indicator.BackgroundColor3 = Config.AccentColor
        indicator.Visible = false
        indicator.Parent = tabBtn

        local tab = {Frame = tabFrame, Button = tabBtn, Indicator = indicator}
        table.insert(tabs, tab)

        for i, btn in ipairs(container:GetChildren()) do
            if btn:IsA("TextButton") then
                btn.Position = UDim2.new(0, 15 + (i-1) * 110, 0, 5)
            end
        end

        tabBtn.MouseButton1Click:Connect(function()
            for _, t in ipairs(tabs) do
                t.Frame.Visible = false
                t.Indicator.Visible = false
                Tween(t.Indicator, {Size = UDim2.new(0, 0, 0, 2)}, 0.2)
            end
            tab.Frame.Visible = true
            tab.Indicator.Visible = true
            Tween(tab.Indicator, {Size = UDim2.new(1, 0, 0, 2)}, 0.3)
        end)

        if #tabs == 1 then tabBtn.MouseButton1Click:Fire() end

        function tab:Button(text, callback)
            local btn = Instance.new("TextButton")
            btn.Size = UDim2.new(1, 0, 0, 36)
            btn.BackgroundColor3 = Config.AccentColor
            btn.Text = text
            btn.TextColor3 = Color3.fromRGB(255, 255, 255)
            btn.Font = Enum.Font.GothamBold
            btn.TextSize = 14
            btn.AutoButtonColor = false
            btn.Parent = tabFrame

            local corner = Instance.new("UICorner")
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

        return tab
    end

    return window
end

print("AetherUI carregada com sucesso!")
return AetherUI

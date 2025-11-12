-- Fluent UI Library para Roblox
-- Biblioteca moderna com animações suaves

local FluentUI = {}
local TweenService = game:GetService("TweenService")
local UserInputService = game:GetService("UserInputService")
local RunService = game:GetService("RunService")

-- Configurações de animação
local AnimConfig = {
    Fast = TweenInfo.new(0.2, Enum.EasingStyle.Quint, Enum.EasingDirection.Out),
    Medium = TweenInfo.new(0.3, Enum.EasingStyle.Quint, Enum.EasingDirection.Out),
    Slow = TweenInfo.new(0.5, Enum.EasingStyle.Quint, Enum.EasingDirection.Out),
    Bounce = TweenInfo.new(0.4, Enum.EasingStyle.Back, Enum.EasingDirection.Out)
}

-- Cores do tema
local Theme = {
    Background = Color3.fromRGB(20, 20, 25),
    Secondary = Color3.fromRGB(30, 30, 35),
    Accent = Color3.fromRGB(120, 120, 255),
    Text = Color3.fromRGB(240, 240, 245),
    TextDark = Color3.fromRGB(160, 160, 170),
    Success = Color3.fromRGB(80, 200, 120),
    Warning = Color3.fromRGB(255, 180, 80),
    Error = Color3.fromRGB(255, 100, 100)
}

-- Função auxiliar para criar elementos
local function Create(class, props)
    local obj = Instance.new(class)
    for k, v in pairs(props) do
        if k ~= "Parent" then
            obj[k] = v
        end
    end
    obj.Parent = props.Parent
    return obj
end

-- Função para animar
local function Tween(obj, props, info)
    info = info or AnimConfig.Medium
    local tween = TweenService:Create(obj, info, props)
    tween:Play()
    return tween
end

-- Criar ScreenGui principal
function FluentUI:CreateWindow(config)
    config = config or {}
    local windowTitle = config.Title or "Fluent UI"
    local subtitle = config.Subtitle or "by FluentDev"
    
    -- Criar ScreenGui
    local ScreenGui = Create("ScreenGui", {
        Name = "FluentUI",
        Parent = game.CoreGui,
        ZIndexBehavior = Enum.ZIndexBehavior.Sibling,
        ResetOnSpawn = false
    })
    
    -- Container principal
    local Main = Create("Frame", {
        Name = "Main",
        Parent = ScreenGui,
        BackgroundColor3 = Theme.Background,
        BorderSizePixel = 0,
        Position = UDim2.new(0.5, -350, 0.5, -250),
        Size = UDim2.new(0, 700, 0, 500),
        ClipsDescendants = true
    })
    
    Create("UICorner", {
        Parent = Main,
        CornerRadius = UDim.new(0, 12)
    })
    
    -- Sombra
    local Shadow = Create("ImageLabel", {
        Name = "Shadow",
        Parent = Main,
        BackgroundTransparency = 1,
        Position = UDim2.new(0, -15, 0, -15),
        Size = UDim2.new(1, 30, 1, 30),
        ZIndex = 0,
        Image = "rbxassetid://5554236805",
        ImageColor3 = Color3.fromRGB(0, 0, 0),
        ImageTransparency = 0.5,
        ScaleType = Enum.ScaleType.Slice,
        SliceCenter = Rect.new(23, 23, 277, 277)
    })
    
    -- Header
    local Header = Create("Frame", {
        Name = "Header",
        Parent = Main,
        BackgroundColor3 = Theme.Secondary,
        BorderSizePixel = 0,
        Size = UDim2.new(1, 0, 0, 60)
    })
    
    Create("UICorner", {
        Parent = Header,
        CornerRadius = UDim.new(0, 12)
    })
    
    -- Título
    local Title = Create("TextLabel", {
        Name = "Title",
        Parent = Header,
        BackgroundTransparency = 1,
        Position = UDim2.new(0, 20, 0, 10),
        Size = UDim2.new(1, -40, 0, 25),
        Font = Enum.Font.GothamBold,
        Text = windowTitle,
        TextColor3 = Theme.Text,
        TextSize = 18,
        TextXAlignment = Enum.TextXAlignment.Left
    })
    
    local Subtitle = Create("TextLabel", {
        Name = "Subtitle",
        Parent = Header,
        BackgroundTransparency = 1,
        Position = UDim2.new(0, 20, 0, 35),
        Size = UDim2.new(1, -40, 0, 15),
        Font = Enum.Font.Gotham,
        Text = subtitle,
        TextColor3 = Theme.TextDark,
        TextSize = 12,
        TextXAlignment = Enum.TextXAlignment.Left
    })
    
    -- Botão fechar
    local CloseBtn = Create("TextButton", {
        Name = "CloseBtn",
        Parent = Header,
        BackgroundColor3 = Theme.Error,
        BorderSizePixel = 0,
        Position = UDim2.new(1, -40, 0, 15),
        Size = UDim2.new(0, 30, 0, 30),
        Font = Enum.Font.GothamBold,
        Text = "×",
        TextColor3 = Color3.fromRGB(255, 255, 255),
        TextSize = 20
    })
    
    Create("UICorner", {
        Parent = CloseBtn,
        CornerRadius = UDim.new(0, 8)
    })
    
    CloseBtn.MouseButton1Click:Connect(function()
        Tween(Main, {Size = UDim2.new(0, 0, 0, 0)}, AnimConfig.Fast)
        task.wait(0.2)
        ScreenGui:Destroy()
    end)
    
    -- Container de abas (sidebar)
    local TabContainer = Create("ScrollingFrame", {
        Name = "TabContainer",
        Parent = Main,
        BackgroundColor3 = Theme.Secondary,
        BorderSizePixel = 0,
        Position = UDim2.new(0, 0, 0, 60),
        Size = UDim2.new(0, 150, 1, -60),
        ScrollBarThickness = 4,
        ScrollBarImageColor3 = Theme.Accent
    })
    
    local TabLayout = Create("UIListLayout", {
        Parent = TabContainer,
        SortOrder = Enum.SortOrder.LayoutOrder,
        Padding = UDim.new(0, 5)
    })
    
    Create("UIPadding", {
        Parent = TabContainer,
        PaddingTop = UDim.new(0, 10),
        PaddingLeft = UDim.new(0, 10),
        PaddingRight = UDim.new(0, 10)
    })
    
    -- Container de conteúdo
    local ContentContainer = Create("Frame", {
        Name = "ContentContainer",
        Parent = Main,
        BackgroundTransparency = 1,
        Position = UDim2.new(0, 150, 0, 60),
        Size = UDim2.new(1, -150, 1, -60)
    })
    
    -- Animação de inicialização
    Main.Size = UDim2.new(0, 0, 0, 0)
    Main.Position = UDim2.new(0.5, 0, 0.5, 0)
    Tween(Main, {
        Size = UDim2.new(0, 700, 0, 500),
        Position = UDim2.new(0.5, -350, 0.5, -250)
    }, AnimConfig.Bounce)
    
    -- Sistema de arrastar
    local dragging, dragInput, dragStart, startPos
    
    Header.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 then
            dragging = true
            dragStart = input.Position
            startPos = Main.Position
            
            input.Changed:Connect(function()
                if input.UserInputState == Enum.UserInputState.End then
                    dragging = false
                end
            end)
        end
    end)
    
    Header.InputChanged:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseMovement then
            dragInput = input
        end
    end)
    
    UserInputService.InputChanged:Connect(function(input)
        if input == dragInput and dragging then
            local delta = input.Position - dragStart
            Main.Position = UDim2.new(
                startPos.X.Scale, startPos.X.Offset + delta.X,
                startPos.Y.Scale, startPos.Y.Offset + delta.Y
            )
        end
    end)
    
    -- Objeto Window
    local Window = {
        Tabs = {},
        CurrentTab = nil
    }
    
    -- Sistema de notificações
    function Window:Notify(config)
        config = config or {}
        local title = config.Title or "Notificação"
        local content = config.Content or "Conteúdo da notificação"
        local duration = config.Duration or 3
        local type = config.Type or "Info" -- Info, Success, Warning, Error
        
        local NotifContainer = Create("Frame", {
            Name = "Notification",
            Parent = ScreenGui,
            BackgroundColor3 = Theme.Secondary,
            BorderSizePixel = 0,
            Position = UDim2.new(1, 10, 0, 100),
            Size = UDim2.new(0, 300, 0, 80),
            ClipsDescendants = true
        })
        
        Create("UICorner", {
            Parent = NotifContainer,
            CornerRadius = UDim.new(0, 10)
        })
        
        local TypeColors = {
            Info = Theme.Accent,
            Success = Theme.Success,
            Warning = Theme.Warning,
            Error = Theme.Error
        }
        
        local Accent = Create("Frame", {
            Name = "Accent",
            Parent = NotifContainer,
            BackgroundColor3 = TypeColors[type] or Theme.Accent,
            BorderSizePixel = 0,
            Size = UDim2.new(0, 4, 1, 0)
        })
        
        local NotifTitle = Create("TextLabel", {
            Name = "Title",
            Parent = NotifContainer,
            BackgroundTransparency = 1,
            Position = UDim2.new(0, 15, 0, 10),
            Size = UDim2.new(1, -30, 0, 20),
            Font = Enum.Font.GothamBold,
            Text = title,
            TextColor3 = Theme.Text,
            TextSize = 14,
            TextXAlignment = Enum.TextXAlignment.Left
        })
        
        local NotifContent = Create("TextLabel", {
            Name = "Content",
            Parent = NotifContainer,
            BackgroundTransparency = 1,
            Position = UDim2.new(0, 15, 0, 35),
            Size = UDim2.new(1, -30, 0, 35),
            Font = Enum.Font.Gotham,
            Text = content,
            TextColor3 = Theme.TextDark,
            TextSize = 12,
            TextWrapped = true,
            TextXAlignment = Enum.TextXAlignment.Left,
            TextYAlignment = Enum.TextYAlignment.Top
        })
        
        -- Animação de entrada
        Tween(NotifContainer, {Position = UDim2.new(1, -310, 0, 100)}, AnimConfig.Bounce)
        
        -- Remover após duração
        task.wait(duration)
        Tween(NotifContainer, {Position = UDim2.new(1, 10, 0, 100)}, AnimConfig.Fast)
        task.wait(0.3)
        NotifContainer:Destroy()
    end
    
    -- Criar aba
    function Window:CreateTab(config)
        config = config or {}
        local tabName = config.Name or "Tab"
        local icon = config.Icon or "📄"
        
        -- Botão da aba
        local TabButton = Create("TextButton", {
            Name = tabName,
            Parent = TabContainer,
            BackgroundColor3 = Theme.Background,
            BorderSizePixel = 0,
            Size = UDim2.new(1, -10, 0, 40),
            Font = Enum.Font.GothamBold,
            Text = "  " .. icon .. "  " .. tabName,
            TextColor3 = Theme.TextDark,
            TextSize = 13,
            TextXAlignment = Enum.TextXAlignment.Left
        })
        
        Create("UICorner", {
            Parent = TabButton,
            CornerRadius = UDim.new(0, 8)
        })
        
        Create("UIPadding", {
            Parent = TabButton,
            PaddingLeft = UDim.new(0, 10)
        })
        
        -- Conteúdo da aba
        local TabContent = Create("ScrollingFrame", {
            Name = tabName .. "Content",
            Parent = ContentContainer,
            BackgroundTransparency = 1,
            Size = UDim2.new(1, 0, 1, 0),
            ScrollBarThickness = 6,
            ScrollBarImageColor3 = Theme.Accent,
            Visible = false
        })
        
        local ContentLayout = Create("UIListLayout", {
            Parent = TabContent,
            SortOrder = Enum.SortOrder.LayoutOrder,
            Padding = UDim.new(0, 8)
        })
        
        Create("UIPadding", {
            Parent = TabContent,
            PaddingTop = UDim.new(0, 15),
            PaddingLeft = UDim.new(0, 15),
            PaddingRight = UDim.new(0, 15),
            PaddingBottom = UDim.new(0, 15)
        })
        
        -- Atualizar tamanho do canvas
        ContentLayout:GetPropertyChangedSignal("AbsoluteContentSize"):Connect(function()
            TabContent.CanvasSize = UDim2.new(0, 0, 0, ContentLayout.AbsoluteContentSize.Y + 30)
        end)
        
        -- Função para mostrar aba
        local function ShowTab()
            for _, tab in pairs(Window.Tabs) do
                tab.Content.Visible = false
                Tween(tab.Button, {BackgroundColor3 = Theme.Background, TextColor3 = Theme.TextDark})
            end
            
            TabContent.Visible = true
            Tween(TabButton, {BackgroundColor3 = Theme.Accent, TextColor3 = Theme.Text})
            Window.CurrentTab = Tab
        end
        
        TabButton.MouseButton1Click:Connect(ShowTab)
        
        -- Objeto Tab
        local Tab = {
            Button = TabButton,
            Content = TabContent,
            Elements = {}
        }
        
        -- Seção
        function Tab:CreateSection(name)
            local Section = Create("TextLabel", {
                Name = "Section",
                Parent = TabContent,
                BackgroundTransparency = 1,
                Size = UDim2.new(1, -20, 0, 30),
                Font = Enum.Font.GothamBold,
                Text = name,
                TextColor3 = Theme.Text,
                TextSize = 15,
                TextXAlignment = Enum.TextXAlignment.Left
            })
            
            return Section
        end
        
        -- Botão
        function Tab:CreateButton(config)
            config = config or {}
            local buttonName = config.Name or "Button"
            local callback = config.Callback or function() end
            
            local Button = Create("TextButton", {
                Name = "Button",
                Parent = TabContent,
                BackgroundColor3 = Theme.Secondary,
                BorderSizePixel = 0,
                Size = UDim2.new(1, -20, 0, 40),
                Font = Enum.Font.GothamBold,
                Text = buttonName,
                TextColor3 = Theme.Text,
                TextSize = 13
            })
            
            Create("UICorner", {
                Parent = Button,
                CornerRadius = UDim.new(0, 8)
            })
            
            Button.MouseEnter:Connect(function()
                Tween(Button, {BackgroundColor3 = Theme.Accent})
            end)
            
            Button.MouseLeave:Connect(function()
                Tween(Button, {BackgroundColor3 = Theme.Secondary})
            end)
            
            Button.MouseButton1Click:Connect(function()
                Tween(Button, {Size = UDim2.new(1, -20, 0, 35)}, AnimConfig.Fast)
                task.wait(0.1)
                Tween(Button, {Size = UDim2.new(1, -20, 0, 40)}, AnimConfig.Fast)
                callback()
            end)
            
            return Button
        end
        
        -- Toggle
        function Tab:CreateToggle(config)
            config = config or {}
            local toggleName = config.Name or "Toggle"
            local default = config.Default or false
            local callback = config.Callback or function() end
            
            local ToggleFrame = Create("Frame", {
                Name = "Toggle",
                Parent = TabContent,
                BackgroundColor3 = Theme.Secondary,
                BorderSizePixel = 0,
                Size = UDim2.new(1, -20, 0, 40)
            })
            
            Create("UICorner", {
                Parent = ToggleFrame,
                CornerRadius = UDim.new(0, 8)
            })
            
            local ToggleLabel = Create("TextLabel", {
                Name = "Label",
                Parent = ToggleFrame,
                BackgroundTransparency = 1,
                Position = UDim2.new(0, 15, 0, 0),
                Size = UDim2.new(1, -70, 1, 0),
                Font = Enum.Font.Gotham,
                Text = toggleName,
                TextColor3 = Theme.Text,
                TextSize = 13,
                TextXAlignment = Enum.TextXAlignment.Left
            })
            
            local ToggleButton = Create("TextButton", {
                Name = "Button",
                Parent = ToggleFrame,
                BackgroundColor3 = default and Theme.Success or Theme.Background,
                BorderSizePixel = 0,
                Position = UDim2.new(1, -50, 0.5, -12),
                Size = UDim2.new(0, 40, 0, 24),
                Text = ""
            })
            
            Create("UICorner", {
                Parent = ToggleButton,
                CornerRadius = UDim.new(1, 0)
            })
            
            local ToggleCircle = Create("Frame", {
                Name = "Circle",
                Parent = ToggleButton,
                BackgroundColor3 = Theme.Text,
                BorderSizePixel = 0,
                Position = default and UDim2.new(1, -20, 0.5, -10) or UDim2.new(0, 4, 0.5, -10),
                Size = UDim2.new(0, 16, 0, 16)
            })
            
            Create("UICorner", {
                Parent = ToggleCircle,
                CornerRadius = UDim.new(1, 0)
            })
            
            local toggled = default
            
            ToggleButton.MouseButton1Click:Connect(function()
                toggled = not toggled
                
                Tween(ToggleButton, {
                    BackgroundColor3 = toggled and Theme.Success or Theme.Background
                })
                
                Tween(ToggleCircle, {
                    Position = toggled and UDim2.new(1, -20, 0.5, -10) or UDim2.new(0, 4, 0.5, -10)
                })
                
                callback(toggled)
            end)
            
            return ToggleFrame
        end
        
        -- Slider
        function Tab:CreateSlider(config)
            config = config or {}
            local sliderName = config.Name or "Slider"
            local min = config.Min or 0
            local max = config.Max or 100
            local default = config.Default or 50
            local callback = config.Callback or function() end
            
            local SliderFrame = Create("Frame", {
                Name = "Slider",
                Parent = TabContent,
                BackgroundColor3 = Theme.Secondary,
                BorderSizePixel = 0,
                Size = UDim2.new(1, -20, 0, 60)
            })
            
            Create("UICorner", {
                Parent = SliderFrame,
                CornerRadius = UDim.new(0, 8)
            })
            
            local SliderLabel = Create("TextLabel", {
                Name = "Label",
                Parent = SliderFrame,
                BackgroundTransparency = 1,
                Position = UDim2.new(0, 15, 0, 8),
                Size = UDim2.new(1, -30, 0, 20),
                Font = Enum.Font.Gotham,
                Text = sliderName,
                TextColor3 = Theme.Text,
                TextSize = 13,
                TextXAlignment = Enum.TextXAlignment.Left
            })
            
            local SliderValue = Create("TextLabel", {
                Name = "Value",
                Parent = SliderFrame,
                BackgroundTransparency = 1,
                Position = UDim2.new(1, -60, 0, 8),
                Size = UDim2.new(0, 45, 0, 20),
                Font = Enum.Font.GothamBold,
                Text = tostring(default),
                TextColor3 = Theme.Accent,
                TextSize = 13,
                TextXAlignment = Enum.TextXAlignment.Right
            })
            
            local SliderBar = Create("Frame", {
                Name = "Bar",
                Parent = SliderFrame,
                BackgroundColor3 = Theme.Background,
                BorderSizePixel = 0,
                Position = UDim2.new(0, 15, 1, -20),
                Size = UDim2.new(1, -30, 0, 6)
            })
            
            Create("UICorner", {
                Parent = SliderBar,
                CornerRadius = UDim.new(1, 0)
            })
            
            local SliderFill = Create("Frame", {
                Name = "Fill",
                Parent = SliderBar,
                BackgroundColor3 = Theme.Accent,
                BorderSizePixel = 0,
                Size = UDim2.new((default - min) / (max - min), 0, 1, 0)
            })
            
            Create("UICorner", {
                Parent = SliderFill,
                CornerRadius = UDim.new(1, 0)
            })
            
            local SliderDot = Create("Frame", {
                Name = "Dot",
                Parent = SliderBar,
                BackgroundColor3 = Theme.Text,
                BorderSizePixel = 0,
                Position = UDim2.new((default - min) / (max - min), -6, 0.5, -6),
                Size = UDim2.new(0, 12, 0, 12)
            })
            
            Create("UICorner", {
                Parent = SliderDot,
                CornerRadius = UDim.new(1, 0)
            })
            
            local dragging = false
            
            SliderBar.InputBegan:Connect(function(input)
                if input.UserInputType == Enum.UserInputType.MouseButton1 then
                    dragging = true
                end
            end)
            
            UserInputService.InputEnded:Connect(function(input)
                if input.UserInputType == Enum.UserInputType.MouseButton1 then
                    dragging = false
                end
            end)
            
            UserInputService.InputChanged:Connect(function(input)
                if dragging and input.UserInputType == Enum.UserInputType.MouseMovement then
                    local pos = math.clamp((input.Position.X - SliderBar.AbsolutePosition.X) / SliderBar.AbsoluteSize.X, 0, 1)
                    local value = math.floor(min + (max - min) * pos)
                    
                    SliderValue.Text = tostring(value)
                    SliderFill.Size = UDim2.new(pos, 0, 1, 0)
                    SliderDot.Position = UDim2.new(pos, -6, 0.5, -6)
                    
                    callback(value)
                end
            end)
            
            return SliderFrame
        end
        
        -- TextBox
        function Tab:CreateTextBox(config)
            config = config or {}
            local boxName = config.Name or "TextBox"
            local placeholder = config.Placeholder or "Digite aqui..."
            local callback = config.Callback or function() end
            
            local TextBoxFrame = Create("Frame", {
                Name = "TextBox",
                Parent = TabContent,
                BackgroundColor3 = Theme.Secondary,
                BorderSizePixel = 0,
                Size = UDim2.new(1, -20, 0, 70)
            })
            
            Create("UICorner", {
                Parent = TextBoxFrame,
                CornerRadius = UDim.new(0, 8)
            })
            
            local BoxLabel = Create("TextLabel", {
                Name = "Label",
                Parent = TextBoxFrame,
                BackgroundTransparency = 1,
                Position = UDim2.new(0, 15, 0, 8),
                Size = UDim2.new(1, -30, 0, 20),
                Font = Enum.Font.Gotham,
                Text = boxName,
                TextColor3 = Theme.Text,
                TextSize = 13,
                TextXAlignment = Enum.TextXAlignment.Left
            })
            
            local TextBox = Create("TextBox", {
                Name = "Input",
                Parent = TextBoxFrame,
                BackgroundColor3 = Theme.Background,
                BorderSizePixel = 0,
                Position = UDim2.new(0, 15, 0, 35),
                Size = UDim2.new(1, -30, 0, 25),
                Font = Enum.Font.Gotham,
                PlaceholderText = placeholder,
                PlaceholderColor3 = Theme.TextDark,
                Text = "",
                TextColor3 = Theme.Text,
                TextSize = 12,
                ClearTextOnFocus = false
            })
            
            Create("UICorner", {
                Parent = TextBox,
                CornerRadius = UDim.new(0, 6)
            })
            
            Create("UIPadding", {
                Parent = TextBox,
                PaddingLeft = UDim.new(0, 10),
                PaddingRight = UDim.new(0, 10)
            })
            
            TextBox.FocusLost:Connect(function(enter)
                if enter then
                    callback(TextBox.Text)
                end
            end)
            
            return TextBoxFrame
        end
        
        -- ColorPicker
        function Tab:CreateColorPicker(config)
            config = config or {}
            local pickerName = config.Name or "Color Picker"
            local default = config.Default or Color3.fromRGB(255, 0, 0)
            local callback = config.Callback or function() end
            
            local currentColor = default
            local hue, sat, val = 0, 1, 1
            
            -- Converter RGB para HSV
            local function RGBtoHSV(color)
                local r, g, b = color.R, color.G, color.B
                local max = math.max(r, g, b)
                local min = math.min(r, g, b)
                local delta = max - min
                
                local h = 0
                if delta > 0 then
                    if max == r then
                        h = ((g - b) / delta) % 6
                    elseif max == g then
                        h = (b - r) / delta + 2
                    else
                        h = (r - g) / delta + 4
                    end
                    h = h / 6
                end
                
                local s = max == 0 and 0 or delta / max
                local v = max
                
                return h, s, v
            end
            
            -- Converter HSV para RGB
            local function HSVtoRGB(h, s, v)
                local r, g, b
                
                local i = math.floor(h * 6)
                local f = h * 6 - i
                local p = v * (1 - s)
                local q = v * (1 - f * s)
                local t = v * (1 - (1 - f) * s)
                
                i = i % 6
                
                if i == 0 then r, g, b = v, t, p
                elseif i == 1 then r, g, b = q, v, p
                elseif i == 2 then r, g, b = p, v, t
                elseif i == 3 then r, g, b = p, q, v
                elseif i == 4 then r, g, b = t, p, v
                elseif i == 5 then r, g, b = v, p, q
                end
                
                return Color3.new(r, g, b)
            end
            
            hue, sat, val = RGBtoHSV(default)
            
            local PickerFrame = Create("Frame", {
                Name = "ColorPicker",
                Parent = TabContent,
                BackgroundColor3 = Theme.Secondary,
                BorderSizePixel = 0,
                Size = UDim2.new(1, -20, 0, 40),
                ClipsDescendants = false
            })
            
            Create("UICorner", {
                Parent = PickerFrame,
                CornerRadius = UDim.new(0, 8)
            })
            
            local PickerLabel = Create("TextLabel", {
                Name = "Label",
                Parent = PickerFrame,
                BackgroundTransparency = 1,
                Position = UDim2.new(0, 15, 0, 0),
                Size = UDim2.new(1, -70, 1, 0),
                Font = Enum.Font.Gotham,
                Text = pickerName,
                TextColor3 = Theme.Text,
                TextSize = 13,
                TextXAlignment = Enum.TextXAlignment.Left
            })
            
            local ColorDisplay = Create("TextButton", {
                Name = "Display",
                Parent = PickerFrame,
                BackgroundColor3 = currentColor,
                BorderSizePixel = 0,
                Position = UDim2.new(1, -45, 0.5, -12),
                Size = UDim2.new(0, 30, 0, 24),
                Text = ""
            })
            
            Create("UICorner", {
                Parent = ColorDisplay,
                CornerRadius = UDim.new(0, 6)
            })
            
            -- Painel do Color Picker (AGORA NA SCREENGUI PRINCIPAL)
            local PickerPanel = Create("Frame", {
                Name = "PickerPanel",
                Parent = ScreenGui,
                BackgroundColor3 = Theme.Background,
                BorderSizePixel = 0,
                Position = UDim2.new(0.5, 380, 0.5, -150),
                Size = UDim2.new(0, 0, 0, 0),
                Visible = false,
                ClipsDescendants = false,
                ZIndex = 100
            })
            
            Create("UICorner", {
                Parent = PickerPanel,
                CornerRadius = UDim.new(0, 12)
            })
            
            -- Sombra do painel
            local PanelShadow = Create("ImageLabel", {
                Name = "Shadow",
                Parent = PickerPanel,
                BackgroundTransparency = 1,
                Position = UDim2.new(0, -15, 0, -15),
                Size = UDim2.new(1, 30, 1, 30),
                ZIndex = 99,
                Image = "rbxassetid://5554236805",
                ImageColor3 = Color3.fromRGB(0, 0, 0),
                ImageTransparency = 0.5,
                ScaleType = Enum.ScaleType.Slice,
                SliceCenter = Rect.new(23, 23, 277, 277)
            })
            
            -- Gradiente de saturação/brilho
            local ColorCanvas = Create("ImageButton", {
                Name = "ColorCanvas",
                Parent = PickerPanel,
                BackgroundColor3 = HSVtoRGB(hue, 1, 1),
                BorderSizePixel = 0,
                Position = UDim2.new(0, 15, 0, 15),
                Size = UDim2.new(0, 220, 0, 220),
                AutoButtonColor = false,
                Image = "",
                ZIndex = 101
            })
            
            Create("UICorner", {
                Parent = ColorCanvas,
                CornerRadius = UDim.new(0, 8)
            })
            
            -- Gradiente branco para a saturação
            local WhiteGradient = Create("Frame", {
                Name = "White",
                Parent = ColorCanvas,
                BackgroundColor3 = Color3.new(1, 1, 1),
                BorderSizePixel = 0,
                Size = UDim2.new(1, 0, 1, 0),
                ZIndex = 102
            })
            
            Create("UICorner", {
                Parent = WhiteGradient,
                CornerRadius = UDim.new(0, 8)
            })
            
            local WhiteGrad = Create("UIGradient", {
                Parent = WhiteGradient,
                Transparency = NumberSequence.new({
                    NumberSequenceKeypoint.new(0, 0),
                    NumberSequenceKeypoint.new(1, 1)
                }),
                Rotation = 0
            })
            
            -- Gradiente preto para o brilho
            local BlackGradient = Create("Frame", {
                Name = "Black",
                Parent = ColorCanvas,
                BackgroundColor3 = Color3.new(0, 0, 0),
                BorderSizePixel = 0,
                Size = UDim2.new(1, 0, 1, 0),
                ZIndex = 103
            })
            
            Create("UICorner", {
                Parent = BlackGradient,
                CornerRadius = UDim.new(0, 8)
            })
            
            local BlackGrad = Create("UIGradient", {
                Parent = BlackGradient,
                Transparency = NumberSequence.new({
                    NumberSequenceKeypoint.new(0, 1),
                    NumberSequenceKeypoint.new(1, 0)
                }),
                Rotation = 90
            })
            
            -- Cursor do canvas
            local CanvasCursor = Create("Frame", {
                Name = "Cursor",
                Parent = ColorCanvas,
                BackgroundColor3 = Color3.new(1, 1, 1),
                BorderSizePixel = 3,
                BorderColor3 = Color3.new(0, 0, 0),
                Position = UDim2.new(sat, -8, 1 - val, -8),
                Size = UDim2.new(0, 16, 0, 16),
                ZIndex = 105
            })
            
            Create("UICorner", {
                Parent = CanvasCursor,
                CornerRadius = UDim.new(1, 0)
            })
            
            -- Barra de HUE (arco-íris)
            local HueBar = Create("ImageButton", {
                Name = "HueBar",
                Parent = PickerPanel,
                BackgroundColor3 = Color3.new(1, 1, 1),
                BorderSizePixel = 0,
                Position = UDim2.new(0, 250, 0, 15),
                Size = UDim2.new(0, 25, 0, 220),
                AutoButtonColor = false,
                Image = "",
                ZIndex = 101
            })
            
            Create("UICorner", {
                Parent = HueBar,
                CornerRadius = UDim.new(0, 8)
            })
            
            local HueGradient = Create("UIGradient", {
                Parent = HueBar,
                Color = ColorSequence.new({
                    ColorSequenceKeypoint.new(0, Color3.fromRGB(255, 0, 0)),
                    ColorSequenceKeypoint.new(0.17, Color3.fromRGB(255, 255, 0)),
                    ColorSequenceKeypoint.new(0.33, Color3.fromRGB(0, 255, 0)),
                    ColorSequenceKeypoint.new(0.5, Color3.fromRGB(0, 255, 255)),
                    ColorSequenceKeypoint.new(0.67, Color3.fromRGB(0, 0, 255)),
                    ColorSequenceKeypoint.new(0.83, Color3.fromRGB(255, 0, 255)),
                    ColorSequenceKeypoint.new(1, Color3.fromRGB(255, 0, 0))
                }),
                Rotation = 90
            })
            
            -- Cursor da barra de HUE
            local HueCursor = Create("Frame", {
                Name = "Cursor",
                Parent = HueBar,
                BackgroundColor3 = Color3.new(1, 1, 1),
                BorderSizePixel = 3,
                BorderColor3 = Color3.new(0, 0, 0),
                Position = UDim2.new(0.5, -15, hue, -4),
                Size = UDim2.new(1, 6, 0, 8),
                ZIndex = 105
            })
            
            Create("UICorner", {
                Parent = HueCursor,
                CornerRadius = UDim.new(0, 4)
            })
            
            -- Display RGB
            local RGBDisplay = Create("TextLabel", {
                Name = "RGB",
                Parent = PickerPanel,
                BackgroundColor3 = Theme.Secondary,
                BorderSizePixel = 0,
                Position = UDim2.new(0, 15, 0, 245),
                Size = UDim2.new(1, -30, 0, 30),
                Font = Enum.Font.GothamBold,
                Text = string.format("RGB: %d, %d, %d", 
                    math.floor(currentColor.R * 255),
                    math.floor(currentColor.G * 255),
                    math.floor(currentColor.B * 255)
                ),
                TextColor3 = Theme.Text,
                TextSize = 12,
                ZIndex = 101
            })
            
            Create("UICorner", {
                Parent = RGBDisplay,
                CornerRadius = UDim.new(0, 8)
            })
            
            -- Função para atualizar cor
            local function UpdateColor()
                currentColor = HSVtoRGB(hue, sat, val)
                ColorDisplay.BackgroundColor3 = currentColor
                ColorCanvas.BackgroundColor3 = HSVtoRGB(hue, 1, 1)
                RGBDisplay.Text = string.format("RGB: %d, %d, %d", 
                    math.floor(currentColor.R * 255),
                    math.floor(currentColor.G * 255),
                    math.floor(currentColor.B * 255)
                )
                callback(currentColor)
            end
            
            -- Arrasto no canvas
            local canvasDragging = false
            
            ColorCanvas.MouseButton1Down:Connect(function()
                canvasDragging = true
            end)
            
            UserInputService.InputEnded:Connect(function(input)
                if input.UserInputType == Enum.UserInputType.MouseButton1 then
                    canvasDragging = false
                end
            end)
            
            ColorCanvas.InputChanged:Connect(function(input)
                if input.UserInputType == Enum.UserInputType.MouseMovement then
                    if canvasDragging then
                        local posX = math.clamp((input.Position.X - ColorCanvas.AbsolutePosition.X) / ColorCanvas.AbsoluteSize.X, 0, 1)
                        local posY = math.clamp((input.Position.Y - ColorCanvas.AbsolutePosition.Y) / ColorCanvas.AbsoluteSize.Y, 0, 1)
                        
                        sat = posX
                        val = 1 - posY
                        
                        CanvasCursor.Position = UDim2.new(sat, -8, 1 - val, -8)
                        UpdateColor()
                    end
                end
            end)
            
            -- Arrasto na barra de HUE
            local hueDragging = false
            
            HueBar.MouseButton1Down:Connect(function()
                hueDragging = true
            end)
            
            UserInputService.InputEnded:Connect(function(input)
                if input.UserInputType == Enum.UserInputType.MouseButton1 then
                    hueDragging = false
                end
            end)
            
            HueBar.InputChanged:Connect(function(input)
                if input.UserInputType == Enum.UserInputType.MouseMovement then
                    if hueDragging then
                        local posY = math.clamp((input.Position.Y - HueBar.AbsolutePosition.Y) / HueBar.AbsoluteSize.Y, 0, 1)
                        
                        hue = posY
                        HueCursor.Position = UDim2.new(0.5, -15, hue, -4)
                        UpdateColor()
                    end
                end
            end)
            
            -- Toggle do painel
            local expanded = false
            
            ColorDisplay.MouseButton1Click:Connect(function()
                expanded = not expanded
                
                if expanded then
                    -- Posicionar ao lado direito da UI principal
                    PickerPanel.Position = UDim2.new(0.5, 380, 0.5, -150)
                    PickerPanel.Visible = true
                    Tween(PickerPanel, {Size = UDim2.new(0, 290, 0, 285)}, AnimConfig.Bounce)
                else
                    Tween(PickerPanel, {Size = UDim2.new(0, 0, 0, 0)}, AnimConfig.Fast)
                    task.wait(0.2)
                    PickerPanel.Visible = false
                end
            end)
            
            return PickerFrame
        end
        
        -- Keybind
        function Tab:CreateKeybind(config)
            config = config or {}
            local keybindName = config.Name or "Keybind"
            local default = config.Default or Enum.KeyCode.E
            local callback = config.Callback or function() end
            
            local KeybindFrame = Create("Frame", {
                Name = "Keybind",
                Parent = TabContent,
                BackgroundColor3 = Theme.Secondary,
                BorderSizePixel = 0,
                Size = UDim2.new(1, -20, 0, 40)
            })
            
            Create("UICorner", {
                Parent = KeybindFrame,
                CornerRadius = UDim.new(0, 8)
            })
            
            local KeybindLabel = Create("TextLabel", {
                Name = "Label",
                Parent = KeybindFrame,
                BackgroundTransparency = 1,
                Position = UDim2.new(0, 15, 0, 0),
                Size = UDim2.new(1, -90, 1, 0),
                Font = Enum.Font.Gotham,
                Text = keybindName,
                TextColor3 = Theme.Text,
                TextSize = 13,
                TextXAlignment = Enum.TextXAlignment.Left
            })
            
            local currentKey = default
            local KeybindButton = Create("TextButton", {
                Name = "Button",
                Parent = KeybindFrame,
                BackgroundColor3 = Theme.Background,
                BorderSizePixel = 0,
                Position = UDim2.new(1, -70, 0.5, -12),
                Size = UDim2.new(0, 55, 0, 24),
                Font = Enum.Font.GothamBold,
                Text = currentKey.Name,
                TextColor3 = Theme.Accent,
                TextSize = 11
            })
            
            Create("UICorner", {
                Parent = KeybindButton,
                CornerRadius = UDim.new(0, 6)
            })
            
            local binding = false
            
            KeybindButton.MouseButton1Click:Connect(function()
                binding = true
                KeybindButton.Text = "..."
                Tween(KeybindButton, {BackgroundColor3 = Theme.Accent})
            end)
            
            UserInputService.InputBegan:Connect(function(input, gameProcessed)
                if binding and input.UserInputType == Enum.UserInputType.Keyboard then
                    currentKey = input.KeyCode
                    KeybindButton.Text = currentKey.Name
                    binding = false
                    Tween(KeybindButton, {BackgroundColor3 = Theme.Background})
                end
                
                if not gameProcessed and input.KeyCode == currentKey then
                    callback()
                end
            end)
            
            return KeybindFrame
        end
        
        -- Dropdown
        function Tab:CreateDropdown(config)
            config = config or {}
            local dropdownName = config.Name or "Dropdown"
            local options = config.Options or {"Opção 1", "Opção 2", "Opção 3"}
            local default = config.Default or options[1]
            local callback = config.Callback or function() end
            
            local DropdownFrame = Create("Frame", {
                Name = "Dropdown",
                Parent = TabContent,
                BackgroundColor3 = Theme.Secondary,
                BorderSizePixel = 0,
                Size = UDim2.new(1, -20, 0, 40),
                ClipsDescendants = false
            })
            
            Create("UICorner", {
                Parent = DropdownFrame,
                CornerRadius = UDim.new(0, 8)
            })
            
            local DropdownLabel = Create("TextLabel", {
                Name = "Label",
                Parent = DropdownFrame,
                BackgroundTransparency = 1,
                Position = UDim2.new(0, 15, 0, 0),
                Size = UDim2.new(1, -30, 0, 40),
                Font = Enum.Font.Gotham,
                Text = dropdownName .. ": " .. default,
                TextColor3 = Theme.Text,
                TextSize = 13,
                TextXAlignment = Enum.TextXAlignment.Left
            })
            
            local DropdownIcon = Create("TextLabel", {
                Name = "Icon",
                Parent = DropdownFrame,
                BackgroundTransparency = 1,
                Position = UDim2.new(1, -30, 0, 0),
                Size = UDim2.new(0, 30, 0, 40),
                Font = Enum.Font.GothamBold,
                Text = "▼",
                TextColor3 = Theme.Accent,
                TextSize = 12
            })
            
            local DropdownList = Create("Frame", {
                Name = "List",
                Parent = DropdownFrame,
                BackgroundColor3 = Theme.Background,
                BorderSizePixel = 0,
                Position = UDim2.new(0, 0, 0, 45),
                Size = UDim2.new(1, 0, 0, 0),
                Visible = false,
                ClipsDescendants = true,
                ZIndex = 10
            })
            
            Create("UICorner", {
                Parent = DropdownList,
                CornerRadius = UDim.new(0, 8)
            })
            
            local ListLayout = Create("UIListLayout", {
                Parent = DropdownList,
                SortOrder = Enum.SortOrder.LayoutOrder,
                Padding = UDim.new(0, 2)
            })
            
            local expanded = false
            local selectedOption = default
            
            for _, option in ipairs(options) do
                local OptionButton = Create("TextButton", {
                    Name = option,
                    Parent = DropdownList,
                    BackgroundColor3 = Theme.Secondary,
                    BorderSizePixel = 0,
                    Size = UDim2.new(1, 0, 0, 30),
                    Font = Enum.Font.Gotham,
                    Text = option,
                    TextColor3 = option == selectedOption and Theme.Accent or Theme.Text,
                    TextSize = 12
                })
                
                Create("UICorner", {
                    Parent = OptionButton,
                    CornerRadius = UDim.new(0, 6)
                })
                
                OptionButton.MouseButton1Click:Connect(function()
                    selectedOption = option
                    DropdownLabel.Text = dropdownName .. ": " .. option
                    
                    for _, btn in ipairs(DropdownList:GetChildren()) do
                        if btn:IsA("TextButton") then
                            Tween(btn, {TextColor3 = btn.Name == option and Theme.Accent or Theme.Text})
                        end
                    end
                    
                    expanded = false
                    Tween(DropdownList, {Size = UDim2.new(1, 0, 0, 0)}, AnimConfig.Fast)
                    Tween(DropdownIcon, {Rotation = 0}, AnimConfig.Fast)
                    task.wait(0.2)
                    DropdownList.Visible = false
                    
                    callback(option)
                end)
                
                OptionButton.MouseEnter:Connect(function()
                    if option ~= selectedOption then
                        Tween(OptionButton, {BackgroundColor3 = Theme.Background})
                    end
                end)
                
                OptionButton.MouseLeave:Connect(function()
                    Tween(OptionButton, {BackgroundColor3 = Theme.Secondary})
                end)
            end
            
            local listHeight = #options * 32
            
            DropdownFrame.InputBegan:Connect(function(input)
                if input.UserInputType == Enum.UserInputType.MouseButton1 then
                    expanded = not expanded
                    
                    if expanded then
                        DropdownList.Visible = true
                        Tween(DropdownList, {Size = UDim2.new(1, 0, 0, listHeight)}, AnimConfig.Medium)
                        Tween(DropdownIcon, {Rotation = 180}, AnimConfig.Fast)
                    else
                        Tween(DropdownList, {Size = UDim2.new(1, 0, 0, 0)}, AnimConfig.Fast)
                        Tween(DropdownIcon, {Rotation = 0}, AnimConfig.Fast)
                        task.wait(0.2)
                        DropdownList.Visible = false
                    end
                end
            end)
            
            return DropdownFrame
        end
        
        -- Adicionar à lista de abas
        table.insert(Window.Tabs, Tab)
        
        -- Mostrar primeira aba
        if #Window.Tabs == 1 then
            ShowTab()
        end
        
        return Tab
    end
    
    return Window
end

return FluentUI

--[[
    ============================================
    EXEMPLO DE USO COM FUNÇÕES FUNCIONAIS
    ============================================
]]

local FluentUI = loadstring(game:HttpGet("URL_DO_SCRIPT"))()
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")

local Player = Players.LocalPlayer
local Character = Player.Character or Player.CharacterAdded:Wait()
local Humanoid = Character:WaitForChild("Humanoid")

-- Criar janela
local Window = FluentUI:CreateWindow({
    Title = "Fluent Hub",
    Subtitle = "v1.0 - Script Completo"
})

-- Notificação de boas-vindas
Window:Notify({
    Title = "Bem-vindo!",
    Content = "Script carregado com sucesso!",
    Duration = 4,
    Type = "Success"
})

-- Variáveis globais
local ESPEnabled = false
local ESPColor = Color3.fromRGB(255, 0, 0)
local FlyEnabled = false
local FlySpeed = 50
local SpeedEnabled = false
local WalkSpeed = 16
local JumpPowerEnabled = false
local JumpPower = 50

-- ======== ABA PRINCIPAL ========
local MainTab = Window:CreateTab({
    Name = "Principal",
    Icon = "🏠"
})

MainTab:CreateSection("🎯 Movimento")

-- Velocidade
MainTab:CreateToggle({
    Name = "Velocidade Customizada",
    Default = false,
    Callback = function(value)
        SpeedEnabled = value
        if not value then
            Humanoid.WalkSpeed = 16
        end
        Window:Notify({
            Title = "Velocidade",
            Content = value and "Ativado!" or "Desativado!",
            Type = value and "Success" or "Warning"
        })
    end
})

MainTab:CreateSlider({
    Name = "Velocidade",
    Min = 16,
    Max = 200,
    Default = 16,
    Callback = function(value)
        WalkSpeed = value
        if SpeedEnabled then
            Humanoid.WalkSpeed = value
        end
    end
})

-- Força do Pulo
MainTab:CreateToggle({
    Name = "Pulo Customizado",
    Default = false,
    Callback = function(value)
        JumpPowerEnabled = value
        if not value then
            Humanoid.JumpPower = 50
        end
        Window:Notify({
            Title = "Força do Pulo",
            Content = value and "Ativado!" or "Desativado!",
            Type = value and "Success" or "Warning"
        })
    end
})

MainTab:CreateSlider({
    Name = "Força do Pulo",
    Min = 50,
    Max = 300,
    Default = 50,
    Callback = function(value)
        JumpPower = value
        if JumpPowerEnabled then
            Humanoid.JumpPower = value
        end
    end
})

-- Fly
MainTab:CreateToggle({
    Name = "Voar (Fly)",
    Default = false,
    Callback = function(value)
        FlyEnabled = value
        
        if value then
            -- Sistema de Fly
            local BodyVelocity = Instance.new("BodyVelocity")
            BodyVelocity.MaxForce = Vector3.new(100000, 100000, 100000)
            BodyVelocity.Velocity = Vector3.new(0, 0, 0)
            BodyVelocity.Parent = Character.HumanoidRootPart
            
            local BodyGyro = Instance.new("BodyGyro")
            BodyGyro.MaxTorque = Vector3.new(100000, 100000, 100000)
            BodyGyro.P = 10000
            BodyGyro.Parent = Character.HumanoidRootPart
            
            local FlyConnection
            FlyConnection = RunService.Heartbeat:Connect(function()
                if not FlyEnabled then
                    BodyVelocity:Destroy()
                    BodyGyro:Destroy()
                    FlyConnection:Disconnect()
                    return
                end
                
                local Camera = workspace.CurrentCamera
                local MoveDirection = Vector3.new(0, 0, 0)
                
                if UserInputService:IsKeyDown(Enum.KeyCode.W) then
                    MoveDirection = MoveDirection + Camera.CFrame.LookVector
                end
                if UserInputService:IsKeyDown(Enum.KeyCode.S) then
                    MoveDirection = MoveDirection - Camera.CFrame.LookVector
                end
                if UserInputService:IsKeyDown(Enum.KeyCode.A) then
                    MoveDirection = MoveDirection - Camera.CFrame.RightVector
                end
                if UserInputService:IsKeyDown(Enum.KeyCode.D) then
                    MoveDirection = MoveDirection + Camera.CFrame.RightVector
                end
                if UserInputService:IsKeyDown(Enum.KeyCode.Space) then
                    MoveDirection = MoveDirection + Vector3.new(0, 1, 0)
                end
                if UserInputService:IsKeyDown(Enum.KeyCode.LeftShift) then
                    MoveDirection = MoveDirection - Vector3.new(0, 1, 0)
                end
                
                BodyVelocity.Velocity = MoveDirection * FlySpeed
                BodyGyro.CFrame = Camera.CFrame
            end)
            
            Window:Notify({
                Title = "Fly",
                Content = "Use WASD + Space/Shift para voar!",
                Type = "Info",
                Duration = 5
            })
        end
    end
})

MainTab:CreateSlider({
    Name = "Velocidade do Fly",
    Min = 10,
    Max = 200,
    Default = 50,
    Callback = function(value)
        FlySpeed = value
    end
})

-- ======== ABA VISUAL ========
local VisualTab = Window:CreateTab({
    Name = "Visual",
    Icon = "👁️"
})

VisualTab:CreateSection("🔍 ESP")

-- ESP Toggle
VisualTab:CreateToggle({
    Name = "ESP (Jogadores)",
    Default = false,
    Callback = function(value)
        ESPEnabled = value
        
        if value then
            -- Criar ESP para todos os jogadores
            for _, player in ipairs(Players:GetPlayers()) do
                if player ~= Player then
                    local function CreateESP(character)
                        if character:FindFirstChild("HumanoidRootPart") and not character:FindFirstChild("ESP_Highlight") then
                            local Highlight = Instance.new("Highlight")
                            Highlight.Name = "ESP_Highlight"
                            Highlight.FillColor = ESPColor
                            Highlight.OutlineColor = ESPColor
                            Highlight.FillTransparency = 0.5
                            Highlight.OutlineTransparency = 0
                            Highlight.Parent = character
                        end
                    end
                    
                    if player.Character then
                        CreateESP(player.Character)
                    end
                    
                    player.CharacterAdded:Connect(function(char)
                        if ESPEnabled then
                            CreateESP(char)
                        end
                    end)
                end
            end
            
            Window:Notify({
                Title = "ESP",
                Content = "ESP ativado!",
                Type = "Success"
            })
        else
            -- Remover todos os ESPs
            for _, player in ipairs(Players:GetPlayers()) do
                if player.Character and player.Character:FindFirstChild("ESP_Highlight") then
                    player.Character.ESP_Highlight:Destroy()
                end
            end
        end
    end
})

-- Cor do ESP
VisualTab:CreateColorPicker({
    Name = "Cor do ESP",
    Default = Color3.fromRGB(255, 0, 0),
    Callback = function(color)
        ESPColor = color
        
        -- Atualizar cor de todos os ESPs ativos
        if ESPEnabled then
            for _, player in ipairs(Players:GetPlayers()) do
                if player.Character and player.Character:FindFirstChild("ESP_Highlight") then
                    local highlight = player.Character.ESP_Highlight
                    highlight.FillColor = color
                    highlight.OutlineColor = color
                end
            end
        end
    end
})

VisualTab:CreateSection("🎨 Interface")

VisualTab:CreateButton({
    Name = "Fullbright",
    Callback = function()
        local Lighting = game:GetService("Lighting")
        Lighting.Brightness = 2
        Lighting.ClockTime = 14
        Lighting.FogEnd = 100000
        Lighting.GlobalShadows = false
        Lighting.OutdoorAmbient = Color3.fromRGB(128, 128, 128)
        
        Window:Notify({
            Title = "Fullbright",
            Content = "Iluminação máxima ativada!",
            Type = "Success"
        })
    end
})

-- ======== ABA TELEPORTE ========
local TeleportTab = Window:CreateTab({
    Name = "Teleporte",
    Icon = "🚀"
})

TeleportTab:CreateSection("📍 Teleporte Rápido")

TeleportTab:CreateTextBox({
    Name = "Nome do Jogador",
    Placeholder = "Digite o nome do jogador...",
    Callback = function(text)
        local targetPlayer = nil
        for _, player in ipairs(Players:GetPlayers()) do
            if player.Name:lower():find(text:lower()) or player.DisplayName:lower():find(text:lower()) then
                targetPlayer = player
                break
            end
        end
        
        if targetPlayer and targetPlayer.Character and targetPlayer.Character:FindFirstChild("HumanoidRootPart") then
            Character.HumanoidRootPart.CFrame = targetPlayer.Character.HumanoidRootPart.CFrame
            Window:Notify({
                Title = "Teleporte",
                Content = "Teleportado para " .. targetPlayer.Name,
                Type = "Success"
            })
        else
            Window:Notify({
                Title = "Erro",
                Content = "Jogador não encontrado!",
                Type = "Error"
            })
        end
    end
})

TeleportTab:CreateDropdown({
    Name = "Teleporte para Jogador",
    Options = (function()
        local names = {}
        for _, player in ipairs(Players:GetPlayers()) do
            if player ~= Player then
                table.insert(names, player.Name)
            end
        end
        return names
    end)(),
    Default = "Selecione",
    Callback = function(option)
        local targetPlayer = Players:FindFirstChild(option)
        if targetPlayer and targetPlayer.Character and targetPlayer.Character:FindFirstChild("HumanoidRootPart") then
            Character.HumanoidRootPart.CFrame = targetPlayer.Character.HumanoidRootPart.CFrame
            Window:Notify({
                Title = "Teleporte",
                Content = "Teleportado para " .. option,
                Type = "Success"
            })
        end
    end
})

-- ======== ABA CONFIGURAÇÕES ========
local SettingsTab = Window:CreateTab({
    Name = "Configurações",
    Icon = "⚙️"
})

SettingsTab:CreateSection("🎮 Controles")

SettingsTab:CreateKeybind({
    Name = "Toggle UI",
    Default = Enum.KeyCode.RightShift,
    Callback = function()
        Main.Visible = not Main.Visible
    end
})

SettingsTab:CreateSection("🔧 Outras Opções")

SettingsTab:CreateButton({
    Name = "Resetar Personagem",
    Callback = function()
        Character.Humanoid.Health = 0
        Window:Notify({
            Title = "Reset",
            Content = "Personagem resetado!",
            Type = "Warning"
        })
    end
})

SettingsTab:CreateButton({
    Name = "Rejoin (Reconectar)",
    Callback = function()
        game:GetService("TeleportService"):TeleportToPlaceInstance(game.PlaceId, game.JobId, Player)
    end
})

-- ======== ABA INFO ========
local InfoTab = Window:CreateTab({
    Name = "Informações",
    Icon = "ℹ️"
})

InfoTab:CreateSection("📖 Sobre o Script")

InfoTab:CreateButton({
    Name = "Copiar Discord",
    Callback = function()
        setclipboard("discord.gg/exemplo")
        Window:Notify({
            Title = "Discord",
            Content = "Link copiado para área de transferência!",
            Type = "Success"
        })
    end
})

InfoTab:CreateButton({
    Name = "Créditos",
    Callback = function()
        Window:Notify({
            Title = "Créditos",
            Content = "Feito com Fluent UI Library",
            Type = "Info",
            Duration = 5
        })
    end
})

-- Loop para manter configurações
RunService.Heartbeat:Connect(function()
    if SpeedEnabled and Humanoid then
        Humanoid.WalkSpeed = WalkSpeed
    end
    
    if JumpPowerEnabled and Humanoid then
        Humanoid.JumpPower = JumpPower
    end
end)

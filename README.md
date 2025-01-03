-- Основной GUI
local player = game.Players.LocalPlayer
local screenGui = Instance.new("ScreenGui")
local frame = Instance.new("Frame")
local title = Instance.new("TextLabel")
local tabs = {} -- Таблица для вкладок

-- Настройка GUI
screenGui.Parent = player.PlayerGui
screenGui.Name = "HumanHub"

frame.Parent = screenGui
frame.Size = UDim2.new(0, 350, 0, 450)
frame.Position = UDim2.new(0, 20, 0, 20)
frame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
frame.BackgroundTransparency = 0.7
frame.BorderSizePixel = 0

title.Parent = frame
title.Size = UDim2.new(1, 0, 0, 40)
title.Text = "Human Hub"
title.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
title.TextColor3 = Color3.fromRGB(255, 255, 255)
title.Font = Enum.Font.SourceSansBold
title.TextScaled = true

-- Функция создания вкладок
local function createTab(name, position)
    local tabFrame = Instance.new("Frame")
    tabFrame.Size = UDim2.new(1, 0, 1, -60)
    tabFrame.Position = UDim2.new(0, 0, 0, 60)
    tabFrame.Visible = false
    tabFrame.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
    tabFrame.Parent = frame

    local button = Instance.new("TextButton")
    button.Text = name
    button.Size = UDim2.new(0, 110, 0, 30)
    button.Position = position
    button.Parent = frame
    button.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
    button.TextColor3 = Color3.fromRGB(255, 255, 255)

    button.MouseButton1Click:Connect(function()
        for _, tab in pairs(tabs) do
            tab.Visible = false
        end
        tabFrame.Visible = true
    end)

    table.insert(tabs, tabFrame)
    return tabFrame
end

-- Вкладка "Автофарм"
local autoFarmTab = createTab("Автофарм", UDim2.new(0, 10, 0, 10))
local autoFarmStatus = false

local autoFarmButton = Instance.new("TextButton")
autoFarmButton.Text = "Запустить автофарм"
autoFarmButton.Size = UDim2.new(0, 300, 0, 40)
autoFarmButton.Position = UDim2.new(0.5, -150, 0.1, 0)
autoFarmButton.Parent = autoFarmTab

autoFarmButton.MouseButton1Click:Connect(function()
    autoFarmStatus = not autoFarmStatus
    autoFarmButton.Text = autoFarmStatus and "Автофарм активен" or "Запустить автофарм"
    if autoFarmStatus then
        while autoFarmStatus do
            for _, toilet in pairs(workspace:GetChildren()) do
                if toilet:IsA("Model") and toilet:FindFirstChild("HumanoidRootPart") then
                    player.Character.HumanoidRootPart.CFrame = toilet.HumanoidRootPart.CFrame + Vector3.new(0, 5, 0)
                    wait(1)
                end
            end
            wait(0.5)
        end
    end
end)

-- Вкладка "Статистика"
local statsTab = createTab("Статистика", UDim2.new(0, 130, 0, 10))
local pointsLabel = Instance.new("TextLabel")
local moneyLabel = Instance.new("TextLabel")

pointsLabel.Text = "Поинты: Загрузка..."
pointsLabel.Size = UDim2.new(0, 300, 0, 30)
pointsLabel.Position = UDim2.new(0.5, -150, 0.1, 0)
pointsLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
pointsLabel.BackgroundTransparency = 1
pointsLabel.Font = Enum.Font.SourceSans
pointsLabel.TextScaled = true
pointsLabel.Parent = statsTab

moneyLabel.Text = "MoneyShop: Загрузка..."
moneyLabel.Size = UDim2.new(0, 300, 0, 30)
moneyLabel.Position = UDim2.new(0.5, -150, 0.2, 0)
moneyLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
moneyLabel.BackgroundTransparency = 1
moneyLabel.Font = Enum.Font.SourceSans
moneyLabel.TextScaled = true
moneyLabel.Parent = statsTab

game:GetService("RunService").RenderStepped:Connect(function()
    if player:FindFirstChild("leaderstats") then
        local points = player.leaderstats:FindFirstChild("Points") or {Value = "Н/Д"}
        local money = player.leaderstats:FindFirstChild("MoneyShop") or {Value = "Н/Д"}
        pointsLabel.Text = "Поинты: " .. points.Value
        moneyLabel.Text = "MoneyShop: " .. money.Value
    end
end)

-- Вкладка "Настройки"
local settingsTab = createTab("Настройки", UDim2.new(0, 250, 0, 10))
local closeButton = Instance.new("TextButton")

closeButton.Text = "Закрыть меню"
closeButton.Size = UDim2.new(0, 300, 0, 40)
closeButton.Position = UDim2.new(0.5, -150, 0.1, 0)
closeButton.Parent = settingsTab

closeButton.MouseButton1Click:Connect(function()
    screenGui.Enabled = false
end)

-- Установить начальную вкладку
tabs[1].Visible = true

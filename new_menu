local player = game.Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local humanoid = character:WaitForChild("Humanoid")
local userInputService = game:GetService("UserInputService")
local runService = game:GetService("RunService")

-- Создаем GUI
local screenGui = Instance.new("ScreenGui", player.PlayerGui)
screenGui.Name = "AdminMenu"

local frame = Instance.new("Frame", screenGui)
frame.Size = UDim2.new(0, 200, 0, 650) -- Начальный размер
frame.Position = UDim2.new(0.5, -150, 0.5, -300) -- Центрируем
frame.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
frame.Draggable = true
frame.Active = true
frame.ClipsDescendants = true -- Обрезаем содержимое, если оно выходит за пределы окна

-- Кнопка закрытия
local cancelButton = Instance.new("TextButton", frame)
cancelButton.Text = "Cancel"
cancelButton.Size = UDim2.new(0, 80, 0, 40)
cancelButton.Position = UDim2.new(1, -90, 0, 0)
cancelButton.BackgroundColor3 = Color3.fromRGB(200, 50, 50)
cancelButton.TextColor3 = Color3.new(1, 1, 1)
cancelButton.MouseButton1Click:Connect(function()
    screenGui:Destroy()
end)

-- Кнопка сворачивания меню
local toggleButton = Instance.new("TextButton", frame)
toggleButton.Text = "≡"
toggleButton.Size = UDim2.new(0, 40, 0, 40)
toggleButton.Position = UDim2.new(0, 0, 0, 0)
toggleButton.BackgroundColor3 = Color3.fromRGB(100, 100, 100)
toggleButton.TextColor3 = Color3.new(1, 1, 1)

-- Таблица для хранения всех кнопок
local buttons = {}

-- Функция для создания кнопок
function createButton(name, position, callback, initialState)
    local button = Instance.new("TextButton", frame)
    button.Text = name
    button.Size = UDim2.new(1, -20, 0, 40)
    button.Position = UDim2.new(0, 10, 0, position)
    button.BackgroundColor3 = initialState and Color3.fromRGB(0, 200, 0) or Color3.fromRGB(200, 50, 50)
    button.TextColor3 = Color3.new(1, 1, 1)
    button.MouseButton1Click:Connect(function()
        callback(button)
    end)
    table.insert(buttons, button) -- Добавляем кнопку в таблицу
    return button
end

-- Функция для скрытия/показа кнопок
local function toggleButtons(visible)
    for _, button in pairs(buttons) do
        button.Visible = visible
    end
end

-- Обработчик нажатия на кнопку сворачивания
toggleButton.MouseButton1Click:Connect(function()
    if frame.Size.Y.Offset == 40 then
        -- Разворачиваем меню
        frame.Size = UDim2.new(0, 300, 0, 600)
        toggleButtons(true) -- Показываем кнопки
    else
        -- Сворачиваем меню
        frame.Size = UDim2.new(0, 300, 0, 40)
        toggleButtons(false) -- Скрываем кнопки
    end
end)

-- Функция для изменения размера окна
local isResizing = false
local resizeStartPosition = Vector2.new(0, 0)
local resizeStartSize = Vector2.new(0, 0)

local resizeButton = Instance.new("TextButton", frame)
resizeButton.Text = "↔️"
resizeButton.Size = UDim2.new(0, 40, 0, 40)
resizeButton.Position = UDim2.new(1, -40, 1, -40)
resizeButton.BackgroundColor3 = Color3.fromRGB(100, 100, 100)
resizeButton.TextColor3 = Color3.new(1, 1, 1)

-- Обработчик начала изменения размера
resizeButton.MouseButton1Down:Connect(function()
    isResizing = true
    resizeStartPosition = userInputService:GetMouseLocation()
    resizeStartSize = Vector2.new(frame.AbsoluteSize.X, frame.AbsoluteSize.Y)
end)

-- Обработчик изменения размера
userInputService.InputChanged:Connect(function(input)
    if isResizing and input.UserInputType == Enum.UserInputType.MouseMovement then
        local currentMousePosition = userInputService:GetMouseLocation()
        local delta = currentMousePosition - resizeStartPosition
        local newSize = resizeStartSize + delta

        -- Устанавливаем минимальный и максимальный размер
        newSize = Vector2.new(
            math.clamp(newSize.X, 200, 800), -- Минимальная ширина: 200, максимальная: 800
            math.clamp(newSize.Y, 100, 1200) -- Минимальная высота: 100, максимальная: 1200
        )

        frame.Size = UDim2.new(0, newSize.X, 0, newSize.Y)
    end
end)

Koslart, [19.07.2025 15:01]
-- Обработчик окончания изменения размера
userInputService.InputEnded:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 then
        isResizing = false
    end
end)

-- Обработчик жестов для мобильных устройств
local touchStartPositions = {}
local touchStartSize = Vector2.new(0, 0)

userInputService.TouchStarted:Connect(function(touch)
    if #touchStartPositions < 2 then
        table.insert(touchStartPositions, touch.Position)
        if #touchStartPositions == 2 then
            touchStartSize = Vector2.new(frame.AbsoluteSize.X, frame.AbsoluteSize.Y)
        end
    end
end)

userInputService.TouchEnded:Connect(function(touch)
    touchStartPositions = {}
end)

userInputService.TouchMoved:Connect(function(touch)
    if #touchStartPositions == 2 then
        local currentPositions = {
            userInputService:GetTouchLocation(touchStartPositions[1].Key),
            userInputService:GetTouchLocation(touchStartPositions[2].Key)
        }

        local startDistance = (touchStartPositions[1] - touchStartPositions[2]).Magnitude
        local currentDistance = (currentPositions[1] - currentPositions[2]).Magnitude

        local scaleFactor = currentDistance / startDistance
        local newSize = touchStartSize * scaleFactor

        -- Устанавливаем минимальный и максимальный размер
        newSize = Vector2.new(
            math.clamp(newSize.X, 200, 800), -- Минимальная ширина: 200, максимальная: 800
            math.clamp(newSize.Y, 100, 1200) -- Минимальная высота: 100, максимальная: 1200
        )

        frame.Size = UDim2.new(0, newSize.X, 0, newSize.Y)
    end
end)


-- Tp Tool
createButton("Tp Tool", 50, function(button)
    local tool = Instance.new("Tool", player.Backpack)
    tool.Name = "Teleport Tool"
    tool.RequiresHandle = false
    tool.Activated:Connect(function()
        local mouse = player:GetMouse()
        if mouse.Target then
            character:SetPrimaryPartCFrame(CFrame.new(mouse.Hit.p))
        end
    end)
    button.BackgroundColor3 = Color3.fromRGB(0, 200, 0)
end, false)

-- Переключатель бесконечного прыжка
local infiniteJumpEnabled = false
local infiniteJumpButton = createButton("Infinite Jump", 100, function(button)
    infiniteJumpEnabled = not infiniteJumpEnabled
    button.BackgroundColor3 = infiniteJumpEnabled and Color3.fromRGB(0, 200, 0) or Color3.fromRGB(200, 50, 50)
end, false)

game:GetService("UserInputService").JumpRequest:Connect(function()
    if infiniteJumpEnabled then
        humanoid:ChangeState(Enum.HumanoidStateType.Jumping)
    end
end)

-- Функция для изменения скорости
createButton("Set Speed (Normall speed 16)", 150, function(button)
    createInputWindow("Введите Speed", function(value)
        if value and value > 0 then
            humanoid.WalkSpeed = value
            print("Speed установлен на: " .. value)
            button.BackgroundColor3 = Color3.fromRGB(0, 200, 0)
        else
            warn("Неверное значение для Speed! Значение должно быть больше 0.")
        end
    end)
end, false)

-- Кнопка для активации Infinite Yield
createButton("Infinite Yield", 200, function(button)
    -- Загрузка и выполнение Infinite Yield
    local infiniteYieldScript = "https://raw.githubusercontent.com/EdgeIY/infiniteyield/master/source"
    local success, message = pcall(function()
        loadstring(game:HttpGet(infiniteYieldScript))()
    end)
    
    if not success then
        warn("Ошибка при загрузке Infinite Yield: " .. message)
    else
        print("Infinite Yield успешно загружен!")
        button.BackgroundColor3 = Color3.fromRGB(0, 200, 0)
    end
end, false)

Koslart, [19.07.2025 15:01]
-- Кнопка "Tornado"
createButton("Tornado", 250, function(button)
    local success, message = pcall(function()
        loadstring(game:HttpGet("https://raw.githubusercontent.com/cnPthPiGon/CM9/refs/heads/main/Super%20Ring%20Part%20V6%20By%20lukas"))()
    end)
    
    if not success then
        warn("Ошибка при загрузке Tornado: " .. message)
    else
        print("Tornado успешно загружен!")
        button.BackgroundColor3 = Color3.fromRGB(0, 200, 0)
    end
end, false)

-- Кнопка "Telekiness"
createButton("Telekiness", 300, function(button)
    local success, message = pcall(function()
        loadstring(game:HttpGet("https://rawscripts.net/raw/Universal-Script-Fe-Telekinesis-V5-21542"))()
    end)
    
    if not success then
        warn("Ошибка при загрузке Telekiness: " .. message)
    else
        print("Telekiness успешно загружен!")
        button.BackgroundColor3 = Color3.fromRGB(0, 200, 0)
    end
end, false)

-- Кнопка "Knife Mode"
createButton("Knife Mode", 350, function(button)
    local success, message = pcall(function()
        loadstring(game:HttpGet("https://rawscripts.net/raw/Universal-Script-Grab-knife-v4-24753"))()
    end)
    
    if not success then
        warn("Ошибка при загрузке Knife Mode: " .. message)
    else
        print("Knife Mode успешно загружен!")
        button.BackgroundColor3 = Color3.fromRGB(0, 200, 0)
    end
end, false)

-- Кнопка "FaceTroll"
createButton("FaceTroll", 400, function(button)
    local success, message = pcall(function()
        loadstring(game:HttpGet("https://raw.githubusercontent.com/AKadminlol/Facefuck/refs/heads/main/CreditsbyAK"))()
    end)
    
    if not success then
        warn("Ошибка при загрузке FaceTroll: " .. message)
    else
        print("FaceTroll успешно загружен!")
        button.BackgroundColor3 = Color3.fromRGB(0, 200, 0)
    end
end, false)

-- Кнопка "Jerk off"
createButton("Jerk off", 450, function(button)
    local success1, message1 = pcall(function()
        loadstring(game:HttpGet("https://pastefy.app/wa3v2Vgm/raw"))()
    end)
    
    local success2, message2 = pcall(function()
        loadstring(game:HttpGet("https://pastefy.app/YZoglOyJ/raw"))()
    end)
    
    if not success1 then
        warn("Ошибка при загрузке первого скрипта Jerk off: " .. message1)
    else
        print("Первый скрипт Jerk off успешно загружен!")
    end
    
    if not success2 then
        warn("Ошибка при загрузке второго скрипта Jerk off: " .. message2)
    else
        print("Второй скрипт Jerk off успешно загружен!")
    end
    
    if success1 or success2 then
        button.BackgroundColor3 = Color3.fromRGB(0, 200, 0)
    end
end, false)

-- Lua Executor
createButton("Lua Executor", 500, function(button)
    createLuaExecutorWindow("Введите Lua-код", function(code)
        local func, err = loadstring(code)
        if func then
            local success, result = pcall(func)
            if not success then
                warn("Ошибка при выполнении скрипта: " .. result)
            end
        else
            warn("Ошибка в скрипте: " .. err)
        end
    end)
end, false)

-- Teleport To
createButton("Teleport To", 550, function(button)
    createTeleportToWindow()
end, false)

-- Clear Inventory
createButton("Clear Inventory", 600, function(button)
    createClearInventoryWindow()
end, false)

Koslart, [19.07.2025 15:01]
-- Функция для создания окна ввода Lua-кода
function createLuaExecutorWindow(titleText, callback)
    local inputGui = Instance.new("ScreenGui", player.PlayerGui)
    local inputFrame = Instance.new("Frame", inputGui)
    inputFrame.Size = UDim2.new(0, 300, 0, 200)
    inputFrame.Position = UDim2.new(0.5, -150, 0.5, -100)
    inputFrame.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
    inputFrame.Draggable = true
    inputFrame.Active = true
    
    local title = Instance.new("TextLabel", inputFrame)
    title.Text = titleText
    title.Size = UDim2.new(1, 0, 0.3, 0)
    title.TextColor3 = Color3.new(1, 1, 1)
    
    local inputBox = Instance.new("TextBox", inputFrame)
    inputBox.Size = UDim2.new(1, 0, 0.7, 0)
    inputBox.Position = UDim2.new(0, 0, 0.3, 0)
    inputBox.Text = "-- Введите Lua-код сюда"
    inputBox.TextScaled = true
    inputBox.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
    inputBox.TextColor3 = Color3.new(1, 1, 1)

    local execButton = Instance.new("TextButton", inputFrame)
    execButton.Size = UDim2.new(0.5, 0, 0.3, 0)
    execButton.Position = UDim2.new(0, 0, 0.7, 0)
    execButton.Text = "Выполнить"
    execButton.BackgroundColor3 = Color3.fromRGB(0, 200, 0)
    execButton.TextColor3 = Color3.new(1, 1, 1)
    execButton.MouseButton1Click:Connect(function()
        callback(inputBox.Text)
        inputGui:Destroy()
    end)
    
    local closeButton = Instance.new("TextButton", inputFrame)
    closeButton.Size = UDim2.new(0.5, 0, 0.3, 0)
    closeButton.Position = UDim2.new(0.5, 0, 0.7, 0)
    closeButton.Text = "Cancel"
    closeButton.BackgroundColor3 = Color3.fromRGB(200, 50, 50)
    closeButton.TextColor3 = Color3.new(1, 1, 1)
    closeButton.MouseButton1Click:Connect(function()
        inputGui:Destroy()
    end)
end

-- Функция для создания окна Teleport To
function createTeleportToWindow()
    local teleportGui = Instance.new("ScreenGui", player.PlayerGui)
    teleportGui.Name = "TeleportToMenu"

    local teleportFrame = Instance.new("Frame", teleportGui)
    teleportFrame.Size = UDim2.new(0, 200, 0, 300)
    teleportFrame.Position = UDim2.new(0.5, -100, 0.5, -150)
    teleportFrame.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
    teleportFrame.Draggable = true
    teleportFrame.Active = true
    
    local title = Instance.new("TextLabel", teleportFrame)
    title.Text = "Teleport To"
    title.Size = UDim2.new(1, 0, 0.1, 0)
    title.TextColor3 = Color3.new(1, 1, 1)
    
    local playerList = Instance.new("ScrollingFrame", teleportFrame)
    playerList.Size = UDim2.new(1, 0, 0.8, 0)
    playerList.Position = UDim2.new(0, 0, 0.1, 0)
    playerList.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
    playerList.CanvasSize = UDim2.new(0, 0, 0, 0) -- Автоматически изменится при добавлении игроков

    local closeButton = Instance.new("TextButton", teleportFrame)
    closeButton.Size = UDim2.new(1, 0, 0.1, 0)
    closeButton.Position = UDim2.new(0, 0, 0.9, 0)
    closeButton.Text = "Cancel"
    closeButton.BackgroundColor3 = Color3.fromRGB(200, 50, 50)
    closeButton.TextColor3 = Color3.new(1, 1, 1)
    closeButton.MouseButton1Click:Connect(function()
        teleportGui:Destroy()
    end)

    -- Добавляем ники всех игроков
    local players = game.Players:GetPlayers()
    local buttonHeight = 30
    playerList.CanvasSize = UDim2.new(0, 0, 0, #players * buttonHeight)

Koslart, [19.07.2025 15:01]
for i, plr in ipairs(players) do
        if plr ~= player then
            local playerButton = Instance.new("TextButton", playerList)
            playerButton.Text = plr.Name
            playerButton.Size = UDim2.new(1, 0, 0, buttonHeight)
            playerButton.Position = UDim2.new(0, 0, 0, (i - 1) * buttonHeight)
            playerButton.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
            playerButton.TextColor3 = Color3.new(1, 1, 1)
            playerButton.MouseButton1Click:Connect(function()
                -- Телепортируемся к игроку
                local targetCharacter = plr.Character
                if targetCharacter and targetCharacter:FindFirstChild("HumanoidRootPart") then
                    character:SetPrimaryPartCFrame(targetCharacter.HumanoidRootPart.CFrame)
                else
                    warn("Не удалось телепортироваться к игроку: " .. plr.Name)
                end
                teleportGui:Destroy()
            end)
        end
    end
end

-- Функция для создания окна ввода чисел
function createInputWindow(titleText, callback)
    local inputGui = Instance.new("ScreenGui", player.PlayerGui)
    local inputFrame = Instance.new("Frame", inputGui)
    inputFrame.Size = UDim2.new(0, 200, 0, 100)
    inputFrame.Position = UDim2.new(0.5, -100, 0.5, -50)
    inputFrame.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
    inputFrame.Draggable = true
    inputFrame.Active = true
    
    local title = Instance.new("TextLabel", inputFrame)
    title.Text = titleText
    title.Size = UDim2.new(1, 0, 0.3, 0)
    title.TextColor3 = Color3.new(1, 1, 1)
    
    local inputBox = Instance.new("TextBox", inputFrame)
    inputBox.Size = UDim2.new(1, 0, 0.4, 0)
    inputBox.Position = UDim2.new(0, 0, 0.3, 0)
    
    local confirmButton = Instance.new("TextButton", inputFrame)
    confirmButton.Size = UDim2.new(0.5, 0, 0.3, 0)
    confirmButton.Position = UDim2.new(0, 0, 0.7, 0)
    confirmButton.Text = "OK"
    confirmButton.MouseButton1Click:Connect(function()
        callback(tonumber(inputBox.Text))
        inputGui:Destroy()
    end)
    
    local closeButton = Instance.new("TextButton", inputFrame)
    closeButton.Size = UDim2.new(0.5, 0, 0.3, 0)
    closeButton.Position = UDim2.new(0.5, 0, 0.7, 0)
    closeButton.Text = "X"
    closeButton.MouseButton1Click:Connect(function()
        inputGui:Destroy()
    end)
end

-- Функция для создания окна подтверждения очистки инвентаря
function createClearInventoryWindow()
    local clearInventoryGui = Instance.new("ScreenGui", player.PlayerGui)
    clearInventoryGui.Name = "ClearInventoryMenu"

Koslart, [19.07.2025 15:01]
local clearInventoryFrame = Instance.new("Frame", clearInventoryGui)
    clearInventoryFrame.Size = UDim2.new(0, 200, 0, 100)
    clearInventoryFrame.Position = UDim2.new(0.5, -100, 0.5, -50)
    clearInventoryFrame.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
    clearInventoryFrame.Draggable = true
    clearInventoryFrame.Active = true
    
    local title = Instance.new("TextLabel", clearInventoryFrame)
    title.Text = "Точно?"
    title.Size = UDim2.new(1, 0, 0.3, 0)
    title.TextColor3 = Color3.new(1, 1, 1)
    
    local confirmButton = Instance.new("TextButton", clearInventoryFrame)
    confirmButton.Size = UDim2.new(0.5, 0, 0.3, 0)
    confirmButton.Position = UDim2.new(0, 0, 0.7, 0)
    confirmButton.Text = "✓"
    confirmButton.BackgroundColor3 = Color3.fromRGB(0, 200, 0)
    confirmButton.TextColor3 = Color3.new(1, 1, 1)
    confirmButton.MouseButton1Click:Connect(function()
        -- Очистка инвентаря
        local backpack = player:FindFirstChild("Backpack")
        if backpack then
            for _, item in ipairs(backpack:GetChildren()) do
                item:Destroy()
            end
        end
        clearInventoryGui:Destroy()
    end)
    
    local closeButton = Instance.new("TextButton", clearInventoryFrame)
    closeButton.Size = UDim2.new(0.5, 0, 0.3, 0)
    closeButton.Position = UDim2.new(0.5, 0, 0.7, 0)
    closeButton.Text = "X"
    closeButton.BackgroundColor3 = Color3.fromRGB(200, 50, 50)
    closeButton.TextColor3 = Color3.new(1, 1, 1)
    closeButton.MouseButton1Click:Connect(function()
        clearInventoryGui:Destroy()
    end)
end

-- Обработчик смерти игрока
local function onCharacterDeath()
    -- Проверяем, существует ли ScreenGui
    if not player.PlayerGui:FindFirstChild("AdminMenu") then
        -- Если нет, создаем его заново
        screenGui = Instance.new("ScreenGui", player.PlayerGui)
        screenGui.Name = "AdminMenu"
        -- Здесь нужно добавить код для повторного создания всех элементов GUI
    end
end

-- Подключаем обработчик смерти
humanoid.Died:Connect(onCharacterDeath)

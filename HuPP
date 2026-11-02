--// MHUB - Personajes / Universal Edition (Delta Executor)
--// Funciones: Fly, ESP, Noclip, WalkSpeed, JumpPower

local Players = game:GetService("Players")
local UserInputService = game:GetService("UserInputService")
local RunService = game:GetService("RunService")
local TweenService = game:GetService("TweenService")

--// Obtener el padre de la GUI de forma segura para Delta
local function getGuiParent()
    local ok, result = pcall(function() return gethui() end)
    if ok and result then return result end
    local ok2, coreGui = pcall(function() return game:GetService("CoreGui") end)
    if ok2 and coreGui then return coreGui end
    return Players.LocalPlayer:WaitForChild("PlayerGui")
end

local guiParent = getGuiParent()

--// Destruir versión anterior
for _, child in ipairs(guiParent:GetChildren()) do
    if child.Name == "MHUB_Universal" then child:Destroy() end
end

--// Crear ScreenGui
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "MHUB_Universal"
screenGui.ResetOnSpawn = false
screenGui.Parent = guiParent

--// Botón flotante (Móvil)
local toggleButton = Instance.new("TextButton")
toggleButton.Name = "ToggleButton"
toggleButton.Size = UDim2.new(0, 60, 0, 60)
toggleButton.Position = UDim2.new(1, -80, 0, 20)
toggleButton.BackgroundColor3 = Color3.fromRGB(138, 43, 226)
toggleButton.TextColor3 = Color3.fromRGB(255, 255, 255)
toggleButton.Text = "MHUB\nPJ"
toggleButton.Font = Enum.Font.GothamBold
toggleButton.TextSize = 14
toggleButton.Parent = screenGui
Instance.new("UICorner", toggleButton).CornerRadius = UDim.new(1, 0)

--// Ventana Principal
local mainFrame = Instance.new("Frame")
mainFrame.Name = "MainFrame"
mainFrame.Size = UDim2.new(0, 450, 0, 350)
mainFrame.Position = UDim2.new(0.5, -225, 0.5, -175)
mainFrame.BackgroundColor3 = Color3.fromRGB(15, 15, 20)
mainFrame.BorderSizePixel = 0
mainFrame.Visible = false
mainFrame.Parent = screenGui
Instance.new("UICorner", mainFrame).CornerRadius = UDim.new(0, 12)

--// Barra Superior
local topBar = Instance.new("Frame")
topBar.Name = "TopBar"
topBar.Size = UDim2.new(1, 0, 0, 45)
topBar.BackgroundColor3 = Color3.fromRGB(25, 25, 35)
topBar.Parent = mainFrame
Instance.new("UICorner", topBar).CornerRadius = UDim.new(0, 12)

local titleLabel = Instance.new("TextLabel")
titleLabel.Size = UDim2.new(1, -80, 1, 0)
titleLabel.Position = UDim2.new(0, 15, 0, 0)
titleLabel.BackgroundTransparency = 1
titleLabel.Text = "MHUB | Personajes"
titleLabel.TextColor3 = Color3.fromRGB(200, 150, 255)
titleLabel.Font = Enum.Font.GothamBold
titleLabel.TextSize = 16
titleLabel.TextXAlignment = Enum.TextXAlignment.Left
titleLabel.Parent = topBar

--// Botón Cerrar
local closeButton = Instance.new("TextButton")
closeButton.Size = UDim2.new(0, 30, 0, 30)
closeButton.Position = UDim2.new(1, -38, 0, 7)
closeButton.BackgroundColor3 = Color3.fromRGB(200, 50, 50)
closeButton.Text = "X"
closeButton.TextColor3 = Color3.new(1, 1, 1)
closeButton.Font = Enum.Font.GothamBold
closeButton.Parent = topBar
Instance.new("UICorner", closeButton).CornerRadius = UDim.new(0, 8)

--// Contenedor de Scripts
local contentFrame = Instance.new("ScrollingFrame")
contentFrame.Name = "Content"
contentFrame.Size = UDim2.new(1, -20, 1, -60)
contentFrame.Position = UDim2.new(0, 10, 0, 50)
contentFrame.BackgroundTransparency = 1
contentFrame.BorderSizePixel = 0
contentFrame.ScrollBarThickness = 6
contentFrame.CanvasSize = UDim2.new(0, 0, 0, 0)
contentFrame.Parent = mainFrame

local contentLayout = Instance.new("UIListLayout")
contentLayout.Padding = UDim.new(0, 8)
contentLayout.SortOrder = Enum.SortOrder.LayoutOrder
contentLayout.Parent = contentFrame

--// Funciones de Arrastre
local dragging, dragInput, dragStart, startPos
local function updateDrag(input)
    local delta = input.Position - dragStart
    mainFrame.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
end
topBar.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
        dragging = true
        dragStart = input.Position
        startPos = mainFrame.Position
        input.Changed:Connect(function()
            if input.UserInputState == Enum.UserInputState.End then dragging = false end
        end)
    end
end)
topBar.InputChanged:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch then dragInput = input end
end)
UserInputService.InputChanged:Connect(function(input)
    if input == dragInput and dragging then updateDrag(input) end
end)

--// ==========================================
--// FUNCIONES DE PERSONAJE (Universales)
--// ==========================================

-- 1. Fly (Volar)
local flyEnabled = false
local flySpeed = 50
local flyConnection
local function toggleFly()
    flyEnabled = not flyEnabled
    local char = Players.LocalPlayer.Character
    if not char then return end
    local hrp = char:FindFirstChild("HumanoidRootPart")
    local hum = char:FindFirstChildOfClass("Humanoid")
    
    if flyEnabled and hrp and hum then
        hum.PlatformStand = true
        local bodyVelocity = Instance.new("BodyVelocity")
        bodyVelocity.Name = "MHUB_Fly"
        bodyVelocity.MaxForce = Vector3.new(math.huge, math.huge, math.huge)
        bodyVelocity.Velocity = Vector3.new(0, 0, 0)
        bodyVelocity.Parent = hrp
        
        flyConnection = RunService.RenderStepped:Connect(function()
            local camera = workspace.CurrentCamera
            local moveDir = Vector3.new()
            if UserInputService:IsKeyDown(Enum.KeyCode.W) then moveDir = moveDir + camera.CFrame.LookVector end
            if UserInputService:IsKeyDown(Enum.KeyCode.S) then moveDir = moveDir - camera.CFrame.LookVector end
            if UserInputService:IsKeyDown(Enum.KeyCode.A) then moveDir = moveDir - camera.CFrame.RightVector end
            if UserInputService:IsKeyDown(Enum.KeyCode.D) then moveDir = moveDir + camera.CFrame.RightVector end
            if UserInputService:IsKeyDown(Enum.KeyCode.Space) then moveDir = moveDir + Vector3.new(0, 1, 0) end
            if UserInputService:IsKeyDown(Enum.KeyCode.LeftShift) then moveDir = moveDir - Vector3.new(0, 1, 0) end
            
            if moveDir.Magnitude > 0 then
                bodyVelocity.Velocity = moveDir.Unit * flySpeed
            else
                bodyVelocity.Velocity = Vector3.new(0, 0, 0)
            end
        end)
    else
        if flyConnection then flyConnection:Disconnect() end
        if hrp then
            local bv = hrp:FindFirstChild("MHUB_Fly")
            if bv then bv:Destroy() end
        end
        if hum then hum.PlatformStand = false end
    end
end

-- 2. ESP (Resaltar Jugadores)
local espEnabled = false
local espTable = {}
local function toggleESP()
    espEnabled = not espEnabled
    for _, player in ipairs(Players:GetPlayers()) do
        if player ~= Players.LocalPlayer then
            if espEnabled then
                local char = player.Character
                if char then
                    local highlight = Instance.new("Highlight")
                    highlight.Name = "MHUB_ESP"
                    highlight.Adornee = char
                    highlight.FillColor = Color3.fromRGB(255, 0, 255)
                    highlight.OutlineColor = Color3.fromRGB(255, 255, 255)
                    highlight.Parent = char
                    espTable[player] = highlight
                end
            else
                if espTable[player] then
                    espTable[player]:Destroy()
                    espTable[player] = nil
                end
            end
        end
    end
end

-- 3. Noclip (Atravesar paredes)
local noclipEnabled = false
local noclipConnection = RunService.Stepped:Connect(function()
    if noclipEnabled then
        local char = Players.LocalPlayer.Character
        if char then
            for _, part in ipairs(char:GetDescendants()) do
                if part:IsA("BasePart") then
                    part.CanCollide = false
                end
            end
        end
    end
end)

-- 4. Cambiar Velocidad / Salto
local function setWalkSpeed(speed)
    local hum = Players.LocalPlayer.Character and Players.LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
    if hum then hum.WalkSpeed = speed end
end
local function setJumpPower(power)
    local hum = Players.LocalPlayer.Character and Players.LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
    if hum then hum.JumpPower = power end
end

--// Función para crear botones
local function createButton(name, callback)
    local btn = Instance.new("TextButton")
    btn.Size = UDim2.new(1, -10, 0, 40)
    btn.BackgroundColor3 = Color3.fromRGB(35, 35, 50)
    btn.Text = name
    btn.TextColor3 = Color3.fromRGB(255, 255, 255)
    btn.Font = Enum.Font.Gotham
    btn.TextSize = 13
    btn.Parent = contentFrame
    Instance.new("UICorner", btn).CornerRadius = UDim.new(0, 6)
    
    btn.MouseButton1Click:Connect(function()
        callback(btn)
    end)
end

--// ==========================================
--// CREAR BOTONES EN LA INTERFAZ
--// ==========================================

createButton("Fly (Volar) - OFF", function(btn)
    toggleFly()
    btn.Text = "Fly (Volar) - " .. (flyEnabled and "ON" or "OFF")
end)

createButton("ESP (Ver Jugadores) - OFF", function(btn)
    toggleESP()
    btn.Text = "ESP (Ver Jugadores) - " .. (espEnabled and "ON" or "OFF")
end)

createButton("Noclip (Atravesar) - OFF", function(btn)
    noclipEnabled = not noclipEnabled
    btn.Text = "Noclip (Atravesar) - " .. (noclipEnabled and "ON" or "OFF")
end)

createButton("Velocidad x2 (WalkSpeed 32)", function()
    setWalkSpeed(32)
end)

createButton("Salto Alto (JumpPower 100)", function()
    setJumpPower(100)
end)

--// Botón para Guest 666 (Requiere Script Externo)
createButton("Cargar Guest 666 (Script Externo)", function()
    -- Reemplaza esta URL con el enlace raw del script de Guest 666 que encuentres
    local guestUrl = "https://raw.githubusercontent.com/TU_USUARIO/TU_REPO/main/guest666.lua"
    local success, err = pcall(function()
        loadstring(game:HttpGet(guestUrl))()
    end)
    if not success then
        warn("MHUB: No se pudo cargar Guest 666. Verifica el enlace.")
    end
end)

--// Actualizar tamaño del Scroll
contentFrame.CanvasSize = UDim2.new(0, 0, 0, contentLayout.AbsoluteContentSize.Y + 20)

--// Abrir/Cerrar
local function toggleUI()
    mainFrame.Visible = not mainFrame.Visible
end
toggleButton.MouseButton1Click:Connect(toggleUI)
closeButton.MouseButton1Click:Connect(toggleUI)

print("MHUB Universal cargado.")

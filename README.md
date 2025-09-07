local Players = game:GetService("Players")
local TweenService = game:GetService("TweenService")
local UserInputService = game:GetService("UserInputService")
local HttpService = game:GetService("HttpService")
local TeleportService = game:GetService("TeleportService")
local ReplicatedStorage = game:GetService("ReplicatedStorage")

local player = Players.LocalPlayer
local playerGui = player:WaitForChild("PlayerGui")

-- Detectar executor real
local function detectRealExecutor()
    local executors = {
        {"Synapse", "syn", "SynapseX"},
        {"ScriptWare", "sw", "ScriptWare"},
        {"ProtoSmasher", "ps", "ProtoSmasher"},
        {"Krnl", "krnl", "Krnl"},
        {"Fluxus", "fluxus", "Fluxus"},
        {"Delta", "delta", "Delta"},
        {"Oxygen", "oxygen", "Oxygen U"},
        {"Electron", "electron", "Electron"},
        {"Comet", "comet", "Comet"},
        {"Codex", "codex", "Codex"},
        {"Arceus X", "arceus", "Arceus X"},
        {"Solara", "solara", "Solara"},
        {"Calamari", "calamari", "Calamari"},
        {"Valyse", "valyse", "Valyse"},
        {"Furk", "furk", "Furk Ultra"},
        {"Hydrogen", "hydrogen", "Hydrogen"},
        {"Elex", "elex", "Elex"},
        {"Trigon", "trigon", "Trigon Evo"},
        {"Node", "node", "Node"},
        {"Temple", "temple", "Temple"}
    }
    
    local detected = "Unknown"
    
    for _, executor in ipairs(executors) do
        local success, result = pcall(function()
            return identifyexecutor and identifyexecutor() or (getexecutorname and getexecutorname()) or (executor and executor())
        end)
        
        if success and type(result) == "string" then
            for _, name in ipairs(executor) do
                if string.find(string.lower(result), string.lower(name)) then
                    detected = executor[3] or executor[1]
                    break
                end
            end
        end
    end
    
    -- Fallback para detecção por funções específicas
    if detected == "Unknown" then
        if pcall(function() return syn and syn.protect_gui end) then
            detected = "Synapse X"
        elseif pcall(function() return secure_load end) then
            detected = "ScriptWare"
        elseif pcall(function() return is_protosmasher_closure end) then
            detected = "ProtoSmasher"
        elseif pcall(function() return krnl and krnl.request end) then
            detected = "Krnl"
        elseif pcall(function() return fluxus and fluxus.request end) then
            detected = "Fluxus"
        end
    end
    
    return detected
end

-- Criar a ScreenGui principal
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "BenjamimHub"
screenGui.Parent = playerGui
screenGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling

-- TELA DE CARREGAMENTO
local loadingFrame = Instance.new("Frame")
loadingFrame.Name = "LoadingFrame"
loadingFrame.Size = UDim2.new(1, 0, 1, 0)
loadingFrame.Position = UDim2.new(0, 0, 0, 0)
loadingFrame.BackgroundColor3 = Color3.fromRGB(20, 20, 30)
loadingFrame.BorderSizePixel = 0
loadingFrame.ZIndex = 10
loadingFrame.Parent = screenGui

local loadingUICorner = Instance.new("UICorner")
loadingUICorner.CornerRadius = UDim.new(0, 0)
loadingUICorner.Parent = loadingFrame

local titleLabel = Instance.new("TextLabel")
titleLabel.Name = "TitleLabel"
titleLabel.Size = UDim2.new(0, 300, 0, 60)
titleLabel.Position = UDim2.new(0.5, -150, 0.4, -30)
titleLabel.BackgroundTransparency = 1
titleLabel.Text = "Benjamim Hub"
titleLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
titleLabel.TextSize = 32
titleLabel.Font = Enum.Font.GothamBold
titleLabel.ZIndex = 11
titleLabel.Parent = loadingFrame

local loadingBarBackground = Instance.new("Frame")
loadingBarBackground.Name = "LoadingBarBackground"
loadingBarBackground.Size = UDim2.new(0, 300, 0, 20)
loadingBarBackground.Position = UDim2.new(0.5, -150, 0.5, 0)
loadingBarBackground.BackgroundColor3 = Color3.fromRGB(60, 60, 80)
loadingBarBackground.BorderSizePixel = 0
loadingBarBackground.ZIndex = 11
loadingBarBackground.Parent = loadingFrame

local loadingBarBackgroundCorner = Instance.new("UICorner")
loadingBarBackgroundCorner.CornerRadius = UDim.new(0, 10)
loadingBarBackgroundCorner.Parent = loadingBarBackground

local loadingBar = Instance.new("Frame")
loadingBar.Name = "LoadingBar"
loadingBar.Size = UDim2.new(0, 0, 1, 0)
loadingBar.Position = UDim2.new(0, 0, 0, 0)
loadingBar.BackgroundColor3 = Color3.fromRGB(0, 170, 255)
loadingBar.BorderSizePixel = 0
loadingBar.ZIndex = 12
loadingBar.Parent = loadingBarBackground

local loadingBarCorner = Instance.new("UICorner")
loadingBarCorner.CornerRadius = UDim.new(0, 10)
loadingBarCorner.Parent = loadingBar

local loadingText = Instance.new("TextLabel")
loadingText.Name = "LoadingText"
loadingText.Size = UDim2.new(0, 200, 0, 30)
loadingText.Position = UDim2.new(0.5, -100, 0.5, 30)
loadingText.BackgroundTransparency = 1
loadingText.Text = "Carregando..."
loadingText.TextColor3 = Color3.fromRGB(200, 200, 200)
loadingText.TextSize = 18
loadingText.Font = Enum.Font.Gotham
loadingText.ZIndex = 11
loadingText.Parent = loadingFrame

-- HUB PRINCIPAL
local hubFrame = Instance.new("Frame")
hubFrame.Name = "HubFrame"
hubFrame.Size = UDim2.new(0, 450, 0, 550)
hubFrame.Position = UDim2.new(0.5, -225, 0.5, -275)
hubFrame.BackgroundColor3 = Color3.fromRGB(30, 60, 120)
hubFrame.BorderSizePixel = 0
hubFrame.Visible = false
hubFrame.ZIndex = 5
hubFrame.Parent = screenGui

local hubCorner = Instance.new("UICorner")
hubCorner.CornerRadius = UDim.new(0, 12)
hubCorner.Parent = hubFrame

local topBar = Instance.new("Frame")
topBar.Name = "TopBar"
topBar.Size = UDim2.new(1, 0, 0, 40)
topBar.Position = UDim2.new(0, 0, 0, 0)
topBar.BackgroundColor3 = Color3.fromRGB(20, 50, 100)
topBar.BorderSizePixel = 0
topBar.ZIndex = 6
topBar.Parent = hubFrame

local topBarCorner = Instance.new("UICorner")
topBarCorner.CornerRadius = UDim.new(0, 12)
topBarCorner.Parent = topBar

local hubTitle = Instance.new("TextLabel")
hubTitle.Name = "HubTitle"
hubTitle.Size = UDim2.new(0, 200, 0, 40)
hubTitle.Position = UDim2.new(0.5, -100, 0, 0)
hubTitle.BackgroundTransparency = 1
hubTitle.Text = "Benjamim Hub"
hubTitle.TextColor3 = Color3.fromRGB(255, 255, 255)
hubTitle.TextSize = 20
hubTitle.Font = Enum.Font.GothamBold
hubTitle.ZIndex = 7
hubTitle.Parent = topBar

local closeButton = Instance.new("TextButton")
closeButton.Name = "CloseButton"
closeButton.Size = UDim2.new(0, 40, 0, 40)
closeButton.Position = UDim2.new(1, -40, 0, 0)
closeButton.BackgroundColor3 = Color3.fromRGB(200, 50, 50)
closeButton.BorderSizePixel = 0
closeButton.Text = "X"
closeButton.TextColor3 = Color3.fromRGB(255, 255, 255)
closeButton.TextSize = 18
closeButton.Font = Enum.Font.GothamBold
closeButton.ZIndex = 7
closeButton.Parent = topBar

local closeButtonCorner = Instance.new("UICorner")
closeButtonCorner.CornerRadius = UDim.new(0, 12)
closeButtonCorner.Parent = closeButton

local openButton = Instance.new("TextButton")
openButton.Name = "OpenButton"
openButton.Size = UDim2.new(0, 50, 0, 50)
openButton.Position = UDim2.new(0, 20, 0, 20)
openButton.BackgroundColor3 = Color3.fromRGB(0, 100, 200)
openButton.BorderSizePixel = 0
openButton.Text = "☰"
openButton.TextColor3 = Color3.fromRGB(255, 255, 255)
openButton.TextSize = 24
openButton.Visible = false
openButton.ZIndex = 5
openButton.Parent = screenGui

local openButtonCorner = Instance.new("UICorner")
openButtonCorner.CornerRadius = UDim.new(0, 12)
openButtonCorner.Parent = openButton

-- Conteúdo do Hub
local scrollFrame = Instance.new("ScrollingFrame")
scrollFrame.Name = "ScrollFrame"
scrollFrame.Size = UDim2.new(1, -20, 1, -100)
scrollFrame.Position = UDim2.new(0, 10, 0, 50)
scrollFrame.BackgroundTransparency = 1
scrollFrame.BorderSizePixel = 0
scrollFrame.ScrollBarThickness = 5
scrollFrame.CanvasSize = UDim2.new(0, 0, 0, 0)
scrollFrame.ZIndex = 6
scrollFrame.Parent = hubFrame

local uiListLayout = Instance.new("UIListLayout")
uiListLayout.Padding = UDim.new(0, 10)
uiListLayout.Parent = scrollFrame

-- Área de informações do executor
local executorFrame = Instance.new("Frame")
executorFrame.Name = "ExecutorFrame"
executorFrame.Size = UDim2.new(1, -20, 0, 40)
executorFrame.Position = UDim2.new(0, 10, 1, -50)
executorFrame.BackgroundColor3 = Color3.fromRGB(40, 80, 160)
executorFrame.BorderSizePixel = 0
executorFrame.ZIndex = 6
executorFrame.Parent = hubFrame

local executorCorner = Instance.new("UICorner")
executorCorner.CornerRadius = UDim.new(0, 8)
executorCorner.Parent = executorFrame

local executorText = Instance.new("TextLabel")
executorText.Name = "ExecutorText"
executorText.Size = UDim2.new(1, 0, 1, 0)
executorText.Position = UDim2.new(0, 0, 0, 0)
executorText.BackgroundTransparency = 1
executorText.Text = "Executor: Detectando..."
executorText.TextColor3 = Color3.fromRGB(255, 255, 255)
executorText.TextSize = 16
executorText.Font = Enum.Font.Gotham
executorText.ZIndex = 7
executorText.Parent = executorFrame

-- Função para simular carregamento
local function simulateLoading()
    for i = 1, 100 do
        loadingBar.Size = UDim2.new(0, i * 3, 1, 0)
        loadingText.Text = "Carregando... " .. i .. "%"
        wait(0.03)
    end
    
    wait(0.5)
    
    -- Animação de desaparecimento
    local tweenInfo = TweenInfo.new(0.5, Enum.EasingStyle.Quad, Enum.EasingDirection.Out)
    local tween = TweenService:Create(loadingFrame, tweenInfo, {BackgroundTransparency = 1})
    tween:Play()
    
    tween.Completed:Connect(function()
        loadingFrame.Visible = false
        openButton.Visible = true
    end)
end

-- Funções para abrir e fechar o hub
local function openHub()
    hubFrame.Visible = true
    openButton.Visible = false
    
    -- Animação de entrada
    hubFrame.Position = UDim2.new(0.5, -225, 0.5, -275)
    local tweenInfo = TweenInfo.new(0.3, Enum.EasingStyle.Back, Enum.EasingDirection.Out, 0, false, 0)
    local tween = TweenService:Create(hubFrame, tweenInfo, {Position = UDim2.new(0.5, -225, 0.5, -275)})
    tween:Play()
end

local function closeHub()
    -- Animação de saída
    local tweenInfo = TweenInfo.new(0.3, Enum.EasingStyle.Back, Enum.EasingDirection.In, 0, false, 0)
    local tween = TweenService:Create(hubFrame, tweenInfo, {Position = UDim2.new(0.5, -225, 1, 50)})
    tween:Play()
    
    tween.Completed:Connect(function()
        hubFrame.Visible = false
        openButton.Visible = true
    end)
end

-- Função para mostrar créditos
local function showCredits()
    local creditsGui = Instance.new("ScreenGui")
    creditsGui.Name = "CreditsGui"
    creditsGui.Parent = playerGui
    creditsGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
    
    local creditsFrame = Instance.new("Frame")
    creditsFrame.Name = "CreditsFrame"
    creditsFrame.Size = UDim2.new(0, 350, 0, 200)
    creditsFrame.Position = UDim2.new(0.5, -175, 0.5, -100)
    creditsFrame.BackgroundColor3 = Color3.fromRGB(40, 80, 160)
    creditsFrame.BorderSizePixel = 0
    creditsFrame.ZIndex = 20
    creditsFrame.Parent = creditsGui
    
    local creditsCorner = Instance.new("UICorner")
    creditsCorner.CornerRadius = UDim.new(0, 12)
    creditsCorner.Parent = creditsFrame
    
    local creditsTitle = Instance.new("TextLabel")
    creditsTitle.Name = "CreditsTitle"
    creditsTitle.Size = UDim2.new(1, 0, 0, 40)
    creditsTitle.Position = UDim2.new(0, 0, 0, 0)
    creditsTitle.BackgroundColor3 = Color3.fromRGB(30, 60, 120)
    creditsTitle.BorderSizePixel = 0
    creditsTitle.Text = "Créditos"
    creditsTitle.TextColor3 = Color3.fromRGB(255, 255, 255)
    creditsTitle.TextSize = 20
    creditsTitle.Font = Enum.Font.GothamBold
    creditsTitle.ZIndex = 21
    creditsTitle.Parent = creditsFrame
    
    local titleCorner = Instance.new("UICorner")
    titleCorner.CornerRadius = UDim.new(0, 12)
    titleCorner.Parent = creditsTitle
    
    local discordText = Instance.new("TextLabel")
    discordText.Name = "DiscordText"
    discordText.Size = UDim2.new(1, -20, 0, 60)
    discordText.Position = UDim2.new(0, 10, 0, 50)
    discordText.BackgroundTransparency = 1
    discordText.Text = "Entre no nosso Discord:\nhttps://discord.gg/EefZABU4"
    discordText.TextColor3 = Color3.fromRGB(255, 255, 255)
    discordText.TextSize = 18
    discordText.Font = Enum.Font.Gotham
    discordText.TextWrapped = true
    discordText.ZIndex = 21
    discordText.Parent = creditsFrame
    
    local closeCreditsButton = Instance.new("TextButton")
    closeCreditsButton.Name = "CloseCreditsButton"
    closeCreditsButton.Size = UDim2.new(0, 100, 0, 40)
    closeCreditsButton.Position = UDim2.new(0.5, -50, 1, -60)
    closeCreditsButton.BackgroundColor3 = Color3.fromRGB(0, 100, 200)
    closeCreditsButton.BorderSizePixel = 0
    closeCreditsButton.Text = "Fechar"
    closeCreditsButton.TextColor3 = Color3.fromRGB(255, 255, 255)
    closeCreditsButton.TextSize = 18
    closeCreditsButton.Font = Enum.Font.GothamBold
    closeCreditsButton.ZIndex = 21
    closeCreditsButton.Parent = creditsFrame
    
    local closeButtonCorner = Instance.new("UICorner")
    closeButtonCorner.CornerRadius = UDim.new(0, 8)
    closeButtonCorner.Parent = closeCreditsButton
    
    closeCreditsButton.MouseButton1Click:Connect(function()
        creditsGui:Destroy()
    end)
end

-- Função para criar botão com estilo
local function createStyledButton(name, parent)
    local button = Instance.new("TextButton")
    button.Name = name
    button.Size = UDim2.new(1, 0, 0, 50)
    button.BackgroundColor3 = Color3.fromRGB(50, 100, 200)
    button.BorderSizePixel = 0
    button.Text = name
    button.TextColor3 = Color3.fromRGB(255, 255, 255)
    button.TextSize = 18
    button.Font = Enum.Font.Gotham
    button.ZIndex = 7
    button.Parent = parent
    
    local buttonCorner = Instance.new("UICorner")
    buttonCorner.CornerRadius = UDim.new(0, 8)
    buttonCorner.Parent = button
    
    button.MouseEnter:Connect(function()
        button.BackgroundColor3 = Color3.fromRGB(70, 130, 230)
    end)
    
    button.MouseLeave:Connect(function()
        button.BackgroundColor3 = Color3.fromRGB(50, 100, 200)
    end)
    
    return button
end

-- Função para criar janela de Troll Player
local function createTrollPlayerWindow()
    local trollGui = Instance.new("ScreenGui")
    trollGui.Name = "TrollPlayerGui"
    trollGui.Parent = playerGui
    trollGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
    
    local trollFrame = Instance.new("Frame")
    trollFrame.Name = "TrollFrame"
    trollFrame.Size = UDim2.new(0, 400, 0, 500)
    trollFrame.Position = UDim2.new(0.5, -200, 0.5, -250)
    trollFrame.BackgroundColor3 = Color3.fromRGB(40, 80, 160)
    trollFrame.BorderSizePixel = 0
    trollFrame.ZIndex = 20
    trollFrame.Parent = trollGui
    
    local trollCorner = Instance.new("UICorner")
    trollCorner.CornerRadius = UDim.new(0, 12)
    trollCorner.Parent = trollFrame
    
    local trollTitle = Instance.new("TextLabel")
    trollTitle.Name = "TrollTitle"
    trollTitle.Size = UDim2.new(1, 0, 0, 40)
    trollTitle.Position = UDim2.new(0, 0, 0, 0)
    trollTitle.BackgroundColor3 = Color3.fromRGB(30, 60, 120)
    trollTitle.BorderSizePixel = 0
    trollTitle.Text = "Troll Player"
    trollTitle.TextColor3 = Color3.fromRGB(255, 255, 255)
    trollTitle.TextSize = 20
    trollTitle.Font = Enum.Font.GothamBold
    trollTitle.ZIndex = 21
    trollTitle.Parent = trollFrame
    
    local titleCorner = Instance.new("UICorner")
    titleCorner.CornerRadius = UDim.new(0, 12)
    titleCorner.Parent = trollTitle
    
    local playerListFrame = Instance.new("Frame")
    playerListFrame.Name = "PlayerListFrame"
    playerListFrame.Size = UDim2.new(1, -20, 0, 150)
    playerListFrame.Position = UDim2.new(0, 10, 0, 50)
    playerListFrame.BackgroundColor3 = Color3.fromRGB(30, 60, 120)
    playerListFrame.BorderSizePixel = 0
    playerListFrame.ZIndex = 21
    playerListFrame.Parent = trollFrame
    
    local playerListCorner = Instance.new("UICorner")
    playerListCorner.CornerRadius = UDim.new(0, 8)
    playerListCorner.Parent = playerListFrame
    
    local playerList = Instance.new("ScrollingFrame")
    playerList.Name = "PlayerList"
    playerList.Size = UDim2.new(1, -10, 1, -10)
    playerList.Position = UDim2.new(0, 5, 0, 5)
    playerList.BackgroundTransparency = 1
    playerList.BorderSizePixel = 0
    playerList.ScrollBarThickness = 5
    playerList.ZIndex = 22
    playerList.Parent = playerListFrame
    
    local playerListLayout = Instance.new("UIListLayout")
    playerListLayout.Padding = UDim.new(0, 5)
    playerListLayout.Parent = playerList
    
    local updateButton = createStyledButton("Atualizar Lista", trollFrame)
    updateButton.Position = UDim2.new(0, 10, 0, 210)
    updateButton.Size = UDim2.new(0.45, -5, 0, 40)
    
    local viewButton = createStyledButton("View Player", trollFrame)
    viewButton.Position = UDim2.new(0.55, 5, 0, 210)
    viewButton.Size = UDim2.new(0.45, -5, 0, 40)
    
    local itemLabel = Instance.new("TextLabel")
    itemLabel.Name = "ItemLabel"
    itemLabel.Size = UDim2.new(1, -20, 0, 30)
    itemLabel.Position = UDim2.new(0, 10, 0, 260)
    itemLabel.BackgroundTransparency = 1
    itemLabel.Text = "Selecionar Item:"
    itemLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
    itemLabel.TextSize = 16
    itemLabel.Font = Enum.Font.Gotham
    itemLabel.ZIndex = 21
    itemLabel.Parent = trollFrame
    
    local itemSelection = Instance.new("Frame")
    itemSelection.Name = "ItemSelection"
    itemSelection.Size = UDim2.new(1, -20, 0, 40)
    itemSelection.Position = UDim2.new(0, 10, 0, 290)
    itemSelection.BackgroundColor3 = Color3.fromRGB(30, 60, 120)
    itemSelection.BorderSizePixel = 0
    itemSelection.ZIndex = 21
    itemSelection.Parent = trollFrame
    
    local itemCorner = Instance.new("UICorner")
    itemCorner.CornerRadius = UDim.new(0, 8)
    itemCorner.Parent = itemSelection
    
    local items = {"Sofá Ball", "Flings Ball", "Flings Kick Ball", "Flings Ball V2"}
    local selectedItem = items[1]
    
    local itemText = Instance.new("TextLabel")
    itemText.Name = "ItemText"
    itemText.Size = UDim2.new(0.7, 0, 1, 0)
    itemText.Position = UDim2.new(0, 0, 0, 0)
    itemText.BackgroundTransparency = 1
    itemText.Text = selectedItem
    itemText.TextColor3 = Color3.fromRGB(255, 255, 255)
    itemText.TextSize = 16
    itemText.Font = Enum.Font.Gotham
    itemText.ZIndex = 22
    itemText.Parent = itemSelection
    
    local arrowButton = Instance.new("TextButton")
    arrowButton.Name = "ArrowButton"
    arrowButton.Size = UDim2.new(0, 40, 1, 0)
    arrowButton.Position = UDim2.new(1, -40, 0, 0)
    arrowButton.BackgroundColor3 = Color3.fromRGB(50, 100, 200)
    arrowButton.BorderSizePixel = 0
    arrowButton.Text = "▼"
    arrowButton.TextColor3 = Color3.fromRGB(255, 255, 255)
    arrowButton.TextSize = 16
    arrowButton.Font = Enum.Font.Gotham
    arrowButton.ZIndex = 22
    arrowButton.Parent = itemSelection
    
    local arrowCorner = Instance.new("UICorner")
    arrowCorner.CornerRadius = UDim.new(0, 8)
    arrowCorner.Parent = arrowButton
    
    local itemDropdown = Instance.new("Frame")
    itemDropdown.Name = "ItemDropdown"
    itemDropdown.Size = UDim2.new(1, 0, 0, 0)
    itemDropdown.Position = UDim2.new(0, 0, 1, 5)
    itemDropdown.BackgroundColor3 = Color3.fromRGB(30, 60, 120)
    itemDropdown.BorderSizePixel = 0
    itemDropdown.Visible = false
    itemDropdown.ZIndex = 23
    itemDropdown.Parent = itemSelection
    
    local dropdownLayout = Instance.new("UIListLayout")
    dropdownLayout.Parent = itemDropdown
    
    arrowButton.MouseButton1Click:Connect(function()
        itemDropdown.Visible = not itemDropdown.Visible
        if itemDropdown.Visible then
            itemDropdown.Size = UDim2.new(1, 0, 0, 160)
        else
            itemDropdown.Size = UDim2.new(1, 0, 0, 0)
        end
    end)
    
    for _, item in ipairs(items) do
        local optionButton = Instance.new("TextButton")
        optio

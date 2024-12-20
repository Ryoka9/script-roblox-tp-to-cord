-- Função principal para criar a interface
local function CreateTeleportGUI()
    -- Criação do ScreenGui
    local ScreenGui = Instance.new("ScreenGui")
    ScreenGui.Name = "TeleportGUI"
    ScreenGui.Parent = game:GetService("CoreGui") -- Usando CoreGui
    ScreenGui.Enabled = true
    ScreenGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling

    -- Função utilitária para criar cantos arredondados
    local function CreateUICorner(radius, parent)
        local UICorner = Instance.new("UICorner")
        UICorner.CornerRadius = UDim.new(0, radius)
        UICorner.Parent = parent
    end

    -- === GUI PRINCIPAL ===
    local Frame = Instance.new("Frame")
    Frame.Parent = ScreenGui
    Frame.Position = UDim2.new(0.5, -150, 0.5, -100)
    Frame.Size = UDim2.new(0, 300, 0, 250)
    Frame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
    Frame.BorderSizePixel = 0
    Frame.Active = true
    CreateUICorner(10, Frame)

    -- === TÍTULO ===
    local Title = Instance.new("TextLabel")
    Title.Parent = Frame
    Title.Position = UDim2.new(0, 0, 0, 0)
    Title.Size = UDim2.new(1, 0, 0, 40)
    Title.Text = "Ryoka - Teleport GUI"
    Title.TextColor3 = Color3.fromRGB(255, 255, 255) -- Branco
    Title.BackgroundColor3 = Color3.fromRGB(50, 50, 50) -- Fundo cinza escuro
    Title.BorderSizePixel = 0
    Title.Font = Enum.Font.GothamBold
    Title.TextSize = 16
    Title.Active = true
    CreateUICorner(10, Title)

    -- === LABEL DE COORDENADAS ===
    local CoordinatesLabel = Instance.new("TextLabel")
    CoordinatesLabel.Parent = Frame
    CoordinatesLabel.Position = UDim2.new(0.1, 0, 0.2, 0)
    CoordinatesLabel.Size = UDim2.new(0.8, 0, 0, 30)
    CoordinatesLabel.Text = "Coordinates: -"
    CoordinatesLabel.TextColor3 = Color3.fromRGB(255, 255, 255) -- Branco
    CoordinatesLabel.BackgroundTransparency = 1
    CoordinatesLabel.Font = Enum.Font.Gotham
    CoordinatesLabel.TextSize = 14

    -- === BOTÃO DE TELEPORTE ===
    local TeleportButton = Instance.new("TextButton")
    TeleportButton.Parent = Frame
    TeleportButton.Position = UDim2.new(0.1, 0, 0.4, 0)
    TeleportButton.Size = UDim2.new(0.8, 0, 0, 40)
    TeleportButton.Text = "Teleport to Coordinates"
    TeleportButton.BackgroundColor3 = Color3.fromRGB(75, 0, 130) -- Roxo escuro
    TeleportButton.TextColor3 = Color3.fromRGB(255, 255, 255) -- Branco
    TeleportButton.Font = Enum.Font.GothamBold
    TeleportButton.TextSize = 14
    TeleportButton.BorderSizePixel = 0
    CreateUICorner(10, TeleportButton)

    -- === BOTÃO AUTO TP ===
    local AutoTPButton = Instance.new("TextButton")
    AutoTPButton.Parent = Frame
    AutoTPButton.Position = UDim2.new(0.1, 0, 0.6, 0)
    AutoTPButton.Size = UDim2.new(0.8, 0, 0, 40)
    AutoTPButton.Text = "Enable Auto TP"
    AutoTPButton.BackgroundColor3 = Color3.fromRGB(75, 0, 130) -- Roxo escuro
    AutoTPButton.TextColor3 = Color3.fromRGB(255, 255, 255) -- Branco
    AutoTPButton.Font = Enum.Font.GothamBold
    AutoTPButton.TextSize = 14
    AutoTPButton.BorderSizePixel = 0
    CreateUICorner(10, AutoTPButton)

    -- === BOTÃO DE FECHAR ===
    local CloseButton = Instance.new("TextButton")
    CloseButton.Parent = Frame
    CloseButton.Position = UDim2.new(1, -35, 0, 7)
    CloseButton.Size = UDim2.new(0, 25, 0, 25)
    CloseButton.Text = "X"
    CloseButton.BackgroundColor3 = Color3.fromRGB(75, 0, 130) -- Roxo escuro
    CloseButton.TextColor3 = Color3.fromRGB(255, 255, 255) -- Branco
    CloseButton.Font = Enum.Font.GothamBold
    CloseButton.TextSize = 14
    CloseButton.BorderSizePixel = 0
    CreateUICorner(5, CloseButton)

    -- === BOTÃO DE MINIMIZAR ===
    local MinimizeButton = Instance.new("TextButton")
    MinimizeButton.Parent = Frame
    MinimizeButton.Position = UDim2.new(1, -65, 0, 7)
    MinimizeButton.Size = UDim2.new(0, 25, 0, 25)
    MinimizeButton.Text = "-"
    MinimizeButton.BackgroundColor3 = Color3.fromRGB(75, 0, 130) -- Roxo escuro
    MinimizeButton.TextColor3 = Color3.fromRGB(255, 255, 255) -- Branco
    MinimizeButton.Font = Enum.Font.GothamBold
    MinimizeButton.TextSize = 14
    MinimizeButton.BorderSizePixel = 0
    CreateUICorner(5, MinimizeButton)

    -- Variáveis e estado
    local AutoTPEnabled = false
    local AutoTPCoroutine = nil
    local Minimized = false

    -- === FUNÇÕES ===

    -- Atualiza o label com as coordenadas do jogador
    local function UpdateCoordinates()
        while true do
            wait(0.1)
            local character = game.Players.LocalPlayer.Character
            if character and character:FindFirstChild("HumanoidRootPart") then
                local pos = character.HumanoidRootPart.Position
                CoordinatesLabel.Text = "Coordinates: " .. math.floor(pos.X) .. " " .. math.floor(pos.Y) .. " " .. math.floor(pos.Z)
            end
        end
    end

    -- Teletransporta o jogador para coordenadas fixas
    local function TeleportPlayer()
        local character = game.Players.LocalPlayer.Character
        if character and character:FindFirstChild("HumanoidRootPart") then
            character:MoveTo(Vector3.new(-121, 247, -532))
        else
            warn("Character or HumanoidRootPart not found.")
        end
    end

    -- Alterna o modo Auto TP (teleporte automático a cada 1 segundo)
    local function ToggleAutoTP()
        AutoTPEnabled = not AutoTPEnabled
        AutoTPButton.Text = AutoTPEnabled and "Disable Auto TP" or "Enable Auto TP"
        if AutoTPEnabled then
            AutoTPCoroutine = coroutine.create(function()
                while AutoTPEnabled do
                    TeleportPlayer()
                    wait(1)
                end
            end)
            coroutine.resume(AutoTPCoroutine)
        else
            AutoTPCoroutine = nil
        end
    end

    -- Minimiza ou maximiza a GUI
    local function ToggleMinimize()
        Minimized = not Minimized
        for _, child in ipairs(Frame:GetChildren()) do
            if child ~= Title and child ~= MinimizeButton and child ~= CloseButton then
                child.Visible = not Minimized
            end
        end
    end

    -- Fecha e encerra o GUI
    local function CloseGUI()
        AutoTPEnabled = false -- Desativa Auto TP
        ScreenGui:Destroy()
    end

    -- === CONEXÕES ===
    TeleportButton.MouseButton1Click:Connect(TeleportPlayer)
    AutoTPButton.MouseButton1Click:Connect(ToggleAutoTP)
    CloseButton.MouseButton1Click:Connect(CloseGUI)
    MinimizeButton.MouseButton1Click:Connect(ToggleMinimize)

    -- Inicia a atualização das coordenadas em segundo plano
    coroutine.wrap(UpdateCoordinates)()
end

-- Inicializa a GUI
CreateTeleportGUI()

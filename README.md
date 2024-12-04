-- Função para criar a GUI
local function CreateTeleportGUI()
    local ScreenGui = Instance.new("ScreenGui")
    ScreenGui.Name = "TeleportGUI" -- Nome para facilitar a identificação
    ScreenGui.Parent = game.Players.LocalPlayer:WaitForChild("PlayerGui")
    ScreenGui.Enabled = true
    ScreenGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling -- Define a prioridade de renderização do GUI

    -- Função para criar cantos arredondados
    local function CreateUICorner(radius, parent)
        local UICorner = Instance.new("UICorner")
        UICorner.CornerRadius = UDim.new(0, radius)
        UICorner.Parent = parent
    end

    local Frame = Instance.new("Frame")
    Frame.Parent = ScreenGui
    Frame.Position = UDim2.new(0.5, -150, 0.5, -100)
    Frame.Size = UDim2.new(0, 300, 0, 250)
    Frame.BackgroundColor3 = Color3.fromRGB(30, 30, 30) -- Preto elegante
    Frame.BorderSizePixel = 0
    Frame.Active = true -- Permite interação com o Frame

    CreateUICorner(10, Frame) -- Cantos arredondados no Frame

    -- Sombra para o menu
    local Shadow = Instance.new("ImageLabel")
    Shadow.Parent = Frame
    Shadow.BackgroundTransparency = 1
    Shadow.Size = UDim2.new(1, 20, 1, 20)
    Shadow.Position = UDim2.new(0, -10, 0, -10)
    Shadow.Image = "rbxassetid://5587865193" -- ID da sombra
    Shadow.ImageColor3 = Color3.fromRGB(0, 0, 0)
    Shadow.ImageTransparency = 0.5
    Shadow.ScaleType = Enum.ScaleType.Slice
    Shadow.SliceCenter = Rect.new(10, 10, 118, 118)

    local Title = Instance.new("TextLabel")
    Title.Parent = Frame
    Title.Position = UDim2.new(0, 0, 0, 0)
    Title.Size = UDim2.new(1, 0, 0, 40)
    Title.Text = "Ryoka"
    Title.TextColor3 = Color3.fromRGB(255, 50, 50) -- Vermelho vibrante
    Title.BackgroundColor3 = Color3.fromRGB(50, 50, 50) -- Fundo cinza escuro
    Title.BorderSizePixel = 0
    Title.Font = Enum.Font.GothamBold
    Title.TextSize = 16

    CreateUICorner(10, Title) -- Cantos arredondados no título

    local CoordinatesLabel = Instance.new("TextLabel")
    CoordinatesLabel.Parent = Frame
    CoordinatesLabel.Position = UDim2.new(0.1, 0, 0.2, 0)
    CoordinatesLabel.Size = UDim2.new(0.8, 0, 0, 30)
    CoordinatesLabel.Text = "Coordinates: -"
    CoordinatesLabel.TextColor3 = Color3.fromRGB(255, 255, 255) -- Branco
    CoordinatesLabel.BackgroundTransparency = 1
    CoordinatesLabel.Font = Enum.Font.Gotham
    CoordinatesLabel.TextSize = 14

    local TeleportButton = Instance.new("TextButton")
    TeleportButton.Parent = Frame
    TeleportButton.Position = UDim2.new(0.1, 0, 0.4, 0)
    TeleportButton.Size = UDim2.new(0.8, 0, 0, 40)
    TeleportButton.Text = "Teleport to Coordinates"
    TeleportButton.BackgroundColor3 = Color3.fromRGB(255, 50, 50) -- Vermelho vibrante
    TeleportButton.TextColor3 = Color3.fromRGB(255, 255, 255) -- Branco
    TeleportButton.Font = Enum.Font.GothamBold
    TeleportButton.TextSize = 14
    TeleportButton.BorderSizePixel = 0

    CreateUICorner(10, TeleportButton) -- Cantos arredondados no botão

    local AutoTPButton = Instance.new("TextButton")
    AutoTPButton.Parent = Frame
    AutoTPButton.Position = UDim2.new(0.1, 0, 0.6, 0)
    AutoTPButton.Size = UDim2.new(0.8, 0, 0, 40)
    AutoTPButton.Text = "Enable Auto TP"
    AutoTPButton.BackgroundColor3 = Color3.fromRGB(50, 255, 50) -- Verde
    AutoTPButton.TextColor3 = Color3.fromRGB(255, 255, 255) -- Branco
    AutoTPButton.Font = Enum.Font.GothamBold
    AutoTPButton.TextSize = 14
    AutoTPButton.BorderSizePixel = 0

    CreateUICorner(10, AutoTPButton) -- Cantos arredondados no botão

    local CloseButton = Instance.new("TextButton")
    CloseButton.Parent = Frame
    CloseButton.Position = UDim2.new(1, -35, 0, 7) -- Centralizado no canto superior direito
    CloseButton.Size = UDim2.new(0, 25, 0, 25)
    CloseButton.Text = "X"
    CloseButton.BackgroundColor3 = Color3.fromRGB(255, 50, 50) -- Vermelho vibrante
    CloseButton.TextColor3 = Color3.fromRGB(255, 255, 255) -- Branco
    CloseButton.Font = Enum.Font.GothamBold
    CloseButton.TextSize = 14
    CloseButton.BorderSizePixel = 0

    CreateUICorner(5, CloseButton) -- Cantos arredondados no botão fechar

    local AutoTPEnabled = false
    local function UpdateCoordinates()
        while true do
            wait(0.1)
            local character = game.Players.LocalPlayer.Character
            if character and character:FindFirstChild("HumanoidRootPart") then
                local x = character.HumanoidRootPart.Position.X
                local y = character.HumanoidRootPart.Position.Y
                local z = character.HumanoidRootPart.Position.Z
                CoordinatesLabel.Text = "Coordinates: " .. math.floor(x) .. " " .. math.floor(y) .. " " .. math.floor(z)
            end
        end
    end

    local function TeleportPlayer()
        local x = -535
        local y = 278
        local z = 1178
        local character = game.Players.LocalPlayer.Character
        if character and character:FindFirstChild("HumanoidRootPart") then
            character:MoveTo(Vector3.new(x, y, z))
        else
            warn("Character or HumanoidRootPart not found.")
        end
    end

    local function ToggleAutoTP()
        AutoTPEnabled = not AutoTPEnabled
        AutoTPButton.Text = AutoTPEnabled and "Disable Auto TP" or "Enable Auto TP"
        AutoTPButton.BackgroundColor3 = AutoTPEnabled and Color3.fromRGB(255, 50, 50) or Color3.fromRGB(50, 255, 50)
        if AutoTPEnabled then
            coroutine.wrap(function()
                while AutoTPEnabled do
                    TeleportPlayer()
                    wait(1) -- Intervalo de teleportação
                end
            end)()
        end
    end

    local function CloseGUI()
        ScreenGui.Enabled = false
    end

    coroutine.wrap(UpdateCoordinates)()

    TeleportButton.MouseButton1Click:Connect(TeleportPlayer)
    AutoTPButton.MouseButton1Click:Connect(ToggleAutoTP)
    CloseButton.MouseButton1Click:Connect(CloseGUI)
end

local player = game.Players.LocalPlayer
player.CharacterAdded:Connect(CreateTeleportGUI)

CreateTeleportGUI()

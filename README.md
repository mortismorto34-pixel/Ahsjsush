-- Rat Cat Hub - Teleporte para o Jogador Mais Próximo (Duelos de Bombas)

local player = game.Players.LocalPlayer

-- Função para encontrar o jogador mais próximo (exceto você)
local function getNearestPlayer()
    local shortest = math.huge
    local nearest = nil
    if not player.Character or not player.Character:FindFirstChild("HumanoidRootPart") then return nil end
    local myPos = player.Character.HumanoidRootPart.Position

    for _, plr in ipairs(game.Players:GetPlayers()) do
        if plr ~= player and plr.Character and plr.Character:FindFirstChild("HumanoidRootPart") then
            local pos = plr.Character.HumanoidRootPart.Position
            local dist = (myPos - pos).Magnitude
            if dist < shortest then
                shortest = dist
                nearest = plr
            end
        end
    end
    return nearest
end

-- Interface simples
local gui = Instance.new("ScreenGui", game.Players.LocalPlayer:WaitForChild("PlayerGui"))
gui.Name = "RatCatTeleport"

local frame = Instance.new("Frame", gui)
frame.Size = UDim2.new(0, 240, 0, 120)
frame.Position = UDim2.new(0.5, -120, 0.7, -60)
frame.BackgroundColor3 = Color3.fromRGB(32, 34, 42)
frame.BorderSizePixel = 0
frame.AnchorPoint = Vector2.new(0.5, 0.5)
frame.Active = true
frame.Draggable = true

local title = Instance.new("TextLabel", frame)
title.Size = UDim2.new(1, 0, 0, 36)
title.Position = UDim2.new(0, 0, 0, 0)
title.BackgroundTransparency = 1
title.Text = "Rat Cat Hub - Teleporte"
title.Font = Enum.Font.GothamBold
title.TextSize = 20
title.TextColor3 = Color3.fromRGB(85, 170, 255)

local info = Instance.new("TextLabel", frame)
info.Size = UDim2.new(1, -20, 0, 24)
info.Position = UDim2.new(0, 10, 0, 40)
info.BackgroundTransparency = 1
info.Text = "Teleporte para o jogador mais perto!"
info.Font = Enum.Font.Gotham
info.TextSize = 16
info.TextColor3 = Color3.fromRGB(220, 220, 220)
info.TextXAlignment = Enum.TextXAlignment.Left

local tpButton = Instance.new("TextButton", frame)
tpButton.Size = UDim2.new(0, 180, 0, 36)
tpButton.Position = UDim2.new(0.5, -90, 1, -46)
tpButton.Text = "Teleportar"
tpButton.Font = Enum.Font.GothamBold
tpButton.TextSize = 18
tpButton.TextColor3 = Color3.fromRGB(255,255,255)
tpButton.BackgroundColor3 = Color3.fromRGB(45,180,100)
tpButton.BorderSizePixel = 0

local closeButton = Instance.new("TextButton", frame)
closeButton.Size = UDim2.new(0, 28, 0, 28)
closeButton.Position = UDim2.new(1, -33, 0, 6)
closeButton.AnchorPoint = Vector2.new(0, 0)
closeButton.Text = "X"
closeButton.Font = Enum.Font.GothamBold
closeButton.TextSize = 14
closeButton.TextColor3 = Color3.fromRGB(255,255,255)
closeButton.BackgroundColor3 = Color3.fromRGB(255,80,80)
closeButton.BorderSizePixel = 0

closeButton.MouseButton1Click:Connect(function()
    gui:Destroy()
end)

tpButton.MouseButton1Click:Connect(function()
    local nearest = getNearestPlayer()
    if nearest and nearest.Character and nearest.Character:FindFirstChild("HumanoidRootPart") then
        local targetPos = nearest.Character.HumanoidRootPart.Position + Vector3.new(0,3,0)
        if player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
            player.Character.HumanoidRootPart.CFrame = CFrame.new(targetPos)
            info.Text = "Teleportado para " .. nearest.Name
        end
    else
        info.Text = "Nenhum jogador encontrado!"
    end
end)

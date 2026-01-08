-- Limpeza de segurança
local oldGui = game.Players.LocalPlayer:WaitForChild("PlayerGui"):FindFirstChild("Xeno_GodSpeed_V22")
if oldGui then oldGui:Destroy() end

local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "Xeno_GodSpeed_V22"
ScreenGui.Parent = game.Players.LocalPlayer:WaitForChild("PlayerGui")
ScreenGui.ResetOnSpawn = false

local MainFrame = Instance.new("Frame")
MainFrame.Size = UDim2.new(0, 250, 0, 220)
MainFrame.Position = UDim2.new(0.5, -125, 0.5, -110)
MainFrame.BackgroundColor3 = Color3.fromRGB(15, 15, 15)
MainFrame.Active = true
MainFrame.Draggable = true
MainFrame.Parent = ScreenGui
Instance.new("UICorner", MainFrame)

local Title = Instance.new("TextLabel")
Title.Size = UDim2.new(1, 0, 0, 45)
Title.Text = "GOD SPEED V22 FIX"
Title.TextColor3 = Color3.fromRGB(255, 255, 255)
Title.BackgroundColor3 = Color3.fromRGB(60, 0, 120) -- Roxo
Title.Font = Enum.Font.GothamBold
Title.Parent = MainFrame
Instance.new("UICorner", Title)

--- LÓGICA DE VELOCIDADE ---
_G.autoRunV22 = false
local player = game.Players.LocalPlayer
local SPEED_VAL = 10^15 -- Valor altíssimo, mas que o Roblox ainda consegue ler sem bugar

task.spawn(function()
    while true do
        task.wait()
        local char = player.Character
        local root = char and char:FindFirstChild("HumanoidRootPart")
        
        if _G.autoRunV22 and root then
            -- Força o Noclip
            for _, v in pairs(char:GetDescendants()) do
                if v:IsA("BasePart") then v.CanCollide = false end
            end

            -- Cria uma força constante para frente
            local bv = root:FindFirstChild("XenoVelocity") or Instance.new("BodyVelocity")
            bv.Name = "XenoVelocity"
            bv.Parent = root
            bv.MaxForce = Vector3.new(1, 1, 1) * math.huge
            bv.Velocity = root.CFrame.LookVector * SPEED_VAL

            -- Trava de Altura (Anti-Void)
            root.CFrame = CFrame.new(root.Position.X, root.Position.Y, root.Position.Z)
        else
            -- Se desligar, remove a força
            if root and root:FindFirstChild("XenoVelocity") then
                root.XenoVelocity:Destroy()
                root.Velocity = Vector3.new(0,0,0)
            end
        end
    end
end)

--- BOTÕES ---
local function createBtn(text, y, color, callback)
    local btn = Instance.new("TextButton", MainFrame)
    btn.Size = UDim2.new(0.9, 0, 0, 50)
    btn.Position = UDim2.new(0.05, 0, 0, y)
    btn.Text = text
    btn.BackgroundColor3 = color
    btn.TextColor3 = Color3.fromRGB(255, 255, 255)
    btn.Font = Enum.Font.SourceSansBold
    btn.TextSize = 18
    btn.MouseButton1Click:Connect(callback)
    Instance.new("UICorner", btn)
end

createBtn("ATIVAR CORRIDA SOLO", 60, Color3.fromRGB(0, 150, 0), function()
    _G.autoRunV22 = true
end)

createBtn("DESATIVAR / PARAR", 120, Color3.fromRGB(150, 0, 0), function()
    _G.autoRunV22 = false
    -- Reativa colisões
    local char = player.Character
    if char then
        for _, v in pairs(char:GetDescendants()) do
            if v:IsA("BasePart") then v.CanCollide = true end
        end
    end
end)

-- Fechar
local X = Instance.new("TextButton", MainFrame)
X.Size = UDim2.new(0, 30, 0, 30)
X.Position = UDim2.new(1, -35, 0, 5)
X.Text = "X"
X.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
X.MouseButton1Click:Connect(function() _G.autoRunV22 = false ScreenGui:Destroy() end)

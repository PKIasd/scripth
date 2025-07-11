--[[
  Script: GojoHub.lua
  Descrição: Hub com seis opções:
    1. Dano de proximidade em NPCs (não players) próximos da peça "ParteDano"
    2. Jogadores que tocarem na peça "ParteDano" ganham voo por 2 segundos
    3. Botão para permitir o player voar livremente enquanto ativado
    4. Botão para criar uma faca que pode ser usada para dar dano em humanoides (item de ferramenta)
    5. Slider para alterar a velocidade do player de 0 a 100
    6. Botão de Créditos que mostra "script feito por leoo"
    O botão de ativação do hub é redondo, todo cinza com borda amarela e pode ser arrastado pela tela.
    Coloque este script em StarterPlayerScripts ou StarterGui e crie uma peça chamada "ParteDano" no Workspace
--]]

local nomeDaPeca = "ParteDano"
local part = workspace:FindFirstChild(nomeDaPeca)
if not part then
    warn("Coloque uma peça chamada '" .. nomeDaPeca .. "' no Workspace para usar o sistema!")
end

local player = game.Players.LocalPlayer

-- Criando a Interface
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "GojoHub"
screenGui.Parent = player:WaitForChild("PlayerGui")

-- BOTÃO DE ATIVAÇÃO DO HUB (redondo, cinza e borda amarela, arrastável)
local hubToggle = Instance.new("TextButton")
hubToggle.Name = "GojoHubToggle"
hubToggle.Size = UDim2.new(0, 60, 0, 60)
hubToggle.Position = UDim2.new(0, 10, 0, 10)
hubToggle.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
hubToggle.TextColor3 = Color3.new(1,1,1)
hubToggle.Text = "☰"
hubToggle.Font = Enum.Font.SourceSansBold
hubToggle.TextSize = 40
hubToggle.AutoButtonColor = true
hubToggle.Parent = screenGui

-- Tornar redondo
local corner = Instance.new("UICorner")
corner.CornerRadius = UDim.new(1, 0)
corner.Parent = hubToggle

-- Cria uma borda amarela
local border = Instance.new("UIStroke")
border.Thickness = 3
border.Color = Color3.fromRGB(255, 204, 0)
border.Parent = hubToggle

-- Tornar o botão arrastável
local drag = Instance.new("Frame")
drag.Size = hubToggle.Size
drag.Position = hubToggle.Position
drag.BackgroundTransparency = 1
drag.Visible = false
drag.Parent = screenGui

hubToggle.ZIndex = 2
drag.ZIndex = 1

local UserInputService = game:GetService("UserInputService")
local dragging = false
local dragInput, mousePos, framePos

hubToggle.MouseButton1Down:Connect(function(input)
    dragging = true
    drag.Position = hubToggle.Position
    drag.Size = hubToggle.Size
    drag.Visible = true
    mousePos = UserInputService:GetMouseLocation()
    framePos = hubToggle.Position
end)

UserInputService.InputChanged:Connect(function(input)
    if dragging and input.UserInputType == Enum.UserInputType.MouseMovement then
        local delta = UserInputService:GetMouseLocation() - mousePos
        hubToggle.Position = UDim2.new(
            framePos.X.Scale,
            math.clamp(framePos.X.Offset + delta.X, 0, screenGui.AbsoluteSize.X - hubToggle.AbsoluteSize.X),
            framePos.Y.Scale,
            math.clamp(framePos.Y.Offset + delta.Y, 0, screenGui.AbsoluteSize.Y - hubToggle.AbsoluteSize.Y)
        )
        drag.Position = hubToggle.Position
    end
end)

UserInputService.InputEnded:Connect(function(input)
    if dragging and input.UserInputType == Enum.UserInputType.MouseButton1 then
        dragging = false
        drag.Visible = false
    end
end)

-- HUB PRINCIPAL (inicialmente escondido)
local frame = Instance.new("Frame")
frame.Size = UDim2.new(0, 280, 0, 390)
frame.Position = UDim2.new(0, 80, 0, 40)
frame.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
frame.Visible = false
frame.Parent = screenGui

local titulo = Instance.new("TextLabel")
titulo.Text = "Gojo Hub"
titulo.Size = UDim2.new(1, 0, 0, 30)
titulo.BackgroundTransparency = 1
titulo.TextColor3 = Color3.new(1,1,1)
titulo.Font = Enum.Font.SourceSansBold
titulo.TextSize = 22
titulo.Parent = frame

-- Botão 1: Dano de Proximidade em NPCs
local botaoDano = Instance.new("TextButton")
botaoDano.Text = "Ativar Dano em NPCs Próximos"
botaoDano.Size = UDim2.new(1, -20, 0, 45)
botaoDano.Position = UDim2.new(0, 10, 0, 40)
botaoDano.BackgroundColor3 = Color3.fromRGB(80, 80, 80)
botaoDano.TextColor3 = Color3.new(1,1,1)
botaoDano.Font = Enum.Font.SourceSans
botaoDano.TextSize = 18
botaoDano.Parent = frame

-- Botão 2: Voo ao encostar
local botaoVoo = Instance.new("TextButton")
botaoVoo.Text = "Ativar Voo ao Encostar (2s)"
botaoVoo.Size = UDim2.new(1, -20, 0, 45)
botaoVoo.Position = UDim2.new(0, 10, 0, 95)
botaoVoo.BackgroundColor3 = Color3.fromRGB(80, 80, 160)
botaoVoo.TextColor3 = Color3.new(1,1,1)
botaoVoo.Font = Enum.Font.SourceSans
botaoVoo.TextSize = 18
botaoVoo.Parent = frame

-- Botão 3: Voo livre para o player
local botaoVooLivre = Instance.new("TextButton")
botaoVooLivre.Text = "Ativar Voo Livre"
botaoVooLivre.Size = UDim2.new(1, -20, 0, 45)
botaoVooLivre.Position = UDim2.new(0, 10, 0, 150)
botaoVooLivre.BackgroundColor3 = Color3.fromRGB(0, 120, 255)
botaoVooLivre.TextColor3 = Color3.new(1,1,1)
botaoVooLivre.Font = Enum.Font.SourceSans
botaoVooLivre.TextSize = 18
botaoVooLivre.Parent = frame

-- Botão 4: Criar faca (item)
local botaoFaca = Instance.new("TextButton")
botaoFaca.Text = "Criar Faca (Ferramenta)"
botaoFaca.Size = UDim2.new(1, -20, 0, 45)
botaoFaca.Position = UDim2.new(0, 10, 0, 205)
botaoFaca.BackgroundColor3 = Color3.fromRGB(160, 160, 80)
botaoFaca.TextColor3 = Color3.new(0,0,0)
botaoFaca.Font = Enum.Font.SourceSans
botaoFaca.TextSize = 18
botaoFaca.Parent = frame

-- Botão/slider de velocidade
local velocidadeLabel = Instance.new("TextLabel")
velocidadeLabel.Text = "Velocidade: 16"
velocidadeLabel.Size = UDim2.new(1, -20, 0, 25)
velocidadeLabel.Position = UDim2.new(0, 10, 0, 255)
velocidadeLabel.BackgroundTransparency = 1
velocidadeLabel.TextColor3 = Color3.new(1,1,0.6)
velocidadeLabel.Font = Enum.Font.SourceSans
velocidadeLabel.TextSize = 18
velocidadeLabel.Parent = frame

local sliderBar = Instance.new("Frame")
sliderBar.Size = UDim2.new(1, -40, 0, 8)
sliderBar.Position = UDim2.new(0, 20, 0, 285)
sliderBar.BackgroundColor3 = Color3.fromRGB(100, 100, 100)
sliderBar.BorderSizePixel = 0
sliderBar.Parent = frame

local sliderKnob = Instance.new("Frame")
sliderKnob.Size = UDim2.new(0, 18, 0, 18)
sliderKnob.Position = UDim2.new(0, 20, 0, 281)
sliderKnob.BackgroundColor3 = Color3.fromRGB(255, 204, 0)
sliderKnob.BorderSizePixel = 0
sliderKnob.Parent = frame

local sliderCorner = Instance.new("UICorner")
sliderCorner.CornerRadius = UDim.new(1, 0)
sliderCorner.Parent = sliderKnob

local speedMin, speedMax = 0, 100
local defaultWalkSpeed = 16
local currentSpeed = defaultWalkSpeed

local function setWalkSpeed(v)
    local char = player.Character or player.CharacterAdded:Wait()
    local humanoid = char and char:FindFirstChildOfClass("Humanoid")
    if humanoid then
        humanoid.WalkSpeed = v
    end
    velocidadeLabel.Text = "Velocidade: " .. tostring(math.floor(v))
end

local function updateSlider(posX)
    local barAbsPos = sliderBar.AbsolutePosition.X
    local barAbsSize = sliderBar.AbsoluteSize.X
    local percent = math.clamp((posX - barAbsPos)/barAbsSize, 0, 1)
    local v = math.floor(speedMin + percent * (speedMax - speedMin))
    sliderKnob.Position = UDim2.new(0, sliderBar.Position.X.Offset + percent * sliderBar.Size.X.Offset - 9, 0, sliderBar.Position.Y.Offset - 5)
    setWalkSpeed(v)
    currentSpeed = v
end

sliderKnob.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 then
        local moveConn, upConn
        moveConn = UserInputService.InputChanged:Connect(function(moveInput)
            if moveInput.UserInputType == Enum.UserInputType.MouseMovement then
                updateSlider(UserInputService:GetMouseLocation().X)
            end
        end)
        upConn = UserInputService.InputEnded:Connect(function(endInput)
            if endInput.UserInputType == Enum.UserInputType.MouseButton1 then
                moveConn:Disconnect()
                upConn:Disconnect()
            end
        end)
    end
end)
sliderBar.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 then
        updateSlider(UserInputService:GetMouseLocation().X)
        local moveConn, upConn
        moveConn = UserInputService.InputChanged:Connect(function(moveInput)
            if moveInput.UserInputType == Enum.UserInputType.MouseMovement then
                updateSlider(UserInputService:GetMouseLocation().X)
            end
        end)
        upConn = UserInputService.InputEnded:Connect(function(endInput)
            if endInput.UserInputType == Enum.UserInputType.MouseButton1 then
                moveConn:Disconnect()
                upConn:Disconnect()
            end
        end)
    end
end)

player.CharacterAdded:Connect(function(char)
    char:WaitForChild("Humanoid").WalkSpeed = currentSpeed
end)
setWalkSpeed(currentSpeed)

-- Botão 6: Créditos
local botaoCreditos = Instance.new("TextButton")
botaoCreditos.Text = "Créditos"
botaoCreditos.Size = UDim2.new(1, -20, 0, 35)
botaoCreditos.Position = UDim2.new(0, 10, 0, 315)
botaoCreditos.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
botaoCreditos.TextColor3 = Color3.new(1,1,0.3)
botaoCreditos.Font = Enum.Font.SourceSansBold
botaoCreditos.TextSize = 18
botaoCreditos.Parent = frame

-- Popup de créditos (invisível até clicar)
local creditosFrame = Instance.new("Frame")
creditosFrame.Size = UDim2.new(0, 170, 0, 50)
creditosFrame.Position = UDim2.new(0.5, -85, 0, -60)
creditosFrame.AnchorPoint = Vector2.new(0,0)
creditosFrame.BackgroundColor3 = Color3.fromRGB(30,30,30)
creditosFrame.BorderSizePixel = 0
creditosFrame.Visible = false
creditosFrame.Parent = frame
local creditosCorner = Instance.new("UICorner")
creditosCorner.CornerRadius = UDim.new(0.3, 0)
creditosCorner.Parent = creditosFrame

local creditosLabel = Instance.new("TextLabel")
creditosLabel.Text = "script feito por leoo"
creditosLabel.Size = UDim2.new(1, 0, 1, 0)
creditosLabel.Position = UDim2.new(0, 0, 0, 0)
creditosLabel.BackgroundTransparency = 1
creditosLabel.TextColor3 = Color3.fromRGB(255, 204, 0)
creditosLabel.Font = Enum.Font.SourceSansBold
creditosLabel.TextSize = 20
creditosLabel.Parent = creditosFrame

botaoCreditos.MouseButton1Click:Connect(function()
    creditosFrame.Visible = not creditosFrame.Visible
    if creditosFrame.Visible then
        task.spawn(function()
            wait(2.5)
            creditosFrame.Visible = false
        end)
    end
end)

-- Lógica para mostrar/esconder o Hub
hubToggle.MouseButton1Click:Connect(function()
    frame.Visible = not frame.Visible
end)

-- Variáveis de controle
local danoAtivo = false
local vooAtivo = false
local conexaoDano
local vooLivreAtivo = false
local flyConnection, flyBodyVelocity, resetConnection

-- Função de dano de proximidade
local function ativarDanoProximidade(ativo, part)
    local runService = game:GetService("RunService")
    local RANGE = 10
    local DANO = 10000000
    local conexao

    if ativo then
        conexao = runService.Heartbeat:Connect(function()
            for _, obj in pairs(workspace:GetDescendants()) do
                if obj:IsA("Humanoid") then
                    local character = obj.Parent
                    local playerNPC = game.Players:GetPlayerFromCharacter(character)
                    if not playerNPC then
                        local humanoidRootPart = character:FindFirstChild("HumanoidRootPart")
                        if humanoidRootPart and (humanoidRootPart.Position - part.Position).Magnitude <= RANGE then
                            obj:TakeDamage(DANO)
                        end
                    end
                end
            end
        end)
    end

    return conexao
end

local function permitirVooTemporario(character, tempo)
    local humanoidRoot = character:FindFirstChild("HumanoidRootPart")
    if not humanoidRoot then return end
    local bodyVelocity = Instance.new("BodyVelocity")
    bodyVelocity.MaxForce = Vector3.new(1e5, 1e5, 1e5)
    bodyVelocity.Velocity = Vector3.new(0, 50, 0)
    bodyVelocity.Parent = humanoidRoot
    local humanoid = character:FindFirstChildOfClass("Humanoid")
    if humanoid then
        humanoid.PlatformStand = true
    end
    task.wait(tempo)
    if humanoid then
        humanoid.PlatformStand = false
    end
    bodyVelocity:Destroy()
end

local function ativarVooLivre()
    local character = player.Character or player.CharacterAdded:Wait()
    local humanoidRoot = character and character:FindFirstChild("HumanoidRootPart")
    local humanoid = character and character:FindFirstChildOfClass("Humanoid")
    if not humanoidRoot or not humanoid then return end
    flyBodyVelocity = Instance.new("BodyVelocity")
    flyBodyVelocity.MaxForce = Vector3.new(1e5, 1e5, 1e5)
    flyBodyVelocity.Velocity = Vector3.new(0,0,0)
    flyBodyVelocity.Parent = humanoidRoot
    humanoid.PlatformStand = true
    local UserInputService = game:GetService("UserInputService")
    local camera = workspace.CurrentCamera
    local direction = Vector3.new(0, 0, 0)
    local speed = 60

    local function updateVelocity()
        if flyBodyVelocity and camera then
            flyBodyVelocity.Velocity = (camera.CFrame:VectorToWorldSpace(direction)) * speed
        end
    end

    flyConnection = UserInputService.InputBegan:Connect(function(input, processed)
        if processed then return end
        if input.KeyCode == Enum.KeyCode.W then
            direction = Vector3.new(0, 0, -1)
        elseif input.KeyCode == Enum.KeyCode.S then
            direction = Vector3.new(0, 0, 1)
        elseif input.KeyCode == Enum.KeyCode.A then
            direction = Vector3.new(-1, 0, 0)
        elseif input.KeyCode == Enum.KeyCode.D then
            direction = Vector3.new(1, 0, 0)
        elseif input.KeyCode == Enum.KeyCode.Space then
            direction = Vector3.new(direction.X, 1, direction.Z)
        elseif input.KeyCode == Enum.KeyCode.LeftControl or input.KeyCode == Enum.KeyCode.LeftShift then
            direction = Vector3.new(direction.X, -1, direction.Z)
        end
        updateVelocity()
    end)

    resetConnection = UserInputService.InputEnded:Connect(function(input, processed)
        if processed then return end
        if input.KeyCode == Enum.KeyCode.W or input.KeyCode == Enum.KeyCode.S then
            direction = Vector3.new(0, direction.Y, 0)
        elseif input.KeyCode == Enum.KeyCode.A or input.KeyCode == Enum.KeyCode.D then
            direction = Vector3.new(0, direction.Y, 0)
        elseif input.KeyCode == Enum.KeyCode.Space or input.KeyCode == Enum.KeyCode.LeftControl or input.KeyCode == Enum.KeyCode.LeftShift then
            direction = Vector3.new(direction.X, 0, direction.Z)
        end
        updateVelocity()
    end)
end

local function desativarVooLivre()
    if flyConnection then
        flyConnection:Disconnect()
        flyConnection = nil
    end
    if resetConnection then
        resetConnection:Disconnect()
        resetConnection = nil
    end
    local character = player.Character
    if character then
        local humanoid = character:FindFirstChildOfClass("Humanoid")
        if humanoid then
            humanoid.PlatformStand = false
        end
        if flyBodyVelocity then
            flyBodyVelocity:Destroy()
            flyBodyVelocity = nil
        end
    end
end

botaoDano.MouseButton1Click:Connect(function()
    if not part then
        warn("Peça não encontrada!")
        return
    end
    danoAtivo = not danoAtivo
    if danoAtivo then
        botaoDano.Text = "Desativar Dano em NPCs Próximos"
        conexaoDano = ativarDanoProximidade(true, part)
    else
        botaoDano.Text = "Ativar Dano em NPCs Próximos"
        if conexaoDano then
            conexaoDano:Disconnect()
            conexaoDano = nil
        end
    end
end)

botaoVoo.MouseButton1Click:Connect(function()
    vooAtivo = not vooAtivo
    if vooAtivo then
        botaoVoo.Text = "Desativar Voo ao Encostar"
    else
        botaoVoo.Text = "Ativar Voo ao Encostar (2s)"
    end
end)

botaoVooLivre.MouseButton1Click:Connect(function()
    vooLivreAtivo = not vooLivreAtivo
    if vooLivreAtivo then
        botaoVooLivre.Text = "Desativar Voo Livre"
        ativarVooLivre()
    else
        botaoVooLivre.Text = "Ativar Voo Livre"
        desativarVooLivre()
    end
end)

botaoFaca.MouseButton1Click:Connect(function()
    if player.Backpack:FindFirstChild("GojoKnife") or (player.Character and player.Character:FindFirstChild("GojoKnife")) then
        warn("Você já tem uma faca Gojo!")
        return
    end

    local tool = Instance.new("Tool")
    tool.Name = "GojoKnife"
    tool.RequiresHandle = true
    tool.CanBeDropped = true

    local handle = Instance.new("Part")
    handle.Name = "Handle"
    handle.Size = Vector3.new(1, 0.2, 4)
    handle.BrickColor = BrickColor.new("Really black")
    handle.Material = Enum.Material.Metal
    handle.CanCollide = false
    handle.Parent = tool

    local blade = Instance.new("Part")
    blade.Name = "Blade"
    blade.Size = Vector3.new(0.2, 0.4, 2.5)
    blade.Position = handle.Position + Vector3.new(0, 0.2, 1.25)
    blade.BrickColor = BrickColor.new("Institutional white")
    blade.Material = Enum.Material.Metal
    blade.CanCollide = false
    blade.Anchored = false
    blade.Parent = tool
    local weld = Instance.new("WeldConstraint")
    weld.Part0 = handle
    weld.Part1 = blade
    weld.Parent = handle

    local scriptDano = Instance.new("Script")
    scriptDano.Name = "KnifeDamage"
    scriptDano.Source = [[
        local tool = script.Parent
        local DAMAGE = 50

        tool.Activated:Connect(function()
            local character = tool.Parent
            local humanoidRoot = character:FindFirstChild("HumanoidRootPart")
            if not humanoidRoot then return end

            for _, obj in pairs(workspace:GetDescendants()) do
                if obj:IsA("Humanoid") and obj.Health > 0 then
                    local theirChar = obj.Parent
                    if theirChar ~= character then
                        local root = theirChar:FindFirstChild("HumanoidRootPart")
                        if root and (root.Position - humanoidRoot.Position).Magnitude <= 6 then
                            obj:TakeDamage(DAMAGE)
                        end
                    end
                end
            end
        end)
    ]]
    scriptDano.Parent = tool

    tool.Parent = player.Backpack
end)

if part then
    part.Touched:Connect(function(hit)
        if not vooAtivo then return end
        local character = hit.Parent
        local playerChar = game.Players:GetPlayerFromCharacter(character)
        if playerChar then
            if not character:FindFirstChild("VoandoTemporario") then
                local flag = Instance.new("BoolValue")
                flag.Name = "VoandoTemporario"
                flag.Parent = character
                permitirVooTemporario(character, 2)
                flag:Destroy()
            end
        end
    end)
end

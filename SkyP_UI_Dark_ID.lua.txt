--[[
    SkyP UI - Dark Edition (Bahasa Indonesia)
    Versi: 1.0
    Penggunaan: Roblox Studio, sebagai LocalScript di StarterPlayer > StarterPlayerScripts
    Catatan keamanan:
      * UI dan kontrol ini berjalan lokal pada client.
      * Tidak ada executor, loadstring, HttpGet, key system, webhook, atau bypass server.
      * Gunakan hanya di experience milik sendiri / dengan izin pengembang.
      * Pengaturan tidak disimpan permanen; Roblox LocalScript tidak menyediakan
        penyimpanan file lokal yang aman. Simpan konfigurasi server-side bila diperlukan.
]]

local Players = game:GetService("Players")
local UserInputService = game:GetService("UserInputService")
local RunService = game:GetService("RunService")

local player = Players.LocalPlayer
local playerGui = player:WaitForChild("PlayerGui")

-- Hindari GUI ganda ketika LocalScript dijalankan ulang di Studio.
local oldGui = playerGui:FindFirstChild("SkyP_UI")
if oldGui then oldGui:Destroy() end

local COLORS = {
    bg = Color3.fromRGB(11, 15, 25),
    panel = Color3.fromRGB(18, 25, 39),
    panel2 = Color3.fromRGB(24, 33, 50),
    accent = Color3.fromRGB(38, 151, 255),
    accentDark = Color3.fromRGB(25, 105, 190),
    text = Color3.fromRGB(238, 244, 255),
    muted = Color3.fromRGB(139, 155, 178),
    green = Color3.fromRGB(62, 205, 135),
    red = Color3.fromRGB(238, 91, 103),
    stroke = Color3.fromRGB(43, 57, 79),
}

local gui = Instance.new("ScreenGui")
gui.Name = "SkyP_UI"
gui.ResetOnSpawn = false
gui.IgnoreGuiInset = false
gui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
gui.Parent = playerGui

local function corner(parent, radius)
    local c = Instance.new("UICorner")
    c.CornerRadius = UDim.new(0, radius or 10)
    c.Parent = parent
    return c
end

local function stroke(parent, color, thickness)
    local s = Instance.new("UIStroke")
    s.Color = color or COLORS.stroke
    s.Thickness = thickness or 1
    s.Transparency = 0.15
    s.Parent = parent
    return s
end

local function makeLabel(parent, text, size, color, font)
    local label = Instance.new("TextLabel")
    label.BackgroundTransparency = 1
    label.Text = text
    label.TextColor3 = color or COLORS.text
    label.TextSize = size or 14
    label.Font = font or Enum.Font.Gotham
    label.TextXAlignment = Enum.TextXAlignment.Left
    label.TextYAlignment = Enum.TextYAlignment.Center
    label.Parent = parent
    return label
end

local main = Instance.new("Frame")
main.Name = "Main"
main.Size = UDim2.fromOffset(590, 405)
main.Position = UDim2.new(0.5, -295, 0.5, -202)
main.BackgroundColor3 = COLORS.bg
main.BorderSizePixel = 0
main.Parent = gui
corner(main, 15)
stroke(main, COLORS.stroke, 1)

local scale = Instance.new("UIScale")
scale.Scale = 1
scale.Parent = main

local top = Instance.new("Frame")
top.Name = "Topbar"
top.Size = UDim2.new(1, 0, 0, 58)
top.BackgroundColor3 = COLORS.panel
top.BorderSizePixel = 0
top.Parent = main
corner(top, 15)

local topCover = Instance.new("Frame")
topCover.Size = UDim2.new(1, 0, 0, 15)
topCover.Position = UDim2.new(0, 0, 1, -15)
topCover.BackgroundColor3 = COLORS.panel
topCover.BorderSizePixel = 0
topCover.Parent = top

local logo = makeLabel(top, "SKYP", 22, COLORS.accent, Enum.Font.GothamBlack)
logo.Position = UDim2.fromOffset(18, 7)
logo.Size = UDim2.fromOffset(90, 25)

local subtitle = makeLabel(top, "GROWBLOX  •  PANEL LOKAL", 10, COLORS.muted, Enum.Font.GothamMedium)
subtitle.Position = UDim2.fromOffset(20, 32)
subtitle.Size = UDim2.fromOffset(220, 16)

local status = makeLabel(top, "● SIAP", 11, COLORS.green, Enum.Font.GothamBold)
status.TextXAlignment = Enum.TextXAlignment.Right
status.AnchorPoint = Vector2.new(1, 0)
status.Position = UDim2.new(1, -55, 0, 20)
status.Size = UDim2.fromOffset(90, 20)

local close = Instance.new("TextButton")
close.Size = UDim2.fromOffset(30, 30)
close.Position = UDim2.new(1, -42, 0, 14)
close.BackgroundColor3 = COLORS.panel2
close.Text = "×"
close.TextColor3 = COLORS.text
close.TextSize = 22
close.Font = Enum.Font.Gotham
close.AutoButtonColor = true
close.Parent = top
corner(close, 8)
close.MouseButton1Click:Connect(function()
    gui:Destroy()
end)

-- Drag jendela menggunakan mouse atau sentuhan.
do
    local dragging, dragInput, dragStart, startPos
    top.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1
            or input.UserInputType == Enum.UserInputType.Touch then
            dragging = true
            dragStart = input.Position
            startPos = main.Position
            input.Changed:Connect(function()
                if input.UserInputState == Enum.UserInputState.End then
                    dragging = false
                end
            end)
        end
    end)
    top.InputChanged:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseMovement
            or input.UserInputType == Enum.UserInputType.Touch then
            dragInput = input
        end
    end)
    UserInputService.InputChanged:Connect(function(input)
        if input == dragInput and dragging then
            local delta = input.Position - dragStart
            main.Position = UDim2.new(
                startPos.X.Scale, startPos.X.Offset + delta.X,
                startPos.Y.Scale, startPos.Y.Offset + delta.Y
            )
        end
    end)
end

local sidebar = Instance.new("Frame")
sidebar.Name = "Sidebar"
sidebar.Position = UDim2.fromOffset(12, 70)
sidebar.Size = UDim2.fromOffset(145, 322)
sidebar.BackgroundColor3 = COLORS.panel
sidebar.BorderSizePixel = 0
sidebar.Parent = main
corner(sidebar, 11)

local sidePadding = Instance.new("UIPadding")
sidePadding.PaddingTop = UDim.new(0, 10)
sidePadding.PaddingLeft = UDim.new(0, 8)
sidePadding.PaddingRight = UDim.new(0, 8)
sidePadding.Parent = sidebar

local sideLayout = Instance.new("UIListLayout")
sideLayout.Padding = UDim.new(0, 6)
sideLayout.SortOrder = Enum.SortOrder.LayoutOrder
sideLayout.Parent = sidebar

local content = Instance.new("Frame")
content.Name = "Content"
content.Position = UDim2.fromOffset(169, 70)
content.Size = UDim2.new(1, -181, 1, -83)
content.BackgroundColor3 = COLORS.panel
content.BorderSizePixel = 0
content.Parent = main
corner(content, 11)

local pageTitle = makeLabel(content, "Beranda", 17, COLORS.text, Enum.Font.GothamBold)
pageTitle.Position = UDim2.fromOffset(16, 12)
pageTitle.Size = UDim2.new(1, -32, 0, 25)

local pageHint = makeLabel(content, "Panel kontrol lokal SkyP.", 11, COLORS.muted)
pageHint.Position = UDim2.fromOffset(16, 38)
pageHint.Size = UDim2.new(1, -32, 0, 18)

local pageBody = Instance.new("ScrollingFrame")
pageBody.Name = "PageBody"
pageBody.Position = UDim2.fromOffset(12, 65)
pageBody.Size = UDim2.new(1, -24, 1, -77)
pageBody.BackgroundTransparency = 1
pageBody.BorderSizePixel = 0
pageBody.ScrollBarThickness = 4
pageBody.ScrollBarImageColor3 = COLORS.accent
pageBody.CanvasSize = UDim2.new()
pageBody.AutomaticCanvasSize = Enum.AutomaticSize.Y
pageBody.Parent = content

local bodyLayout = Instance.new("UIListLayout")
bodyLayout.Padding = UDim.new(0, 8)
bodyLayout.SortOrder = Enum.SortOrder.LayoutOrder
bodyLayout.Parent = pageBody

local function clearBody()
    for _, child in ipairs(pageBody:GetChildren()) do
        if child ~= bodyLayout then child:Destroy() end
    end
end

local function card(height)
    local f = Instance.new("Frame")
    f.Size = UDim2.new(1, -4, 0, height or 66)
    f.BackgroundColor3 = COLORS.panel2
    f.BorderSizePixel = 0
    f.Parent = pageBody
    corner(f, 9)
    return f
end

local function notice(message)
    status.Text = "● " .. message
    status.TextColor3 = COLORS.green
end

local function addInfo(title, description)
    local f = card(66)
    local t = makeLabel(f, title, 13, COLORS.text, Enum.Font.GothamBold)
    t.Position = UDim2.fromOffset(12, 8)
    t.Size = UDim2.new(1, -24, 0, 20)
    local d = makeLabel(f, description, 11, COLORS.muted)
    d.Position = UDim2.fromOffset(12, 31)
    d.Size = UDim2.new(1, -24, 0, 25)
    d.TextWrapped = true
end

local function addToggle(title, description, defaultValue, callback)
    local f = card(68)
    local t = makeLabel(f, title, 13, COLORS.text, Enum.Font.GothamBold)
    t.Position = UDim2.fromOffset(12, 7)
    t.Size = UDim2.new(1, -105, 0, 20)
    local d = makeLabel(f, description, 10, COLORS.muted)
    d.Position = UDim2.fromOffset(12, 31)
    d.Size = UDim2.new(1, -105, 0, 25)
    d.TextWrapped = true

    local enabled = defaultValue == true
    local b = Instance.new("TextButton")
    b.Size = UDim2.fromOffset(68, 30)
    b.Position = UDim2.new(1, -80, 0.5, -15)
    b.BackgroundColor3 = enabled and COLORS.accentDark or COLORS.bg
    b.Text = enabled and "AKTIF" or "NONAKTIF"
    b.TextColor3 = COLORS.text
    b.TextSize = 10
    b.Font = Enum.Font.GothamBold
    b.Parent = f
    corner(b, 8)
    b.MouseButton1Click:Connect(function()
        enabled = not enabled
        b.Text = enabled and "AKTIF" or "NONAKTIF"
        b.BackgroundColor3 = enabled and COLORS.accentDark or COLORS.bg
        if callback then callback(enabled) end
        notice(enabled and "FITUR AKTIF" or "FITUR MATI")
    end)
end

local function addButton(title, description, buttonText, callback)
    local f = card(72)
    local t = makeLabel(f, title, 13, COLORS.text, Enum.Font.GothamBold)
    t.Position = UDim2.fromOffset(12, 6)
    t.Size = UDim2.new(1, -112, 0, 20)
    local d = makeLabel(f, description, 10, COLORS.muted)
    d.Position = UDim2.fromOffset(12, 30)
    d.Size = UDim2.new(1, -112, 0, 30)
    d.TextWrapped = true
    local b = Instance.new("TextButton")
    b.Size = UDim2.fromOffset(78, 32)
    b.Position = UDim2.new(1, -90, 0.5, -16)
    b.BackgroundColor3 = COLORS.accentDark
    b.Text = buttonText
    b.TextColor3 = COLORS.text
    b.TextSize = 10
    b.Font = Enum.Font.GothamBold
    b.Parent = f
    corner(b, 8)
    b.MouseButton1Click:Connect(callback)
end

-- Kontrol karakter lokal; hanya untuk experience milik sendiri / yang mengizinkan.
local walkSpeedEnabled = false
local speedValue = 24
local flyEnabled = false
local flyVelocity, flyGyro
local jumpConnection
local characterConnections = {}

local function getCharacter()
    return player.Character
end

local function applySpeed()
    local char = getCharacter()
    local hum = char and char:FindFirstChildOfClass("Humanoid")
    if hum then hum.WalkSpeed = walkSpeedEnabled and speedValue or 16 end
end

player.CharacterAdded:Connect(function(char)
    char:WaitForChild("Humanoid", 8)
    task.wait(0.2)
    applySpeed()
    if flyEnabled then
        flyEnabled = false
        notice("FLY DIRESET SAAT RESPAWN")
    end
end)

local function stopFly()
    local char = getCharacter()
    local root = char and char:FindFirstChild("HumanoidRootPart")
    if flyVelocity then flyVelocity:Destroy(); flyVelocity = nil end
    if flyGyro then flyGyro:Destroy(); flyGyro = nil end
    local hum = char and char:FindFirstChildOfClass("Humanoid")
    if hum then hum.PlatformStand = false end
end

local function startFly()
    local char = getCharacter()
    local root = char and char:FindFirstChild("HumanoidRootPart")
    local hum = char and char:FindFirstChildOfClass("Humanoid")
    if not root or not hum then
        notice("KARAKTER BELUM SIAP")
        flyEnabled = false
        return
    end
    hum.PlatformStand = true
    flyVelocity = Instance.new("BodyVelocity")
    flyVelocity.MaxForce = Vector3.new(1e5, 1e5, 1e5)
    flyVelocity.Velocity = Vector3.zero
    flyVelocity.Parent = root
    flyGyro = Instance.new("BodyGyro")
    flyGyro.MaxTorque = Vector3.new(1e5, 1e5, 1e5)
    flyGyro.P = 1e4
    flyGyro.CFrame = root.CFrame
    flyGyro.Parent = root
end

local function buildHome()
    pageTitle.Text = "Beranda"
    pageHint.Text = "Ringkasan panel dan status sesi."
    clearBody()
    addInfo("Selamat datang di SkyP", "Antarmuka dark mode berbahasa Indonesia. Semua kontrol berjalan lokal.")
    addInfo("Status keamanan", "Tidak menggunakan key, HTTP eksternal, executor, atau akses server tersembunyi.")
    addInfo("Panduan pemasangan", "Pasang file ini sebagai LocalScript di StarterPlayer > StarterPlayerScripts.")
    addInfo("Catatan", "Fitur farming Growblox membutuhkan API resmi dari pembuat game dan tidak diaktifkan di panel ini.")
end

local function buildFarm()
    pageTitle.Text = "Auto Farm"
    pageHint.Text = "Panel contoh; tidak mengirim aksi farming ke server."
    clearBody()
    addInfo("Grid target 5 × 5", "Pilih tile untuk menandai target visual. Ini hanya pilihan lokal.")
    local grid = Instance.new("Frame")
    grid.Size = UDim2.new(1, -4, 0, 190)
    grid.BackgroundColor3 = COLORS.panel2
    grid.BorderSizePixel = 0
    grid.Parent = pageBody
    corner(grid, 9)
    local layout = Instance.new("UIGridLayout")
    layout.CellSize = UDim2.fromOffset(43, 30)
    layout.CellPadding = UDim2.fromOffset(7, 7)
    layout.HorizontalAlignment = Enum.HorizontalAlignment.Center
    layout.VerticalAlignment = Enum.VerticalAlignment.Center
    layout.Parent = grid
    local selected = {}
    for i = 1, 25 do
        local b = Instance.new("TextButton")
        b.Text = tostring(i)
        b.TextColor3 = COLORS.text
        b.TextSize = 11
        b.Font = Enum.Font.GothamBold
        b.BackgroundColor3 = COLORS.bg
        b.Parent = grid
        corner(b, 6)
        b.MouseButton1Click:Connect(function()
            selected[i] = not selected[i]
            b.BackgroundColor3 = selected[i] and COLORS.accentDark or COLORS.bg
        end)
    end
    addInfo("Target terpilih", "Tile berwarna biru adalah target visual yang dipilih.")
    addToggle("Mode multi-target (UI)", "Sakelar contoh untuk mengatur tampilan panel.", false, nil)
end

local function buildMovement()
    pageTitle.Text = "Gerakan"
    pageHint.Text = "Kontrol gerakan karakter lokal."
    clearBody()
    addToggle("Walkspeed", "Mengatur WalkSpeed karakter lokal menjadi 24.", false, function(on)
        walkSpeedEnabled = on
        applySpeed()
    end)
    addToggle("Fly (lokal)", "Mode uji gerakan lokal; gunakan tombol arah dan lompat.", false, function(on)
        flyEnabled = on
        if on then startFly() else stopFly() end
    end)
    addToggle("Infinite Jump (lokal)", "Mengizinkan lompatan ulang saat tombol lompat ditekan.", false, function(on)
        if jumpConnection then jumpConnection:Disconnect(); jumpConnection = nil end
        if on then
            jumpConnection = UserInputService.JumpRequest:Connect(function()
                local char = getCharacter()
                local hum = char and char:FindFirstChildOfClass("Humanoid")
                if hum then hum:ChangeState(Enum.HumanoidStateType.Jumping) end
            end)
        end
    end)
    addButton("Reset gerakan", "Kembalikan kecepatan dan matikan fly.", "RESET", function()
        walkSpeedEnabled = false
        flyEnabled = false
        stopFly()
        applySpeed()
        notice("GERAKAN DIRESET")
        buildMovement()
    end)
end

local watermark

local function buildTools()
    pageTitle.Text = "Alat & Statistik"
    pageHint.Text = "Kontrol antarmuka lokal dan informasi sesi."
    clearBody()
    addToggle("Watermark SkyP", "Tampilkan label kecil di layar sendiri.", true, function(on)
        watermark.Visible = on
    end)
    addButton("Bersihkan status", "Reset indikator status pada bagian atas.", "BERSIHKAN", function()
        notice("SIAP")
    end)
    addInfo("Statistik", "Penghitung Gems/jam dan item memerlukan data resmi dari game.")
    addInfo("Config", "Nilai toggle berlaku selama sesi ini. Untuk penyimpanan permanen, gunakan DataStore melalui server game milik sendiri.")
end

local function buildSettings()
    pageTitle.Text = "Pengaturan"
    pageHint.Text = "Informasi konfigurasi SkyP."
    clearBody()
    addInfo("Bahasa", "Indonesia")
    addInfo("Tema", "Dark Blue / Black")
    addInfo("Versi", "SkyP UI 1.0")
    addInfo("Privasi", "Tidak ada webhook, pengiriman data, atau permintaan HTTP.")
    addButton("Tutup panel", "Hapus antarmuka dari layar.", "TUTUP", function()
        gui:Destroy()
    end)
end

-- Watermark lokal di layar sendiri.
watermark = Instance.new("TextLabel")
watermark.Name = "SkyPWatermark"
watermark.AnchorPoint = Vector2.new(1, 0)
watermark.Position = UDim2.new(1, -14, 0, 12)
watermark.Size = UDim2.fromOffset(145, 25)
watermark.BackgroundColor3 = COLORS.panel
watermark.BackgroundTransparency = 0.12
watermark.Text = "✦ SkyP  •  LOCAL"
watermark.TextColor3 = COLORS.accent
watermark.TextSize = 11
watermark.Font = Enum.Font.GothamBold
watermark.Parent = gui
corner(watermark, 8)
stroke(watermark, COLORS.stroke, 1)

task.spawn(function()
    local hue = 0.56
    while gui.Parent do
        hue = (hue + 0.002) % 1
        watermark.TextColor3 = Color3.fromHSV(hue, 0.65, 1)
        task.wait(0.04)
    end
end)

local tabs = {
    {name = "Beranda", icon = "⌂", build = buildHome},
    {name = "Auto Farm", icon = "▦", build = buildFarm},
    {name = "Gerakan", icon = "➤", build = buildMovement},
    {name = "Alat & Statistik", icon = "✧", build = buildTools},
    {name = "Pengaturan", icon = "⚙", build = buildSettings},
}

for index, tab in ipairs(tabs) do
    local b = Instance.new("TextButton")
    b.Name = tab.name:gsub("%W", "") .. "Tab"
    b.LayoutOrder = index
    b.Size = UDim2.new(1, 0, 0, 42)
    b.BackgroundColor3 = COLORS.panel2
    b.Text = tab.icon .. "   " .. tab.name
    b.TextColor3 = COLORS.muted
    b.TextSize = 11
    b.Font = Enum.Font.GothamSemibold
    b.TextXAlignment = Enum.TextXAlignment.Left
    b.AutoButtonColor = true
    b.Parent = sidebar
    corner(b, 8)
    local pad = Instance.new("UIPadding")
    pad.PaddingLeft = UDim.new(0, 10)
    pad.Parent = b
    b.MouseButton1Click:Connect(function()
        for _, child in ipairs(sidebar:GetChildren()) do
            if child:IsA("TextButton") then
                child.BackgroundColor3 = COLORS.panel2
                child.TextColor3 = COLORS.muted
            end
        end
        b.BackgroundColor3 = COLORS.accentDark
        b.TextColor3 = COLORS.text
        tab.build()
    end)
    if index == 1 then
        b.BackgroundColor3 = COLORS.accentDark
        b.TextColor3 = COLORS.text
    end
end

-- Fly: input gerak lokal berdasarkan kamera.
RunService.RenderStepped:Connect(function()
    if not flyEnabled or not flyVelocity or not flyGyro then return end
    local camera = workspace.CurrentCamera
    if not camera then return end
    local direction = Vector3.zero
    if UserInputService:IsKeyDown(Enum.KeyCode.W) then direction += camera.CFrame.LookVector end
    if UserInputService:IsKeyDown(Enum.KeyCode.S) then direction -= camera.CFrame.LookVector end
    if UserInputService:IsKeyDown(Enum.KeyCode.A) then direction -= camera.CFrame.RightVector end
    if UserInputService:IsKeyDown(Enum.KeyCode.D) then direction += camera.CFrame.RightVector end
    if UserInputService:IsKeyDown(Enum.KeyCode.Space) then direction += Vector3.yAxis end
    if UserInputService:IsKeyDown(Enum.KeyCode.LeftControl) then direction -= Vector3.yAxis end
    flyVelocity.Velocity = direction.Magnitude > 0 and direction.Unit * 42 or Vector3.zero
    flyGyro.CFrame = camera.CFrame
end)

buildHome()
notice("SIAP")

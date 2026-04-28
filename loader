local Players    = game:GetService("Players")
local UIS        = game:GetService("UserInputService")
local TweenService = game:GetService("TweenService")
local task, pcall, pairs, ipairs = task, pcall, pairs, ipairs

local function rnd(len)
    local s = ""
    for i = 1, len do s = s .. string.char(math.random(97, 122)) end
    return s
end

-- ============================================================
-- FOX FREEZER COLOUR PALETTE
-- ============================================================
local C = {
    bg      = Color3.fromRGB(4,   4,   7),      -- same dark base as FoxFreezer
    card    = Color3.fromRGB(12,  7,   10),     -- FoxFreezer CARD colour
    cell    = Color3.fromRGB(18,  8,   14),     -- slightly lighter
    log     = Color3.fromRGB(8,   4,   7),
    border  = Color3.fromRGB(255, 30,  45),     -- FoxFreezer RED stroke
    dim     = Color3.fromRGB(40,  14,  22),
    grid    = Color3.fromRGB(255, 30,  45),     -- red grid
    accent  = Color3.fromRGB(255, 30,  45),     -- FoxFreezer RED
    bright  = Color3.fromRGB(255, 235, 235),    -- FoxFreezer title text
    text    = Color3.fromRGB(248, 200, 200),
    subtext = Color3.fromRGB(120, 40,  55),
    muted   = Color3.fromRGB(90,  18,  28),
    done    = Color3.fromRGB(255, 30,  45),
}

-- ============================================================
-- GUI ROOT  — semi-transparent so game is visible behind
-- ============================================================
local loadGui = Instance.new("ScreenGui")
loadGui.Name           = rnd(12)
loadGui.Parent         = game:GetService("CoreGui")
loadGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
loadGui.IgnoreGuiInset = true

local loadBg = Instance.new("Frame", loadGui)
loadBg.Size                   = UDim2.new(1, 0, 1, 0)
loadBg.BackgroundColor3       = C.bg
loadBg.BackgroundTransparency = 0.30   -- semi-transparent: game visible behind
loadBg.BorderSizePixel        = 0
loadBg.ZIndex                 = 1

-- Subtle red grid overlay (very faint)
for i = 1, 10 do
    local g = Instance.new("Frame", loadBg)
    g.BackgroundColor3       = C.grid
    g.BackgroundTransparency = 0.97
    g.BorderSizePixel        = 0
    g.Size                   = UDim2.new(1, 0, 0, 1)
    g.Position               = UDim2.new(0, 0, i/10, 0)
    g.ZIndex                 = 2
end
for i = 1, 16 do
    local g = Instance.new("Frame", loadBg)
    g.BackgroundColor3       = C.grid
    g.BackgroundTransparency = 0.97
    g.BorderSizePixel        = 0
    g.Size                   = UDim2.new(0, 1, 1, 0)
    g.Position               = UDim2.new(i/16, 0, 0, 0)
    g.ZIndex                 = 2
end

-- Red corner brackets (Fox Freezer style sharp corners)
local function makeCorner(xScale, yScale, xFlip, yFlip)
    local sz = 26
    local function ln(w, h, xo, yo)
        local f = Instance.new("Frame", loadBg)
        f.BackgroundColor3       = C.accent
        f.BackgroundTransparency = 0.2
        f.BorderSizePixel        = 0
        f.ZIndex                 = 5
        f.Size                   = UDim2.new(0, w, 0, h)
        f.Position               = UDim2.new(xScale, xFlip*xo + (xFlip < 0 and -w or 0),
                                              yScale, yFlip*yo + (yFlip < 0 and -h or 0))
    end
    ln(sz, 2, 14, 14)
    ln(2, sz, 14, 14)
end
makeCorner(0, 0,  1,  1)
makeCorner(1, 0, -1,  1)
makeCorner(0, 1,  1, -1)
makeCorner(1, 1, -1, -1)

-- Thin scan line
local scanLine = Instance.new("Frame", loadBg)
scanLine.Size                   = UDim2.new(1, 0, 0, 1)
scanLine.BackgroundColor3       = C.accent
scanLine.BackgroundTransparency = 0.78
scanLine.BorderSizePixel        = 0
scanLine.ZIndex                 = 4

-- ============================================================
-- CARD  — Fox Freezer style: dark card, red UIStroke
-- ============================================================
local CARD_W, CARD_H = 400, 330
local card = Instance.new("Frame", loadBg)
card.Size             = UDim2.new(0, CARD_W, 0, CARD_H)
card.Position         = UDim2.new(0.5, -CARD_W/2, 0.5, -CARD_H/2)
card.BackgroundColor3 = C.card
card.BorderSizePixel  = 0
card.ZIndex           = 10
Instance.new("UICorner", card).CornerRadius = UDim.new(0, 18)   -- matches FoxFreezer rx=18

local cardStroke = Instance.new("UIStroke", card)
cardStroke.Color     = C.border   -- red, same as FoxFreezer mainStroke
cardStroke.Thickness = 1.5

-- Red top accent line (same as FoxFreezer headerLine)
local topBar = Instance.new("Frame", card)
topBar.Size             = UDim2.new(1, -44, 0, 1)
topBar.Position         = UDim2.new(0, 22, 0, 80)   -- positioned at header bottom like FoxFreezer
topBar.BackgroundColor3 = C.accent
topBar.BackgroundTransparency = 0.18
topBar.BorderSizePixel  = 0
topBar.ZIndex           = 11

-- ── HEADER ROW (mirrors FoxFreezer header layout)
local headerFrame = Instance.new("Frame", card)
headerFrame.BackgroundTransparency = 1
headerFrame.Size     = UDim2.new(1, -24, 0, 70)
headerFrame.Position = UDim2.new(0, 14, 0, 8)
headerFrame.ZIndex   = 11

-- Title label — SciFi font like FoxFreezer
local titleLbl = Instance.new("TextLabel", headerFrame)
titleLbl.BackgroundTransparency = 1
titleLbl.Size           = UDim2.new(1, -80, 0, 38)
titleLbl.Position       = UDim2.new(0, 0, 0, 6)
titleLbl.Text           = "FOX FREEZER"
titleLbl.TextColor3     = C.bright
titleLbl.TextSize       = 34
titleLbl.Font           = Enum.Font.SciFi   -- FoxFreezer font
titleLbl.TextXAlignment = Enum.TextXAlignment.Left
titleLbl.ZIndex         = 12

local subLbl = Instance.new("TextLabel", headerFrame)
subLbl.BackgroundTransparency = 1
subLbl.Size           = UDim2.new(1, -80, 0, 18)
subLbl.Position       = UDim2.new(0, 0, 0, 46)
subLbl.Text           = "INITIALIZING SYSTEMS"
subLbl.TextColor3     = C.muted
subLbl.TextSize       = 12
subLbl.Font           = Enum.Font.SciFi
subLbl.TextXAlignment = Enum.TextXAlignment.Left
subLbl.ZIndex         = 12

-- Spinning ring (top-right of header, like FoxFreezer logo area)
local RING = 54
local ringOuter = Instance.new("Frame", headerFrame)
ringOuter.BackgroundTransparency = 1
ringOuter.Size     = UDim2.new(0, RING, 0, RING)
ringOuter.Position = UDim2.new(1, -RING, 0.5, -RING/2)
ringOuter.ZIndex   = 12

local ringBg = Instance.new("Frame", ringOuter)
ringBg.BackgroundTransparency = 1
ringBg.Size   = UDim2.new(1, 0, 1, 0)
ringBg.ZIndex = 12
Instance.new("UICorner", ringBg).CornerRadius = UDim.new(1, 0)
local ringBgS = Instance.new("UIStroke", ringBg)
ringBgS.Color = C.dim; ringBgS.Thickness = 3

local ringArc = Instance.new("Frame", ringOuter)
ringArc.BackgroundTransparency = 1
ringArc.Size   = UDim2.new(1, 0, 1, 0)
ringArc.ZIndex = 13
Instance.new("UICorner", ringArc).CornerRadius = UDim.new(1, 0)
local ringArcS = Instance.new("UIStroke", ringArc)
ringArcS.Color = C.accent; ringArcS.Thickness = 3

local ringPct = Instance.new("TextLabel", ringOuter)
ringPct.BackgroundTransparency = 1
ringPct.Size           = UDim2.new(1, 0, 1, 0)
ringPct.Text           = "0%"
ringPct.TextColor3     = C.bright
ringPct.TextSize       = 13
ringPct.Font           = Enum.Font.SciFi
ringPct.TextXAlignment = Enum.TextXAlignment.Center
ringPct.TextYAlignment = Enum.TextYAlignment.Center
ringPct.ZIndex         = 14

-- ── 3 STAT CELLS  — styled like FoxFreezer cards (dark bg, red stroke, rounded)
local moduleData = {
    { label="SPEED",  sub="WalkSpeed ctrl" },
    { label="STEAL",  sub="Auto steal sys" },
    { label="COMBAT", sub="Auto duel sys"  },
}
local modCells = {}
local CELL_Y  = 98
local CELL_W  = (CARD_W - 28 - 12) / 3

for i, m in ipairs(moduleData) do
    local cell = Instance.new("Frame", card)
    cell.BackgroundColor3 = C.cell
    cell.BorderSizePixel  = 0
    cell.Size             = UDim2.new(0, CELL_W, 0, 66)
    cell.Position         = UDim2.new(0, 14 + (i-1)*(CELL_W+6), 0, CELL_Y)
    cell.ZIndex           = 11
    Instance.new("UICorner", cell).CornerRadius = UDim.new(0, 10)   -- FoxFreezer card rx=10

    local cs = Instance.new("UIStroke", cell)
    cs.Color = C.border; cs.Thickness = 1.15; cs.Transparency = 0.08  -- mirrors FoxFreezer cardStroke

    local clbl = Instance.new("TextLabel", cell)
    clbl.BackgroundTransparency = 1
    clbl.Size           = UDim2.new(1, -8, 0, 14)
    clbl.Position       = UDim2.new(0, 10, 0, 8)
    clbl.Text           = m.label
    clbl.TextColor3     = C.muted
    clbl.TextSize       = 9
    clbl.Font           = Enum.Font.SciFi
    clbl.TextXAlignment = Enum.TextXAlignment.Left
    clbl.ZIndex         = 12

    local cval = Instance.new("TextLabel", cell)
    cval.BackgroundTransparency = 1
    cval.Size           = UDim2.new(1, -8, 0, 18)
    cval.Position       = UDim2.new(0, 10, 0, 22)
    cval.Text           = "Inactive"
    cval.TextColor3     = C.accent   -- red "Inactive" like FoxFreezer status
    cval.TextSize       = 14
    cval.Font           = Enum.Font.SciFi
    cval.TextXAlignment = Enum.TextXAlignment.Left
    cval.ZIndex         = 12

    local csub = Instance.new("TextLabel", cell)
    csub.BackgroundTransparency = 1
    csub.Size           = UDim2.new(1, -8, 0, 11)
    csub.Position       = UDim2.new(0, 10, 0, 40)
    csub.Text           = m.sub
    csub.TextColor3     = C.muted
    csub.TextSize       = 8
    csub.Font           = Enum.Font.SciFi
    csub.TextXAlignment = Enum.TextXAlignment.Left
    csub.ZIndex         = 12

    local ctrack = Instance.new("Frame", cell)
    ctrack.BackgroundColor3 = C.dim
    ctrack.BorderSizePixel  = 0
    ctrack.Size             = UDim2.new(1, -20, 0, 3)
    ctrack.Position         = UDim2.new(0, 10, 0, 55)
    ctrack.ZIndex           = 12
    ctrack.ClipsDescendants = true
    Instance.new("UICorner", ctrack).CornerRadius = UDim.new(1, 0)

    local cfill = Instance.new("Frame", ctrack)
    cfill.BackgroundColor3 = C.accent
    cfill.BorderSizePixel  = 0
    cfill.Size             = UDim2.new(0, 0, 1, 0)
    cfill.ZIndex           = 13
    Instance.new("UICorner", cfill).CornerRadius = UDim.new(1, 0)

    table.insert(modCells, { cell=cell, cval=cval, cfill=cfill, cs=cs })
end

-- Divider below cells
local div2 = Instance.new("Frame", card)
div2.BackgroundColor3 = C.border
div2.BackgroundTransparency = 0.82
div2.BorderSizePixel  = 0
div2.Size             = UDim2.new(1, -44, 0, 1)
div2.Position         = UDim2.new(0, 22, 0, CELL_Y + 72)
div2.ZIndex           = 11

-- ── LOG BOX
local LOG_Y = CELL_Y + 78
local logBox = Instance.new("Frame", card)
logBox.BackgroundColor3 = C.log
logBox.BorderSizePixel  = 0
logBox.Size             = UDim2.new(1, -28, 0, 72)
logBox.Position         = UDim2.new(0, 14, 0, LOG_Y)
logBox.ZIndex           = 11
logBox.ClipsDescendants = true
Instance.new("UICorner", logBox).CornerRadius = UDim.new(0, 10)
local logStroke = Instance.new("UIStroke", logBox)
logStroke.Color = C.border; logStroke.Thickness = 1.1; logStroke.Transparency = 0.08

local logLines = {}
for i = 1, 3 do
    local ll = Instance.new("TextLabel", logBox)
    ll.BackgroundTransparency = 1
    ll.Size           = UDim2.new(1, -16, 0, 16)
    ll.Position       = UDim2.new(0, 10, 0, 6 + (i-1)*20)
    ll.Text           = ""
    ll.TextColor3     = C.muted
    ll.TextSize       = 9
    ll.Font           = Enum.Font.SciFi
    ll.TextXAlignment = Enum.TextXAlignment.Left
    ll.ZIndex         = 12
    table.insert(logLines, ll)
end

local cursorLbl = Instance.new("TextLabel", logBox)
cursorLbl.BackgroundTransparency = 1
cursorLbl.Size           = UDim2.new(0, 10, 0, 14)
cursorLbl.Text           = "█"
cursorLbl.TextColor3     = C.accent
cursorLbl.TextSize       = 9
cursorLbl.Font           = Enum.Font.SciFi
cursorLbl.TextXAlignment = Enum.TextXAlignment.Left
cursorLbl.Visible        = false
cursorLbl.ZIndex         = 13

-- ── PROGRESS BAR
local BAR_Y = LOG_Y + 80
local barTrack = Instance.new("Frame", card)
barTrack.BackgroundColor3 = C.dim
barTrack.BorderSizePixel  = 0
barTrack.Size             = UDim2.new(1, -28, 0, 4)
barTrack.Position         = UDim2.new(0, 14, 0, BAR_Y)
barTrack.ZIndex           = 11
barTrack.ClipsDescendants = true
Instance.new("UICorner", barTrack).CornerRadius = UDim.new(1, 0)

local barFill = Instance.new("Frame", barTrack)
barFill.BackgroundColor3 = C.accent
barFill.BorderSizePixel  = 0
barFill.Size             = UDim2.new(0, 0, 1, 0)
barFill.ZIndex           = 12
Instance.new("UICorner", barFill).CornerRadius = UDim.new(1, 0)

-- Bar glow tip
local barGlow = Instance.new("Frame", barFill)
barGlow.BackgroundColor3       = Color3.fromRGB(255, 100, 120)
barGlow.BackgroundTransparency = 0.35
barGlow.BorderSizePixel        = 0
barGlow.Size                   = UDim2.new(0, 18, 1, 0)
barGlow.Position               = UDim2.new(1, -18, 0, 0)
barGlow.ZIndex                 = 13
Instance.new("UICorner", barGlow).CornerRadius = UDim.new(0, 2)

local statusLbl = Instance.new("TextLabel", card)
statusLbl.BackgroundTransparency = 1
statusLbl.Size           = UDim2.new(1, -80, 0, 16)
statusLbl.Position       = UDim2.new(0, 14, 0, BAR_Y + 10)
statusLbl.Text           = "Initializing..."
statusLbl.TextColor3     = C.subtext
statusLbl.TextSize       = 11
statusLbl.Font           = Enum.Font.SciFi
statusLbl.TextXAlignment = Enum.TextXAlignment.Left
statusLbl.ZIndex         = 11

local pctLbl = Instance.new("TextLabel", card)
pctLbl.BackgroundTransparency = 1
pctLbl.Size           = UDim2.new(0, 50, 0, 16)
pctLbl.Position       = UDim2.new(1, -64, 0, BAR_Y + 10)
pctLbl.Text           = "0%"
pctLbl.TextColor3     = C.accent
pctLbl.TextSize       = 12
pctLbl.Font           = Enum.Font.SciFi
pctLbl.TextXAlignment = Enum.TextXAlignment.Right
pctLbl.ZIndex         = 11

-- ============================================================
-- ANIMATIONS
-- ============================================================
local loadDone = false

-- Scan line sweep
task.spawn(function()
    while not loadDone do
        TweenService:Create(scanLine,
            TweenInfo.new(2.8, Enum.EasingStyle.Sine, Enum.EasingDirection.InOut),
            {Position = UDim2.new(0,0,1,-1)}):Play()
        task.wait(2.8)
        TweenService:Create(scanLine,
            TweenInfo.new(2.8, Enum.EasingStyle.Sine, Enum.EasingDirection.InOut),
            {Position = UDim2.new(0,0,0,0)}):Play()
        task.wait(2.8)
    end
end)

-- Ring spin
task.spawn(function()
    while not loadDone do
        ringArc.Rotation = 0
        local t = TweenService:Create(ringArc,
            TweenInfo.new(1.1, Enum.EasingStyle.Linear),
            {Rotation = 360})
        t:Play(); t.Completed:Wait()
    end
end)

-- Cursor blink
task.spawn(function()
    while not loadDone do
        cursorLbl.TextTransparency = 0
        task.wait(0.45)
        cursorLbl.TextTransparency = 1
        task.wait(0.45)
    end
end)

-- Subtitle typewriter
local subtitles = {
    "INITIALIZING SYSTEMS",
    "BYPASSING ANTICHEAT",
    "INJECTING PAYLOAD",
    "ENCRYPTING CHANNELS",
    "LOADING MODULES",
    "SPOOFING FINGERPRINT",
}
task.spawn(function()
    local si = 1
    while not loadDone do
        local line = subtitles[si]
        subLbl.Text = ""
        for c = 1, #line do
            if loadDone then break end
            subLbl.Text = line:sub(1,c)
            task.wait(0.04)
        end
        task.wait(1.1)
        for c = #line, 0, -1 do
            if loadDone then break end
            subLbl.Text = line:sub(1,c)
            task.wait(0.022)
        end
        si = (si % #subtitles) + 1
        task.wait(0.18)
    end
end)

-- ============================================================
-- LOG HELPER
-- ============================================================
local logHistory = {}
local function pushLog(txt, bright)
    table.insert(logHistory, {txt=txt, bright=bright})
    if #logHistory > 3 then table.remove(logHistory, 1) end
    for i, entry in ipairs(logHistory) do
        logLines[i].Text       = entry.txt
        logLines[i].TextColor3 = entry.bright and C.text or C.muted
    end
    local lastLine = logLines[#logHistory]
    if lastLine and bright then
        cursorLbl.Visible  = true
        cursorLbl.Position = UDim2.new(0, 10 + lastLine.TextBounds.X + 2,
                                        0, 6 + (#logHistory-1)*20 + 1)
    end
end

-- ============================================================
-- LOAD SEQUENCE
-- ============================================================
local stageData = {
    { pct=30, modIdx=1, msg="Loading speed module...",
      logTxt="[ 00:01 ]  speed_module ........... injected"  },
    { pct=62, modIdx=2, msg="Loading steal module...",
      logTxt="[ 00:03 ]  steal_module ........... injected"  },
    { pct=88, modIdx=3, msg="Loading combat module...",
      logTxt="[ 00:05 ]  combat_module .......... injected" },
    { pct=96, modIdx=nil, msg="Finalizing payload...",
      logTxt="[ 00:07 ]  finalizing payload..."             },
}

local loadingMessages = {
    "Initializing systems...","Patching memory...","Loading modules...",
    "Verifying integrity...","Encrypting channels...","Calibrating modules...",
    "Spoofing fingerprint...","Resolving pointer chains...","Almost there...",
}

pushLog("[ boot ]  sequence initiated — standby", false)

local nextStage = 1
local msgIdx    = 1

local function applyStage(sc)
    statusLbl.Text = sc.msg
    if sc.modIdx then
        if sc.modIdx > 1 then
            local prev = modCells[sc.modIdx - 1]
            prev.cval.Text       = "Active"     -- match FoxFreezer "Active" label
            prev.cval.TextColor3 = C.bright
            TweenService:Create(prev.cfill, TweenInfo.new(0.3),
                {Size = UDim2.new(1, 0, 1, 0)}):Play()
        end
        local cur = modCells[sc.modIdx]
        cur.cval.Text       = "Loading..."
        cur.cval.TextColor3 = C.text
        TweenService:Create(cur.cfill,
            TweenInfo.new(1.5, Enum.EasingStyle.Sine, Enum.EasingDirection.InOut),
            {Size = UDim2.new(0.6, 0, 1, 0)}):Play()
    end
    pushLog(sc.logTxt, true)
end

-- Phase 1: 0→96% in ~8s
local PHASE1_STEPS  = 96
local PHASE1_TIME   = 8.0
local PHASE1_STEP_T = PHASE1_TIME / PHASE1_STEPS

TweenService:Create(barFill,
    TweenInfo.new(PHASE1_TIME, Enum.EasingStyle.Quad, Enum.EasingDirection.Out),
    {Size = UDim2.new(0.96, 0, 1, 0)}):Play()

for i = 0, PHASE1_STEPS do
    local pct = i
    ringPct.Text = pct .. "%"
    pctLbl.Text  = pct .. "%"

    while nextStage <= #stageData and pct >= stageData[nextStage].pct do
        applyStage(stageData[nextStage])
        nextStage = nextStage + 1
    end

    if i % 8 == 0 then
        statusLbl.Text = loadingMessages[((msgIdx-1) % #loadingMessages) + 1]
        msgIdx = msgIdx + 1
    end

    task.wait(PHASE1_STEP_T)
end

-- Phase 2: 96→99% slow crawl (~290s)
local PHASE2_TIME = 290.0
local slowMessages = {
    "Verifying kernel signatures...",
    "Deep scanning memory blocks...",
    "Resolving pointer chains...",
    "Patching bytecode checksums...",
    "Rebuilding obfuscation layers...",
    "Synchronizing with remote node...",
    "Bypassing integrity validator...",
    "Finalizing stealth payload...",
    "Cross-referencing injection map...",
    "Waiting for server handshake...",
    "Validating encryption keys...",
    "Calibrating exploit vectors...",
}

TweenService:Create(barFill,
    TweenInfo.new(PHASE2_TIME, Enum.EasingStyle.Linear),
    {Size = UDim2.new(1, 0, 1, 0)}):Play()

local slowIdx     = 1
local phase2Start = tick()

task.spawn(function()
    while not loadDone do
        local msg = slowMessages[((slowIdx-1) % #slowMessages) + 1]
        statusLbl.Text = msg
        pushLog("[ " .. string.format("%02d", slowIdx*15) .. ":00 ]  " .. msg:lower(), true)
        slowIdx = slowIdx + 1
        task.wait(15)
    end
end)

local curPct = 96
while curPct < 99 do
    local elapsed = tick() - phase2Start
    local newPct  = 96 + math.floor((elapsed / PHASE2_TIME) * 3)
    if newPct > curPct and newPct < 100 then
        curPct = newPct
        ringPct.Text = curPct .. "%"
        pctLbl.Text  = curPct .. "%"
    end
    task.wait(1)
end

local remaining = PHASE2_TIME - (tick() - phase2Start)
if remaining > 0 then task.wait(remaining) end

-- ── FINISH
loadDone = true
cursorLbl.Visible = false

for _, mc in ipairs(modCells) do
    mc.cval.Text       = "Active"   -- FoxFreezer uses "Active" not "READY"
    mc.cval.TextColor3 = C.bright
    TweenService:Create(mc.cfill, TweenInfo.new(0.4),
        {Size = UDim2.new(1, 0, 1, 0)}):Play()
end

ringPct.Text        = "100%"
ringPct.TextColor3  = C.bright
ringArcS.Color      = C.bright
pctLbl.Text         = "100%"
pctLbl.TextColor3   = C.bright
statusLbl.Text      = "All systems ready"
statusLbl.TextColor3 = C.bright
subLbl.Text         = "ALL SYSTEMS READY"
subLbl.TextColor3   = C.bright
titleLbl.TextColor3 = C.bright
TweenService:Create(barFill, TweenInfo.new(0.3),
    {Size = UDim2.new(1,0,1,0)}):Play()
pushLog("[ DONE  ]  all modules ready — have fun", true)
if logLines[3] then logLines[3].TextColor3 = C.bright end

task.wait(1.2)

-- Fade out
TweenService:Create(card,
    TweenInfo.new(0.4, Enum.EasingStyle.Quad, Enum.EasingDirection.In),
    {BackgroundTransparency = 1,
     Position = UDim2.new(0.5, -CARD_W/2, 0.44, -CARD_H/2)}):Play()
TweenService:Create(cardStroke, TweenInfo.new(0.3), {Transparency = 1}):Play()
TweenService:Create(loadBg,
    TweenInfo.new(0.5, Enum.EasingStyle.Quad, Enum.EasingDirection.In),
    {BackgroundTransparency = 1}):Play()

for _, lbl in ipairs({titleLbl, subLbl, statusLbl, pctLbl, ringPct}) do
    TweenService:Create(lbl, TweenInfo.new(0.3), {TextTransparency = 1}):Play()
end
for _, mc in ipairs(modCells) do
    TweenService:Create(mc.cell, TweenInfo.new(0.3), {BackgroundTransparency = 1}):Play()
    TweenService:Create(mc.cval, TweenInfo.new(0.3), {TextTransparency = 1}):Play()
end
for _, ll in ipairs(logLines) do
    TweenService:Create(ll, TweenInfo.new(0.3), {TextTransparency = 1}):Play()
end

task.wait(0.55)
loadGui:Destroy()

print("Fox Freezer — Loading complete!")

--criminality

local Fluent = loadstring(game:HttpGet("https://github.com/dawid-scripts/Fluent/releases/latest/download/main.lua"))()
local SaveManager = loadstring(game:HttpGet("https://raw.githubusercontent.com/dawid-scripts/Fluent/master/Addons/SaveManager.lua"))()
local InterfaceManager = loadstring(game:HttpGet("https://raw.githubusercontent.com/dawid-scripts/Fluent/master/Addons/InterfaceManager.lua"))()

local Window = Fluent:CreateWindow({
    Title = "Criminality Scripts " .. Fluent.Version,
    SubTitle = "by TheMeetly [CRIMINALITY]",
    TabWidth = 160,
    Size = UDim2.fromOffset(580, 460),
    Acrylic = true, -- The blur may be detectable, setting this to false disables blur entirely
    Theme = "Rose",
    MinimizeKey = Enum.KeyCode.LeftAlt-- Used when theres no MinimizeKeybind
})

--Fluent provides Lucide Icons https://lucide.dev/icons/ for the tabs, icons are optional
local Tabs = {
    Main = Window:AddTab({ Title = "Main", Icon = "" }),
    Visual = Window:AddTab({ Title = "Visual", Icon = "" }),
    Updates = Window:AddTab({ Title = "Updates", Icon = "" }),
    Settings = Window:AddTab({ Title = "Settings", Icon = "settings" })
}

local Options = Fluent.Options

do
    Fluent:Notify({
        Title = "CRIMINALITY",
        Content = "Script By Meetly",
        SubContent = "Ezz", -- Optional
        Duration = 5 -- Set to nil to make the notification not disappear
    })

    Tabs.Main:AddButton({
        Title = "AimBot UI",
        Description = "AimBot Criminality",
        Callback = function()
loadstring(game:HttpGet('https://raw.githubusercontent.com/Mick-gordon/Hyper-Escape/main/DeleteMobCheatEngine.lua'))()
        end
    })

Tabs.Visual:AddButton({
        Title = "ESP",
        Description = "Esp",
        Callback = function(esp)
loadstring(game:HttpGet('https://raw.githubusercontent.com/Lucasfin000/SpaceHub/main/UESP'))()
        end
    })

    local Toggle = Tabs.Visual:AddToggle("Chams", {Title = "Chams", Default = false })

    Toggle:OnChanged(function(Value)
        _G.highlightEnabled = Value
    end)


    _G.highlightEnabled = false

    local function getRoleColor(plr)
        if (plr.Backpack:FindFirstChild("Knife") or plr.Character:FindFirstChild("Knife")) then
            return Color3.new(1, 0, 0)
        elseif (plr.Backpack:FindFirstChild("Gun") or plr.Character:FindFirstChild("Gun")) then
            return Color3.new(0, 0, 1)
        else
            return Color3.new(186,85,211)
        end
    end
    
    local function handleHighlight()
        while true do
            if _G.highlightEnabled then
                for _, v in pairs(game.Players:GetChildren()) do
                    if v ~= game.Players.LocalPlayer and v.Character then
                        if not v.Character:FindFirstChild("Highlight") then
                            local highlight = Instance.new("Highlight", v.Character)
                            highlight.FillTransparency = 0.5
                            highlight.OutlineTransparency = 0.5
                            highlight.FillColor = getRoleColor(v)
                        else
                            v.Character.Highlight.FillColor = getRoleColor(v)
                        end
                    end
                end
            else
                for _, v in pairs(game.Players:GetChildren()) do
                    if v ~= game.Players.LocalPlayer and v.Character and v.Character:FindFirstChild("Highlight") then
                        v.Character.Highlight:Destroy()
                    end
                end
            end
            wait(0.1)
        end
    end
    
    spawn(handleHighlight)

    local Camera = game.Workspace.CurrentCamera
    local RunService = game:GetService("RunService")
    
    local Slider = Tabs.Visual:AddSlider("Slider", {
        Title = "FOV changer",
        Description = "FOV",
        Default = 60,
        Min = 60,
        Max = 120,
        Rounding = 1,
        Callback = function(Value)
            
            Camera.FieldOfView = Value
        end
    })

    RunService.RenderStepped:Connect(function()
        
        if Camera.FieldOfView ~= Slider.Value then
            Camera.FieldOfView = Slider.Value
        end
    end)

------------------------------------------------------------------------------DEALER & ATM

    local function toggleHighlight(state, objectName, color)
        if state then
            for _, v in pairs(game.Workspace:GetDescendants()) do
                if v:IsA("Model") and v.Name == objectName then
                    if not v:FindFirstChild("Highlight") then
                        local highlight = Instance.new("Highlight")
                        highlight.Parent = v
                        highlight.FillColor = color
                        highlight.OutlineColor = color
                    end
                end
            end
        else
            for _, v in pairs(game.Workspace:GetDescendants()) do
                if v:IsA("Model") and v.Name == objectName and v:FindFirstChild("Highlight") then
                    v.Highlight:Destroy()
                end
            end
        end
    end
    
    local ToggleDealer = Tabs.Visual:AddToggle("Dealer", {Title = "Dealer ESP", Default = false })
    ToggleDealer:OnChanged(function(Value)
        toggleHighlight(Value, "DealerMan", Color3.fromRGB(218, 165, 32))
    end)
    

    local function toggleHighlight(state, objectName, color)
        if state then
            for _, v in pairs(game.Workspace:GetDescendants()) do
                if v:IsA("Model") and v.Name == objectName then
                    if not v:FindFirstChild("Highlight") then
                        local highlight = Instance.new("Highlight")
                        highlight.Parent = v
                        highlight.FillColor = color
                        highlight.OutlineColor = color
                    end
                end
            end
        else
            for _, v in pairs(game.Workspace:GetDescendants()) do
                if v:IsA("Model") and v.Name == objectName and v:FindFirstChild("Highlight") then
                    v.Highlight:Destroy()
                end
            end
        end
    end
    
    local ToggleATM = Tabs.Visual:AddToggle("ATM", {Title = "ATM ESP", Default = false })
    ToggleATM:OnChanged(function(Value)
        toggleHighlight(Value, "ATM", Color3.fromRGB(0, 191, 255))
    end)
    



    Tabs.Visual:AddButton({
        Title = "FullBright",
        Description = "FullBright",
        Callback = function(esp)
pcall(function()
                local lighting = game:GetService("Lighting");
                lighting.Ambient = Color3.fromRGB(255, 255, 255);
                lighting.Brightness = 1;
                lighting.FogEnd = 1e10;
                for i, v in pairs(lighting:GetDescendants()) do
                    if v:IsA("BloomEffect") or v:IsA("BlurEffect") or v:IsA("ColorCorrectionEffect") or v:IsA("SunRaysEffect") then
                        v.Enabled = false;
                    end;
                end;
                lighting.Changed:Connect(function()
                    lighting.Ambient = Color3.fromRGB(255, 255, 255);
                    lighting.Brightness = 1;
                    lighting.FogEnd = 1e10;
                end);
                spawn(function()
                    local character = game:GetService("Players").LocalPlayer.Character;
                    while wait() do
                        repeat wait() until character ~= nil;
                        if not character.HumanoidRootPart:FindFirstChildWhichIsA("PointLight") then
                            local headlight = Instance.new("PointLight", character.HumanoidRootPart);
                            headlight.Brightness = 1;
                            headlight.Range = 60;
                        end;
                    end;
                end);
            end)
        end
    })

    Tabs.Visual:AddButton({
        Title = "Show Chat",
        Description = "Show Chat",
        Callback = function()
-- // Services
local Players = game:GetService("Players")

-- // Vars
local ChatFrame = Players.LocalPlayer.PlayerGui.Chat.Frame

-- //
ChatFrame.ChatChannelParentFrame.Visible = true
ChatFrame.ChatBarParentFrame.Position = UDim2.new(0, 0, 1, -42)
        end
    })

    Tabs.Visual:AddButton({
        Title = "KeyStrokes",
        Description = "KeyStrokes",
        Callback = function(esp)
getgenv().k1 = "W"
getgenv().k2 = "A"
getgenv().k3 =  "S"
getgenv().k4 = "D"

getgenv().backdrop = false -- only if you want the shadow bg.
getgenv().showms = true -- only if you want to have your ms shown.
getgenv().showfps = true -- only if you want to have your fps shown.
getgenv().showkps = true -- only if you want to have your kps shown.
getgenv().animated = true -- only if you want the GUI to have the animated shadow.
getgenv().showarrows = false -- only if you want arrow keys to be shown.
getgenv().keydrag = false -- only if you want the keys to be draggable, can also be buggy, will be worked on in the future.

loadstring(game:HttpGet("https://raw.githubusercontent.com/Zirmith/Util-Tools/main/keyStrokes.lua"))()

        end
    })

    Tabs.Main:AddButton({
        Title = "Glide Fly",
        Description = "Flight Bind: B - Fly/Speed; U - Only Speed",
        Callback = function(esp)
togglekey = "b"  -- fly toggle
upkey = "="      -- makes speed faster
downkey = "-"    -- makes speed slower
enablepart = "u" -- enables part fly
speed = -0.5     -- changes set speed
updown = false   -- true if you want to go up
notify = true    -- true if you want notifcations
flypart = true   -- true for part fly
local user = game:GetService("UserInputService")
local player = game:GetService("Players").LocalPlayer
local GuiService = game:GetService("StarterGui")
local mouse = game.Players.LocalPlayer:GetMouse()
local holdingWKey = false
local holdingSKey = false
local holdingAKey = false
local holdingDKey = false
local holdingSpaceKey = false
local holdingShiftKey = false
local check = false
GuiService:SetCore("SendNotification", {Title = "Speed", Text = "Script made by TheMeetly";})
mouse.KeyDown:connect(function(key)
   if key == enablepart then
       if flypart then
           flypart = false
           if notify then
               GuiService:SetCore("SendNotification", {Title = "Speed", Text = "Disabled part fly";})
           end
       else
           flypart = true
           if notify then
               GuiService:SetCore("SendNotification", {Title = "Speed", Text = "Enabled part fly";})
           end
       end
   end
end)
mouse.KeyDown:connect(function(key)
   if key == upkey then
       speed = speed + -0.1
       if notify then
           GuiService:SetCore("SendNotification", {Title = "Speed", Text = "Speed is now set to " .. speed;})
       end
   end
end)
mouse.KeyDown:connect(function(key)
   if key == downkey then
       speed = speed - -0.1
       if notify then
           GuiService:SetCore("SendNotification", {Title = "Speed", Text = "Speed is now set to " .. speed;})
       end
   end
end)
mouse.KeyDown:connect(function(key)
   if key == togglekey then
       if check  == true then
           check = false
           if notify then
               GuiService:SetCore("SendNotification", {Title = "Speed", Text = "Speed is now disabled";})
           end
           game.Workspace.fly:Destroy()
       else
           check = true
           if notify then
               GuiService:SetCore("SendNotification", {Title = "Speed", Text = "Speed is now enabled";})
           end
           if flypart then
               local brick = Instance.new("Part", workspace)
               brick.Size = Vector3.new(8, 2, 8)
               brick.CFrame = game:GetService("Players").LocalPlayer.Character.HumanoidRootPart.CFrame + Vector3.new(0, -4, 0)
               brick.Transparency = 1 brick.Anchored = true brick.Name = "fly"
               game:GetService('RunService').Stepped:connect(function()
                   brick.CFrame = game:GetService("Players").LocalPlayer.Character.HumanoidRootPart.CFrame + Vector3.new(0, -4, 0)
                   brick.Size = Vector3.new(8+-speed, 2, 8+-speed)
               end)
           end
       end
   end
end)
game:GetService('RunService').Stepped:connect(function()
   if check then
       if holdingWKey == true then
         game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame * CFrame.new(0,0,speed)
       end
       if holdingSKey == true then
        game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame * CFrame.new(0,0,-speed)
   end
       if holdingAKey == true then
   game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame * CFrame.new(speed,0,0)
   end
       if holdingDKey == true then
   game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame * CFrame.new(-speed,0,0)
       end
       if holdingShiftKey == true then
           game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame * CFrame.new(0,speed,0)
       end
       if updown then
           if holdingSpaceKey == true then
               game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame * CFrame.new(0,-speed,0)
           end
       end
   end
end)
user.InputBegan:Connect(function(inputObject)
   if (inputObject.KeyCode == Enum.KeyCode.W) then
       holdingWKey = true
   end
   if (inputObject.KeyCode == Enum.KeyCode.S) then
       holdingSKey = true
   end
   if (inputObject.KeyCode == Enum.KeyCode.A) then
       holdingAKey = true
   end
   if (inputObject.KeyCode == Enum.KeyCode.D) then
       holdingDKey = true
   end
   if (inputObject.KeyCode == Enum.KeyCode.LeftControl) then
       holdingShiftKey = true
   end
   if (inputObject.KeyCode == Enum.KeyCode.Space) then
       holdingSpaceKey = true
   end
end)
user.InputEnded:Connect(function(inputObject)
   if (inputObject.KeyCode == Enum.KeyCode.W) then
       holdingWKey = false
   end
   if( inputObject.KeyCode == Enum.KeyCode.S) then
       holdingSKey = false
   end
   if (inputObject.KeyCode == Enum.KeyCode.A) then
       holdingAKey = false
   end
   if (inputObject.KeyCode == Enum.KeyCode.D) then
       holdingDKey = false
   end
   if (inputObject.KeyCode == Enum.KeyCode.LeftControl) then
       holdingShiftKey = false
   end
   if (inputObject.KeyCode == Enum.KeyCode.Space) then
       holdingSpaceKey = false
   end
end)
        end
    })

	local Toggle = Tabs.Main:AddToggle("GravityToggle", {Title = "Jump-Boost", Default = false })
Toggle:OnChanged(function(Value)
    _G.gravityEnabled = Value
end)
_G.gravityEnabled = false

local function changeGravity()
    while true do
        if _G.gravityEnabled then
            game.Workspace.Gravity = 80
        else
            game.Workspace.Gravity = 192
        end
        wait(0.1) 
    end
end

spawn(changeGravity) 


    Tabs.Main:AddButton({
        Title = "Spin (P)",
        Description = "SpinBot",
        Callback = function()
local player = game.Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local humanoidRootPart = character:WaitForChild("HumanoidRootPart")

local rotationSpeed = 200
local rotating = false 

local function toggleRotation()
    rotating = not rotating 
    
    while rotating do
        
        humanoidRootPart.CFrame = humanoidRootPart.CFrame * CFrame.Angles(0, math.rad(rotationSpeed), 0)
        wait(0.1) 
    end
end

local UserInputService = game:GetService("UserInputService")

UserInputService.InputBegan:Connect(function(input, gameProcessed)
    if not gameProcessed and input.KeyCode == Enum.KeyCode.P then
        toggleRotation()
    end
end)
        end
    })

    Tabs.Main:AddButton({
        Title = "Noclip [H]",
        Description = "Noclip bind - H",
        Callback = function()
            local player = game.Players.LocalPlayer
            local character = player.Character or player.CharacterAdded:Wait()
            local humanoidRootPart = character:WaitForChild("HumanoidRootPart")
            local speed = 75
            local noclip = false
            
            local function toggleNoclip(enable)
                for _, part in pairs(character:GetDescendants()) do
                    if part:IsA("BasePart") and part.Name ~= "HumanoidRootPart" then
                        part.CanCollide = not enable
                    end
                end
            end
            
            game:GetService("UserInputService").InputBegan:Connect(function(input)
                if input.KeyCode == Enum.KeyCode.H then
                    noclip = not noclip
                    toggleNoclip(noclip)
                end
            end)
            
            game:GetService("RunService").RenderStepped:Connect(function(deltaTime)
                if noclip then
                    local hitSomething = false
                    for _, part in pairs(workspace:GetPartBoundsInBox(humanoidRootPart.CFrame, humanoidRootPart.Size)) do
                        if part and part.CanCollide and part.Parent ~= character then
                            hitSomething = true
                            break
                        end
                    end
                    if hitSomething then
                        humanoidRootPart.CFrame = humanoidRootPart.CFrame + humanoidRootPart.CFrame.LookVector * speed * deltaTime
                    end
                end
            end)
            
        end
    })
    
    Tabs.Updates:AddParagraph({
        Title = "Version 1.3",
        Content = "Other"
    })

    Tabs.Updates:AddParagraph({
        Title = "[-] Free-cam",
        Content = "Main"
    })

    Tabs.Updates:AddParagraph({
        Title = "[+] Dealer ESP",
        Content = "Main"
    })

    Tabs.Updates:AddParagraph({
        Title = "[+] ATM ESP",
        Content = "Main"
    })

    Tabs.Updates:AddParagraph({
        Title = "[/] Fix ESP lag",
        Content = "Other"
    })

    Tabs.Updates:AddParagraph({
        Title = "[/] Improved Noclip",
        Content = "Main"
    })

-- Addons:
-- SaveManager (Allows you to have a configuration system)
-- InterfaceManager (Allows you to have a interface managment system)

-- Hand the library over to our managers
SaveManager:SetLibrary(Fluent)
InterfaceManager:SetLibrary(Fluent)

-- Ignore keys that are used by ThemeManager.
-- (we dont want configs to save themes, do we?)
SaveManager:IgnoreThemeSettings()

-- You can add indexes of elements the save manager should ignore
SaveManager:SetIgnoreIndexes({})

-- use case for doing it this way:
-- a script hub could have themes in a global folder
-- and game configs in a separate folder per game
InterfaceManager:SetFolder("FluentScriptHub")
SaveManager:SetFolder("FluentScriptHub/specific-game")

InterfaceManager:BuildInterfaceSection(Tabs.Settings)
SaveManager:BuildConfigSection(Tabs.Settings)


Window:SelectTab(1)

Fluent:Notify({
    Title = "Fluent",
    Content = "The script has been loaded.",
    Duration = 8
})

-- You can use the SaveManager:LoadAutoloadConfig() to load a config
-- which has been marked to be one that auto loads!
SaveManager:LoadAutoloadConfig()
end

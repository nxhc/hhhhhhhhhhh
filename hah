-- // Variables
getgenv().VNMLoaded = true
local VNMLogo, GetTime = game:HttpGet("https://i.ibb.co/b1d2vRy/vnm-ware.png"), os.clock()

local NoclipMacro, Macro, PanicMode, TriggerBot, Desync, FreezePos, FrameSkip, FakeSpike = false, false, false, false, false, false, false, false
local ClosestPointCF, ToolConnection, SilentTarget, AimTarget, ForceLock, keybindTime, LastStutter = nil, nil, nil, nil, nil, 0, tick()
local SmoothingFactor, PositionData, PositionData2, CurrentIndex, CurrentIndex2, CurrentVelocity, CurrentVelocity2 = 6, {}, {}, 1, 1, Vector3.zero, Vector3.zero

local Script = {Functions = {}, Friends = {}, Drawing = {}, EspPlayers = {}, NotifyNote = {}, BeizerManager = {}, BeizerCurve = {}, PanicModeSaves = {}, SavedValue = {}}

local Saves = {ShotCheck = {}, CharCheck = {}, AddedCheck = {}}
local Detection = {OldAmmo = {}, Suspicious = {}, NewBullets = nil}

local Players, Client, Mouse, RS, Camera, GuiS, Uis, Tween = game:GetService("Players"), game:GetService("Players").LocalPlayer, game:GetService("Players").LocalPlayer:GetMouse(), game:GetService("RunService"), game:GetService("Workspace").CurrentCamera, game:GetService("GuiService"), game:GetService("UserInputService"), game:GetService("TweenService")
local Text_1, Text_2, Text_3, Text_4, Text_5, Text_6, Text_7 = tostring(math.random(VNM.MemorySpoofer.Lowest, VNM.MemorySpoofer.Maximum)), tostring("." .. math.random(1, 99) .. " MB"), tostring(math.random(VNM.MemorySpoofer.Lowest, VNM.MemorySpoofer.Maximum)), tostring("." .. math.random(1, 99) .. " MB"), tostring(math.random(VNM.MemorySpoofer.Lowest, VNM.MemorySpoofer.Maximum)), tostring("."..math.random(1, 999)), tostring("."..math.random(1, 999))

Script.SavedValue.SilentPart = VNM.Silent.Part
Script.SavedValue.AimAssistSmoothX = VNM.AimAssist.Smoothness_X
Script.SavedValue.AimAssistSmoothY = VNM.AimAssist.Smoothness_Y
Script.SavedValue.ShakeX = VNM.AimAssist.Shake.Shake_X
Script.SavedValue.ShakeY = VNM.AimAssist.Shake.Shake_Y
Script.SavedValue.ShakeZ = VNM.AimAssist.Shake.Shake_Z

Script.BeizerCurve.Offset = Vector2.new(0,0)

-- // Using Thise For TweenService Support + Im 2 Lazy
local SilentFovRadius = Instance.new("NumberValue")
SilentFovRadius.Name = "DevConsole"
SilentFovRadius.Parent = nil
SilentFovRadius.Value = VNM.Silent.Fov.Radius

-- // Main Intro Local
if VNM.Options.Intro then
    local Flash = Instance.new("ColorCorrectionEffect")
    local Blur = Instance.new("BlurEffect")
    local Gui = Instance.new("ScreenGui")
    local Image = Instance.new("ImageLabel")
    
    Flash.Parent = game.Lighting

    Blur.Parent = game.Lighting
    Blur.Size = 0
    
    Gui.Name = "RobloxGui"
    Gui.Parent = game.CoreGui
    Gui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
    
    Image.Parent = Gui
    Image.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
    Image.BackgroundTransparency = 1.000
    Image.Position = UDim2.new(0.405, 0, 0.405, 0)
    Image.Size = UDim2.new(0.17, 0, 0.27, 0)
    Image.ImageTransparency = 1
    Image.Image = "rbxassetid://17159766839"
    
    local Create = Tween:Create(Blur, TweenInfo.new(1.5, Enum.EasingStyle.Cubic, Enum.EasingDirection.Out), {Size = 50})
    Create:Play()
    Create.Completed:Wait()
    local Create2 = Tween:Create(Image, TweenInfo.new(1, Enum.EasingStyle.Cubic, Enum.EasingDirection.Out), {ImageTransparency = 0.2})
    Create2:Play()
    Create2.Completed:Wait()
    local Create3 = Tween:Create(Image, TweenInfo.new(0.3, Enum.EasingStyle.Bounce, Enum.EasingDirection.Out), {Size = UDim2.new(0.15, 0, 0.25, 0)})
    Create3:Play()
    local Create4 = Tween:Create(Image, TweenInfo.new(0.3, Enum.EasingStyle.Bounce, Enum.EasingDirection.Out), {Position = UDim2.new(0.415, 0, 0.415, 0)})
    Create4:Play()
    Flash.TintColor = Color3.fromRGB(223, 91, 91)
    Tween:Create(Flash, TweenInfo.new(0.7), {TintColor = Color3.fromRGB(255, 255, 255)}):Play()
    task.wait(1)
    Tween:Create(Image, TweenInfo.new(3, Enum.EasingStyle.Quad, Enum.EasingDirection.Out), {ImageTransparency = 1}):Play()
    local Create5 = Tween:Create(Blur, TweenInfo.new(3, Enum.EasingStyle.Quad, Enum.EasingDirection.Out), {Size = 0})
    Create5:Play()
    Create5.Completed:Wait()
    
    Gui:Destroy()
    Flash:Destroy()
    Blur:Destroy()
end

-- // Low Gfx Function
if VNM.Options.AutoLowGfx then
    for _, v in pairs(game:GetService("Workspace"):GetDescendants()) do
        if v:IsA("BasePart") and not v.Parent:FindFirstChild("Humanoid") then
            v.Material = Enum.Material.SmoothPlastic
            if v:IsA("Texture") then
                v:Destroy()
            end
        end
    end
end

-- // BoomBox Mute Function
if VNM.Options.MuteBoomBox then
    for _, v in pairs(game:GetService("Workspace"):GetDescendants()) do
        if v:IsA("Sound") and not (v.Name == "ShootSound" or v.Name == "NoAmmo") then
            v.Volume = 0
        end
    end
end

-- // Seat Remove Function
if VNM.Options.RemoveSeats then
    for _, v in pairs(game:GetService("Workspace"):GetDescendants()) do
        if v:IsA("Seat") then
            v:Destroy()
        end
    end
end

-- // The Gui V2. Not Giving Away

-- // Drawing For AimAssist, SilentAim And Desync Dot
Script.Drawing.SilentCircle = Drawing.new("Circle")
Script.Drawing.SilentCircle.Color = Color3.new(1,1,1)
Script.Drawing.SilentCircle.Thickness = 1
Script.Drawing.SilentCircle.NumSides = 128

Script.Drawing.AimAssistCircle = Drawing.new("Circle")
Script.Drawing.AimAssistCircle.Color = Color3.new(1,1,1)
Script.Drawing.AimAssistCircle.Thickness = 1
Script.Drawing.AimAssistCircle.NumSides = 128

Script.Drawing.DesyncCircle = Drawing.new("Circle")
Script.Drawing.DesyncCircle.Color = VNM.Desync.Visualize.Color
Script.Drawing.DesyncCircle.NumSides = 128
Script.Drawing.DesyncCircle.Transparency = 1
Script.Drawing.DesyncCircle.Radius = VNM.Desync.Visualize.Radius
Script.Drawing.DesyncCircle.Filled = true
Script.Drawing.DesyncCircle.Visible = false

-- // Checks If The Player Is Alive
Script.Functions.Alive = function(Plr)
    return (Plr and Plr.Character and Plr.Character:FindFirstChild("Humanoid") and Plr.Character:FindFirstChild("HumanoidRootPart")) and true or false
end

-- // Checks If Player Is On Your Screen
Script.Functions.OnScreen = function(Object)
    local _, OnScreen = Camera:WorldToScreenPoint(Object.Position)
    return OnScreen
end

-- // Gets The Fov Method
Script.Functions.GetFovPosition = function()
    if VNM.Silent.Fov.Method == ("Screen") then
        return Vector2.new(Camera.ViewportSize.X / 2, Camera.ViewportSize.Y / 2)
    else
        return Vector2.new(Mouse.X, Mouse.Y)
    end
end

-- // Gets Magnitude From Part Position And Mouse
Script.Functions.GetMagnitudeFromMouse = function(Part)
    local PartPos, OnScreen = Camera:WorldToScreenPoint(Part.Position)
    if OnScreen then
        local GetScreenPos = Script.Functions.GetFovPosition()
        local Magnitude = ((Vector2.new(PartPos.X, PartPos.Y) - GetScreenPos) - VNM.Silent.Fov.Offset).Magnitude
        return Magnitude
    end
    return math.huge
end

-- // Makes Random Number With Vector3 
Script.Functions.RandomVec3 = function(Number, Multi)
    return (Vector3.new(math.random(-Number, Number), math.random(-Number, Number), math.random(-Number, Number)) * Multi or 1)
end

-- // Checks If The Player Is Behind A Wall Or Something Else
Script.Functions.WallCheck = function(Pos, PartDescendant)
    local Character = Client.Character
    local Origin = Camera.CFrame.Position

    local RayCastParams = RaycastParams.new()
    RayCastParams.FilterType = Enum.RaycastFilterType.Blacklist
    RayCastParams.FilterDescendantsInstances = {Character, Camera}

    local Result = Workspace.Raycast(Workspace, Origin, Pos - Origin, RayCastParams)
    
    if (Result) then
        local PartHit = Result.Instance
        local Visible = (not PartHit or Instance.new("Part").IsDescendantOf(PartHit, PartDescendant))
        
        return Visible
    end
    return false
end

-- // Random Number To Compare
Script.Functions.CalculateChance = function(Percentage)
    Percentage = math.floor(Percentage)
    return math.random(0, 99) < Percentage
end

-- // Check If Crew Folder Is A Thing
Script.Functions.FindCrew = function(Plr)
	if Plr:FindFirstChild("DataFolder") and Plr.DataFolder:FindFirstChild("Information") and Plr.DataFolder.Information:FindFirstChild("Crew") and Client:FindFirstChild("DataFolder") and Client.DataFolder:FindFirstChild("Information") and Client.DataFolder.Information:FindFirstChild("Crew") then
        if Client.DataFolder.Information:FindFirstChild("Crew").Value ~= nil and Plr.DataFolder.Information:FindFirstChild("Crew").Value ~= nil and Plr.DataFolder.Information:FindFirstChild("Crew").Value ~= ("") and Client.DataFolder.Information:FindFirstChild("Crew").Value ~= ("") then 
			return true
		end
	end
	return false
end

-- // Clears AnyThing In The Console
Script.Functions.ClearConsole = function()
    coroutine.resume(coroutine.create(function()
        local DevConsole = game:GetService("CoreGui"):WaitForChild("DevConsoleMaster")
        local DevWindow = DevConsole:WaitForChild("DevConsoleWindow")
        local DevUI = DevWindow:WaitForChild("DevConsoleUI")
        local MainView = DevUI:WaitForChild("MainView")
        local ClientLog = MainView:WaitForChild("ClientLog")
        for _, v in pairs(ClientLog:GetChildren()) do
            if v:IsA("GuiObject") and v.Name == v.Name:match("%d+") then
                v:Destroy()
            end
        end
    end))
end

-- // Calculates The Velocity
Script.Functions.GetVelocity = function(PositionData)
	local TotalVelocity = 0
	local AveragePosition = Vector3.zero
	local AverageTime = 0
	local GetData = #PositionData
	if GetData == 0 then
		return AveragePosition, AverageTime
	end
	for i = 1, GetData do
		local Data = PositionData[i]
		if Data and Data.Position then
			local Velocity = SmoothingFactor - i + 1
			AveragePosition = AveragePosition + (Data.Position * Velocity)
			AverageTime = AverageTime + (Data.Time * Velocity)
			TotalVelocity = TotalVelocity + Velocity
		end
	end
	AveragePosition = AveragePosition / TotalVelocity
	AverageTime = AverageTime / TotalVelocity
	return AveragePosition, AverageTime
end

-- // Smoothness The Velocity Out
Script.Functions.ComPlexVelocity = function(Plr, CurrentPos, Mode)
	local GetTick = tick()
    local GetPos = CurrentPos

    -- // Fixes Calculating The Wrong Position
    if Mode then
        if PositionData2[CurrentIndex2] and PositionData2[CurrentIndex2].Target and tostring(Plr) ~= PositionData2[CurrentIndex2].Target then
            PositionData2[CurrentIndex2] = {Target = tostring(Plr), Position = GetPos, Time = GetTick}
            return Vector3.zero
        end
    else
        if PositionData[CurrentIndex] and PositionData[CurrentIndex].Target and tostring(Plr) ~= PositionData[CurrentIndex].Target then
            PositionData[CurrentIndex] = {Target = tostring(Plr), Position = GetPos, Time = GetTick}
            return Vector3.zero
        end
    end

    if Mode then
    	PositionData2[CurrentIndex2] = {Target = tostring(Plr), Position = GetPos, Time = GetTick}
    	CurrentIndex2 = (CurrentIndex2 % SmoothingFactor) + 1
    	local AveragePosition, AverageTime = Script.Functions.GetVelocity(PositionData2)
    	local PreviousData = PositionData2[CurrentIndex2]
    	if PreviousData and PreviousData.Position then
    		local Velocity = (GetPos - PreviousData.Position) / (GetTick - PreviousData.Time)
    		return Velocity
    	end
    else
    	PositionData[CurrentIndex] = {Target = tostring(Plr), Position = GetPos, Time = GetTick}
    	CurrentIndex = (CurrentIndex % SmoothingFactor) + 1
    	local AveragePosition, AverageTime = Script.Functions.GetVelocity(PositionData)
    	local PreviousData = PositionData[CurrentIndex]
    	if PreviousData and PreviousData.Position then
    		local Velocity = (GetPos - PreviousData.Position) / (GetTick - PreviousData.Time)
    		return Velocity
    	end
    end
    return Vector3.zero
end

-- // Changes The Mouse Position
Script.Functions.MouseOffset = function(self, X, Y)
    local NewPosition = Vector2.new(X, Y) - VNM.Silent.Fov.Offset
    mousemoveabs(NewPosition.X, NewPosition.Y)
end

-- // Splits The Gun Name And Splits []
Script.Functions.GetGunName = function(Name)
    local split = string.split(string.split(Name, "[")[2], "]")[1]
    return split
end

-- // Gets Current Gun
Script.Functions.GetCurrentWeaponName = function()
    if Client.Character and Client.Character:FindFirstChildWhichIsA("Tool") then
       local Tool =  Client.Character:FindFirstChildWhichIsA("Tool")
       if string.find(Tool.Name, "%[") and string.find(Tool.Name, "%]") and not string.find(Tool.Name, "Wallet") and not string.find(Tool.Name, "Phone") then
          return Script.Functions.GetGunName(Tool.Name)
       end
    end
    return nil
end

-- // Drawing Function With Property Attached
Script.Functions.NewDrawing = function(Type, Properties)
    local NewDrawing = Drawing.new(Type)
    for i, v in next, Properties or {} do
        NewDrawing[i] = v
    end
    return NewDrawing
end

-- // Draws For The New Players Joining For Esp
Script.Functions.NewPlayer = function(Plr)
    Script.EspPlayers[Plr] = {
        Name = Script.Functions.NewDrawing("Text", {Color = Color3.fromRGB(255,2550, 255), Outline = true, Visible = false, Center = true, Size = 13, Font = 0}),
        
        BoxOutline = Script.Functions.NewDrawing("Square", {Color = Color3.fromRGB(0, 0, 0), Thickness = 3, Visible = false}),
        Box = Script.Functions.NewDrawing("Square", {Color = Color3.fromRGB(255, 255, 255), Thickness = 1, Visible = false}),
        
        HealthBarOutline = Script.Functions.NewDrawing("Line", {Color = Color3.fromRGB(0, 0, 0), Thickness = 3, Visible = false}),
        HealthBar = Script.Functions.NewDrawing("Line", {Color = Color3.fromRGB(0, 255, 0), Thickness = 1, Visible = false}),
        HealthText = Script.Functions.NewDrawing("Text", {Color = Color3.fromRGB(0, 255, 0), Outline = true, Visible = false, Center = true, Size = 13, Font = 0}),
        
        ArmorBarOutline = Script.Functions.NewDrawing("Line", {Color = Color3.fromRGB(0, 0, 0), Thickness = 3, Visible = false}),
        ArmorBar = Script.Functions.NewDrawing("Line", {Color = Color3.fromRGB(0, 255, 0), Thickness = 1, Visible = false}),
        ArmorText = Script.Functions.NewDrawing("Text", {Color = Color3.fromRGB(0, 255, 0), Outline = true, Visible = false, Center = true, Size = 13, Font = 0}),
        
        Distance = Script.Functions.NewDrawing("Text", {Color = Color3.fromRGB(255, 255, 255), Outline = true, Visible = false, Center = true, Size = 13, Font = 0}),
        
        Tool = Script.Functions.NewDrawing("Text", {Color = Color3.fromRGB(255, 255, 255), Outline = true, Visible = false, Center = true, Size = 13, Font = 0}),
        
        Flag = Script.Functions.NewDrawing("Text", {Color = Color3.fromRGB(255, 255, 255), Outline = true, Visible = false, Center = true, Size = 13, Font = 0}),
        
        Tracer = Script.Functions.NewDrawing("Line", {Color = Color3.fromRGB(255, 255, 244), Thickness = 1, Visible = false}),
    }
end

-- // The Notification Ui Function
Script.Functions.CreateNotification = function(Text, CustomColor)
	local Gap = 25
	local Width = 18
	local Alpha = 255
	local Time = 0
	local EStep = 0
	local EEStep = 0.02
	local InSety = 0

	local Note = {
		Enabled = true,
		TargetPos = Vector2.new(50, 33),
		Size = Vector2.new(200, Width),
		
		Drawings = {
		    Outline = Script.Functions.NewDrawing("Square", {Size = Vector2.new(202, Width + 2), Filled = false, Visible = true, Thickness = 1, Position = Vector2.new(), Color = Color3.new(0, 0, 0)}),
			Fade = Script.Functions.NewDrawing("Square", {Size = Vector2.new(202, Width + 2), Filled = false, Visible = true, Thickness = 1, Position = Vector2.new(), Color = Color3.new(0, 0, 0)}),
		},
        
		Remove = function(self, Part)
			if Part.Position.x < Part.Size.x then
				for _, v in pairs(self.Drawings) do
					v:Remove()
					v = false
				end
				self.Enabled = false
			end
		end,

		Update = function(self, Number, ListLength, Dt)
			local Pos = self.TargetPos
			local indexOffset = (ListLength - Number) * Gap
            
			if InSety < indexOffset then
				InSety = InSety - (InSety - indexOffset) * 0.2
			else
				InSety = indexOffset
			end

			local Size = self.Size
			local LastPos = Vector2.new(Pos.x - Size.x / Time - (Alpha / 255 * -Size.x + Size.x), Pos.y + InSety)
			self.Pos = LastPos
			
			local CurrentPos = {
				X = math.ceil(LastPos.x),
				Y = math.ceil(LastPos.y),
				W = math.floor(Size.x - (255 - Alpha) / (255 * 70)),
				H = Size.y,
			}
			
			local Fade = math.min(Time * 12, Alpha)
			Fade = Fade > 255 and 255 or Fade < 0 and 0 or Fade

			if self.Enabled then
				local LineRepeat = 1
				for i, v in pairs(self.Drawings) do
					v.Transparency = Fade / 255
					if type(i) == ("number") then
						v.Position = Vector2.new(CurrentPos.X + 1, CurrentPos.Y + i)
						v.Size = Vector2.new(CurrentPos.W - 2, 1)
					elseif i == ("Image") then
					    v.Position = LastPos + Vector2.new(6, 2)
					elseif i == ("Text") then
						v.Position = LastPos + Vector2.new(25, 2)
					elseif i == ("Outline") then
						v.Position = Vector2.new(CurrentPos.X, CurrentPos.Y)
						v.Size = Vector2.new(CurrentPos.W, CurrentPos.H)
					elseif i == ("Fade") then
						v.Position = Vector2.new(CurrentPos.X - 1, CurrentPos.Y - 1)
						v.Size = Vector2.new(CurrentPos.W + 2, CurrentPos.H + 2)
						local T = (200 - Fade) / 255 / 3
						v.Transparency = T < 0.4 and 0.4 or T
					elseif i:find("line") then
						v.Position = Vector2.new(CurrentPos.X + LineRepeat, CurrentPos.Y + 1)
						LineRepeat = LineRepeat + 1
					end
				end

				Time = Time + EStep * Dt * 128 
				EStep = EStep + EEStep * Dt * 64
			end
		end,

		Fade = function(self, Number, Len, Dt)
			if self.Pos.x > self.TargetPos.x - 0.2 * Len or self.Fading then
				if not self.Fading then
					EStep = 0
				end
				self.Fading = true
				Alpha = Alpha - EStep / 4 * Len * Dt * 50
				EEStep = EEStep + 0.01 * Dt * 100
			end
			
            if Alpha <= 0 then
				self:Remove(self.Drawings[1])
			end
		end,
	}

    for i = 1, Note.Size.y - 2 do
    	local X = 0.28 - i / 80
    	Note.Drawings[i] = Script.Functions.NewDrawing("Square", {Size = Vector2.new(200, 1), Filled = true, Visible = true, Thickness = 1, Position = Vector2.new(), Color = Color3.new(X, X, X)})
    end
    
    local C = CustomColor or Color3.fromRGB(206, 67, 67)
    Note.Drawings.Text = Script.Functions.NewDrawing("Text", {Size = 13,Text = Text , Visible = true, Center = false, Outline = true, Font = 2, Position = Vector2.new(), Color = Color3.new(1, 1, 1)})
    Note.Drawings.Image = Script.Functions.NewDrawing("Image", {Size = Vector2.new(15,15), Visible = true, Position = Vector2.new(), Data = VNMLogo})

    if Note.Drawings.Text.TextBounds.x + 7 > Note.Size.x then
    	Note.Size = Vector2.new(Note.Drawings.Text.TextBounds.x + 27, Note.Size.y)
    end
    
    Note.Drawings.line = Script.Functions.NewDrawing("Square", {Size = Vector2.new(1, Note.Size.y - 2), Filled = true, Visible = true, Thickness = 1, Position = Vector2.new(), Color = C})
    Note.Drawings.line1 = Script.Functions.NewDrawing("Square", {Size = Vector2.new(1, Note.Size.y - 2), Filled = true, Visible = true, Thickness = 1, Position = Vector2.new(), Color = C})
    Script.NotifyNote[#Script.NotifyNote + 1] = Note
end

-- // Gets The Closest Part Of The Character To Cursor
Script.Functions.GetClosestBodyPart = function(Char)
    local Distance = math.huge
    local ClosestPart = nil
    local Filterd = {}

    local Parts = Char:GetChildren()
    for _, v in pairs(Parts) do
        if VNM.Silent.UseWhitelistedParts and table.find(VNM.Silent.WhitelistedPart, v.Name) == nil then continue end
        if v:IsA("MeshPart") and v:IsA("BasePart") and Script.Functions.OnScreen(v) then
            table.insert(Filterd, v)
            for _, v in pairs(Filterd) do                
                local Magnitude = Script.Functions.GetMagnitudeFromMouse(v)
                if Magnitude < Distance then
                    ClosestPart = v
                    Distance = Magnitude
                end
            end
        end
    end
    return ClosestPart
end

-- // Gets An Random Part Of The Character
Script.Functions.GetRandomBodyPart = function(Char)
    local CurrentParts = {}
    local Parts = Char:GetChildren()
    
    for _, v in pairs(Parts) do
        if VNM.Silent.UseWhitelistedParts and table.find(VNM.Silent.WhitelistedPart, v.Name) == nil then continue end
        if v:IsA("MeshPart") and v:IsA("BasePart") and Script.Functions.OnScreen(v) then
            table.insert(CurrentParts, v)
        end
    end
    
    local Randomize = CurrentParts[math.random(1, #CurrentParts)]
    return Randomize
end

-- // Gets The Closest Point From Cursor
Script.Functions.GetClosestPointOnPart = function(Part)
    local Transform = Part.CFrame:PointToObjectSpace(Mouse.Hit.Position)
    return Part.CFrame * Vector3.new(
        math.clamp(Transform.X, - Part.Size.X % (VNM.Silent.ClosestPointScale / 2), Part.Size.X % (VNM.Silent.ClosestPointScale / 2)), 
        math.clamp(Transform.Y, - Part.Size.Y % (VNM.Silent.ClosestPointScale / 2), Part.Size.Y % (VNM.Silent.ClosestPointScale / 2)), 
        math.clamp(Transform.Z, - Part.Size.Z % (VNM.Silent.ClosestPointScale / 2), Part.Size.Z % (VNM.Silent.ClosestPointScale / 2))
    )
end

-- // Mod Detection Function
Script.Functions.CheckIfMod = function(Plr)
    if VNM.ModDetection.Enabled then
        if (game:GetService("MarketplaceService"):GetProductInfo(game.PlaceId).Creator.CreatorType == "Group" and true or false) == true then
            local GetId = game:GetService("GroupService"):GetGroupInfoAsync(game:GetService("MarketplaceService"):GetProductInfo(game.PlaceId).Creator.CreatorTargetId).Id
            local GroupId = tonumber(GetId)
            
            if Plr:IsInGroup(GroupId) and Plr:GetRankInGroup(GroupId) > VNM.ModDetection.Rank then
                if VNM.ModDetection.Method == ("Kick") then 
                    task.wait(VNM.ModDetection.Delay)
                    Client:Kick("Detected Moderator / Admin: " .. tostring(Plr))
                elseif VNM.ModDetection.Method == ("Notification") then
                    Script.Functions.CreateNotification("Detected Moderator / Admin: " .. tostring(Plr), Color3.fromRGB(206, 67, 67))
                end
            end
        end
    end
end

-- // Gets The Closest Player For Cursor (Silent Aim)
Script.Functions.GetClosestPlayer = function(Mode)
    local Target, Closest, HitChance = nil, math.huge, Script.Functions.CalculateChance(VNM.Silent.HitChance)
    
    if not HitChance then
        return nil
    end
    if Mode then
        for _, v in pairs(Players:GetPlayers()) do
            if v.Character and v ~= Client and v.Character:FindFirstChild("HumanoidRootPart") then
                if not Script.Functions.OnScreen(v.Character.HumanoidRootPart) then continue end
                if VNM.UniversalCheck.WallCheck and not Script.Functions.WallCheck(v.Character.HumanoidRootPart.Position, v.Character) then continue end
                local Distance = Script.Functions.GetMagnitudeFromMouse(v.Character.HumanoidRootPart)
                if (VNM.Silent.ForceLock and Script.Drawing.SilentCircle.Radius + (Distance * 0.3) < Distance) then continue end
                if Distance < Closest then
                    Closest = Distance
                    Target = v
                end
            end
        end
    else
        if VNM.Silent.ForceLock_AimAssistTarget and Script.Functions.Alive(AimTarget) then
            Target = AimTarget
        elseif VNM.Silent.ForceLock_AimAssistTarget == false then
            Target = ForceLock
        end
    end
    
    if Script.Functions.Alive(Target) then
        local Velocity = Script.Functions.ComPlexVelocity(Target, Target.Character.HumanoidRootPart.Position, false)
        CurrentVelocity = Velocity
    else
        CurrentVelocity = Vector3.zero
    end

    return Target
end

-- // Target Check For Silent Aim Target
Script.Functions.SilentCheck = function(Plr)
    if VNM.UniversalCheck.KoCheck and Plr.Character:FindFirstChild("BodyEffects") then
        local KoCheck = false
        if Plr.Character.BodyEffects:FindFirstChild("K.O") then
            KoCheck = Plr.Character.BodyEffects["K.O"].Value
        elseif Plr.Character.BodyEffects:FindFirstChild("KO") then
            KoCheck = Plr.Character.BodyEffects.KO.Value
        end
        local Grabbed = Plr.Character:FindFirstChild("GRABBING_CONSTRAINT") ~= nil
        if KoCheck or Grabbed then 
            return false 
        end
    end
    if VNM.UniversalCheck.FriendCheck and table.find(Script.Friends, Plr) then
        return false  
    end
    if VNM.UniversalCheck.TargetDeathCheck and Plr.Character:FindFirstChild("Humanoid") and Plr.Character.Humanoid.health < 4 then
        return false 
    end
    if VNM.UniversalCheck.ForceFieldCheck and Plr.Character:FindFirstChildOfClass("ForceField") then
        return false
    end
    if VNM.UniversalCheck.InVisibleCheck and Plr.Character:FindFirstChild("Head") and Plr.Character.Head.Transparency > 0.5 then
        return false  
    end
    if VNM.UniversalCheck.CrewCheck and Script.Functions.FindCrew(Plr) and Plr.DataFolder.Information:FindFirstChild("Crew").Value ~= Client.DataFolder.Information:FindFirstChild("Crew").Value then 
        return false  
    end
    if VNM.UniversalCheck.TeamCheck and Client.Team ~= nil and Plr.Team ~= nil and Plr.Team == Client.Team then 
        return false 
    end
    return true
end

-- // Error Detection Function

-- // Cheater Detection Function

-- // Gets The Closest Part From The Target And Checks If AimAssist Is On To Save Fps
Script.Functions.GetClosestPartMethod = function(Plr)
    local ClosestPart, PartClosest, Filterd = nil, math.huge, {}
    
    if VNM.Silent.LegitMode then
        if VNM.Silent.ClosestPart == false and Plr.Character:FindFirstChild(VNM.Silent.Part) then
            return Plr.Character[VNM.Silent.Part]
        elseif AimTarget and VNM.AimAssist.ClosestPart and Plr.Character:FindFirstChild(VNM.AimAssist.Part) then
            return Plr.Character[VNM.AimAssist.Part]
        end
        return nil
    end
    
    if Script.Functions.Alive(Plr) then
        if SilentTarget.Character.Humanoid:GetState() == Enum.HumanoidStateType.Freefall then
            local AirHitChance = Script.Functions.CalculateChance(VNM.Silent.AirHitChance)
            if not AirHitChance then
                return nil
            end
            if VNM.Silent.UseAirPart and SilentTarget.Character:FindFirstChild(VNM.Silent.AirPart) then
                return SilentTarget.Character[VNM.Silent.AirPart]
            end
        else
            if VNM.Silent.UseAirPart and SilentTarget.Character:FindFirstChild(Script.SavedValue.SilentPart) then
                return SilentTarget.Character[Script.SavedValue.SilentPart]
            end
        end
        
        if VNM.Silent.ClosestPart == false and Plr.Character:FindFirstChild(VNM.Silent.Part) then
            local PartDistance = Script.Functions.GetMagnitudeFromMouse(Plr.Character[VNM.Silent.Part])

            if (VNM.Silent.ForceLock == false and Script.Drawing.SilentCircle.Radius + 4 > PartDistance) or VNM.Silent.ForceLock == true then
                return Plr.Character[VNM.Silent.Part]
            end
            return nil
        end
        
        local Parts = Plr.Character:GetChildren()
        for _, v in pairs(Parts) do
            if VNM.Silent.UseWhitelistedParts and table.find(VNM.Silent.WhitelistedPart, v.Name) == nil then continue end
            if v:IsA("MeshPart") and v:IsA("BasePart") and Script.Functions.OnScreen(v) then
                table.insert(Filterd, v)
                for _, v in pairs(Filterd) do
                    local Mag = Script.Functions.GetMagnitudeFromMouse(v)
                    if (VNM.Silent.ForceLock == false and VNM.Silent.LegitMode == false and Script.Drawing.SilentCircle.Radius + 4 < Mag) then continue end
                    if Mag < PartClosest then
                        ClosestPart = v
                        PartClosest = Mag
                    end
                end
            end
        end
        
        return ClosestPart
    end
    return nil
end

-- // Gets Closest Player From Mouse For AimAssist
Script.Functions.GetClosestPlayer2 = function()
    local Target, Distance, Closest = nil, nil, math.huge
    
    for _, v in pairs(Players:GetPlayers()) do
        if v.Character and v ~= Client and v.Character:FindFirstChild("HumanoidRootPart") then
            if not Script.Functions.OnScreen(v.Character.HumanoidRootPart) then continue end
            if VNM.UniversalCheck.WallCheck and not Script.Functions.WallCheck(v.Character.HumanoidRootPart.Position, v.Character) then continue end
        	if VNM.UniversalCheck.InVisibleCheck and v.Character:FindFirstChild("Head") then
        		if v.Character.Head.Transparency > 0.5 then
        			continue
        		end
        	end
            if VNM.UniversalCheck.ForceField and not v.Character:FindFirstChildOfClass("ForceField") then
                continue
            end
        	if VNM.UniversalCheck.CrewCheck and Script.Functions.FindCrew(v) and v.DataFolder.Information:FindFirstChild("Crew").Value ~= Client.DataFolder.Information:FindFirstChild("Crew").Value then
        		continue
        	end
            if VNM.UniversalCheck.TeamCheck and Client.Team ~= nil and v.Team ~= nil and v.Team ~= Client.Team then
                continue
            end
            if VNM.UniversalCheck.FriendCheck and table.find(Script.Friends, v) ~= nil then
                continue
            end
            local Distance = Script.Functions.GetMagnitudeFromMouse(v.Character.HumanoidRootPart)
            if Distance < Closest then
                if (VNM.AimAssist.UseCircleRadius and Script.Drawing.AimAssistCircle.Radius + (Distance * 0.3) < Distance) then continue end
                Closest = Distance
                Target = v
            end
        end
    end

    return Target
end

-- // Main Functions For The BeizerManager Functions
Script.BeizerManager.__index = Script.BeizerManager

-- // The Main Function That Makes It Work
Script.BeizerManager.New = function()
    local self = setmetatable({}, Script.BeizerManager)
    
    self.T = 0
    self.T_Threshold = 0.99995
    self.StartPoint = Vector2.new()
    self.EndPoint = Vector2.new()
    self.CurvePoints = {
        Vector2.new(1, 1),
        Vector2.new(1, 1)
    }
    self.Active = false
    self.Smoothness = 0.0025
    self.DrawPath = false
    self.Function = Script.Functions.MouseOffset

    self.Started = false

    return self
end

-- // Changes Current self Propertiesself.T 
Script.BeizerManager.ChangeData = function(self, Data)
    self.StartPoint = (self.GetStartPoint() or Data.StartPoint)
    self.EndPoint = self.ModifyEndPoint(Data.TargetPosition)
    self.Smoothness = Data.Smoothness or self.Smoothness
    self.CurvePoints = Data.CurvePoints or self.CurvePoints
    self.DrawPath = Data.DrawPath or self.DrawPath

    self.T = 0
    self.Active = true
end

-- // Calculates Every Points For More Accuracy
Script.BeizerManager.CubicCurve = function(T, StartPoint, EndPoint, ControlPointA, ControlPointB)
    local T1 = (1 - T)

    local A = T1^3 * StartPoint
    local B = 3 * T1^2 * T * ControlPointA
    local C = 3 * T1 * T^2 * ControlPointB
    local D = T^3 * EndPoint
    
    return A + B + C + D
end

-- // Controls And Calculates StartPoint And Other Stuff
Script.BeizerManager.DoControlPoint = function(StartPoint, EndPoint, ControlPointA, ControlPointB)
    local Change = (EndPoint - StartPoint)

    local A = StartPoint + (Change * ControlPointA)
    local B = StartPoint + (Change * ControlPointB)
    return A, B
end

-- // Draws Circle Where The CurvePosition And The Client Position
Script.BeizerManager.DrawPathFunc = function(CurvePosition, A, B)
    local Path = Script.Functions.NewDrawing("Circle", {Color = Color3.fromRGB(255, 150, 150), Radius = 2, Visible = true, Position = CurvePosition,})
    task.delay(1, function()
        Path:Remove()
    end)

    local ControlPointA = Script.Functions.NewDrawing("Circle", {Color = Color3.fromRGB(255, 150, 255), Radius = 5, Visible = true, Position = A,})
    task.delay(1, function()
        ControlPointA:Remove()
    end)

    local ControlPointB = Script.Functions.NewDrawing("Circle", {Color = Color3.fromRGB(255, 150, 255), Radius = 5, Visible = true, Position = B,})
    task.delay(1, function()
        ControlPointB:Remove()
    end)
end

-- // Changes The self For The Curving To Happend
Script.BeizerManager.DoIteration = function(self)
    if (self.Active == false) then
        return
    end
    local BeizerCurve = self.CubicCurve
    local T = self.T
    while (T <= 1 and self.Active) do RS.RenderStepped:Wait()
        T = T + self.Smoothness
        if (T >= self.T_Threshold) then
            local clampedT = math.clamp(T, 0, 1)
            local New = self.StartPoint:Lerp(self.EndPoint, clampedT)

            self:Function(New.X, New.Y)
        else
            local A, B = self.DoControlPoint(self.StartPoint, self.EndPoint, unpack(self.CurvePoints))
            local CurvePosition = BeizerCurve(T, self.StartPoint, self.EndPoint, A, B)
                
            if (self.DrawPath) then
                Script.BeizerManager.DrawPathFunc(CurvePosition, A, B)
            end
            self:Function(CurvePosition.X, CurvePosition.Y)
        end
    end
    self.Active = false
end

-- // Modifies The EndPoint Just For Double Checks
Script.BeizerManager.ModifyEndPoint = function(EndPoint)
    return EndPoint
end

-- // Starts The Motion
Script.BeizerManager.Start = function(self)
    self.Started = true

    local Thread = coroutine.resume(coroutine.create(function()
        while (self.Started) do RS.RenderStepped:Wait()
            self:DoIteration()
        end
    end))

    return Thread
end

-- // Stops The Motion
Script.BeizerManager.Stop = function(self)
    self.Started = false
end

-- // Stops The Current Motion
Script.BeizerManager.StopCurrent = function(self)
    self.Active = false
    self.T = 0
end

-- // Gets Camera Instead Of Mouse
Script.BeizerManager.CameraMode = function(self)
    self.GetStartPoint = function()
        local Pitch, Yaw, _ = Camera.CFrame:ToEulerAnglesYXZ()
        local StartPoint = Vector2.new(Pitch, Yaw)
        return StartPoint
    end

    self.ModifyEndPoint = function(EndPoint)
        local LookAtEndPoint = CFrame.lookAt(Camera.CFrame.Position, EndPoint)
        local Pitch, Yaw, _ = LookAtEndPoint:ToEulerAnglesYXZ()
        EndPoint = Vector2.new(Pitch, Yaw)
        return EndPoint
    end

    self.Function = function(self, Pitch, Yaw)
        local RotationMatrix = CFrame.fromEulerAnglesYXZ(Pitch, Yaw, 0)
        Camera.CFrame = CFrame.new(Camera.CFrame.Position) * RotationMatrix
    end

    self.DrawPathFunc = function()
        -- // Working On It
    end
end

-- // Gets Mouse Position ServerSided
Script.BeizerManager.GetStartPoint = function()
    return Uis:GetMouseLocation()
end

-- // Gets Mouse Instead Of Camera
Script.BeizerManager.MouseMode = function(self)
    self.GetStartPoint = Script.BeizerManager.GetStartPoint
    self.ModifyEndPoint = Script.BeizerManager.ModifyEndPoint
    self.Function = Script.Functions.MouseOffset
    self.DrawPathFunc = Script.BeizerManager.DrawPathFunc
end

-- // Start BeizerCurve
local ManagerA = Script.BeizerManager.New()
local ManagerB = Script.BeizerManager.New()
Script.BeizerCurve.ManagerA = ManagerA
Script.BeizerCurve.ManagerB = ManagerB

Script.BeizerCurve.AimTo = function(...)
    ManagerA:ChangeData(...)
end

Script.BeizerCurve.AimToB = function(...)
    ManagerB:ChangeData(...)
end

ManagerB:CameraMode()

ManagerB.Function = function(self, Pitch, Yaw)
    local RotationMatrix = CFrame.fromEulerAnglesYXZ(Pitch, Yaw, 0)
    Utilities.SetCameraCFrame(CFrame.new(Camera.CFrame.Position) * RotationMatrix)
end

ManagerA:Start()
ManagerB:Start()

-- // Silent Get Part Position
Script.Functions.GetPartPosition = function(Plr)
    local TargetCF = nil
    local TargetFalling = false
    local SilentPos = nil
    
    if VNM.Silent.ClosestPoint then
        TargetCF = ClosestPointCF
    else
        if Plr.Character:FindFirstChild(VNM.Silent.Part) then
            TargetCF = Plr.Character[VNM.Silent.Part].Position
        end
    end

    if VNM.Silent.AntiGroundShots and CurrentVelocity.Y < VNM.Silent.AntiGroundActivation then
        TargetFalling = true
    end
    if TargetCF and CurrentVelocity then
        SilentPos = TargetCF
        if VNM.Silent.PredictMovement then 
            if VNM.Silent.BlatantMode then
                local Enabled = true
                local Mag = (Plr.Character.Humanoid.MoveDirection).Magnitude
                local SilentVel = Plr.Character.HumanoidRootPart.Velocity
                if (SilentVel).Magnitude > 100 then
                    Enabled = false
                elseif SilentVel.Y > 50 then
                    Enabled = false
                elseif SilentVel.Y < -35 then
                    Enabled = false
                elseif SilentVel.Y > 75 then
                    Enabled = false
                elseif (SilentVel).Magnitude < 1 and Mag > 0.01 then
                    Enabled = false
                elseif (SilentVel).Magnitude > 5 and Mag < 0.01 then
                    Enabled = false
                end
                if Enabled and VNM.UniversalCheck.WallCheck_V2 and not Script.Functions.WallCheck(Plr.Character.HumanoidRootPart.Position + (SilentTarget.Character.HumanoidRootPart.Velocity * VNM.Silent.Prediction), Plr.Character) then
                    return
                end
                if Enabled then
                    if TargetFalling then
                        SilentPos = SilentPos + (Vector3.new(SilentVel.X, (SilentVel.Y * VNM.Silent.AntiGroundValue), SilentVel.Z) * VNM.Silent.Prediction)
                    else
                        SilentPos = SilentPos + (SilentVel * VNM.Silent.Prediction)
                    end
                else
                    if TargetFalling then
                        SilentPos = SilentPos + (Vector3.new(CurrentVelocity.X, (CurrentVelocity.Y * VNM.Silent.AntiGroundValue), CurrentVelocity.Z) * VNM.Silent.Prediction)
                    else
                        SilentPos = SilentPos + (CurrentVelocity * VNM.Silent.Prediction)
                    end
                end
            else
                if TargetFalling then
                    SilentPos = SilentPos + (Vector3.new(CurrentVelocity.X, (CurrentVelocity.Y * VNM.Silent.AntiGroundValue), CurrentVelocity.Z) * VNM.Silent.Prediction)
                else
                    SilentPos = SilentPos + (CurrentVelocity * VNM.Silent.Prediction)
                end
            end
        end
        if VNM.Silent.Humanize then
            local HumanizeValue = VNM.Silent.HumanizeValue 
            SilentPos = (SilentPos + Script.Functions.RandomVec3(HumanizeValue, 0.01))
        end
    end
    if SilentPos then
        if VNM.Silent.LegitMode then
            local PartPos = Camera:WorldToScreenPoint(SilentPos)
            local GetScreenPos = Script.Functions.GetFovPosition()
            local Magnitude = (Vector2.new(PartPos.X, PartPos.Y) - GetScreenPos).Magnitude

            if (VNM.Silent.ForceLock == false and Script.Drawing.SilentCircle.Radius + 4 > Magnitude) or VNM.Silent.ForceLock == true then
                return SilentPos
            end
        else
            return SilentPos
        end
    end
    return nil
end

local MousePosChanger2 = nil 
MousePosChanger2 = hookmetamethod(game, "__index", function(self, Index)
    if not checkcaller() and VNM.Silent.Enabled and VNM.Silent.AntiAimViewer == false and self == Mouse and Script.Functions.Alive(SilentTarget) then
        if Index == ("Hit") then
            local EndPoint = Script.Functions.GetPartPosition(SilentTarget)
            if EndPoint then
                return EndPoint
            end
        elseif Index == ("Target") and SilentTarget.Character:FindFirstChild(VNM.Silent.Part) then 
            return SilentTarget.Character[VNM.Silent.Part]
        end
    end
    return MousePosChanger2(self, Index)
end)

-- // The AimAssist Mouse Dragging/Check Functions
Script.Functions.MouseChanger = function()
    if VNM.AimAssist.Enabled and not Uis:GetFocusedTextBox() and Script.Functions.Alive(AimTarget) and Script.Functions.Alive(Client) and AimTarget.Character:FindFirstChild(VNM.AimAssist.Part) then
        local EndPosition = nil
        local TargetPos = AimTarget.Character[VNM.AimAssist.Part].Position

        if VNM.AimAssist.ThirdPerson and VNM.AimAssist.FirstPerson == false then
            if (Camera.Focus.p - Camera.CoordinateFrame.p).Magnitude < 1 then
                return
            end
        elseif VNM.AimAssist.ThirdPerson == false and VNM.AimAssist.FirstPerson then
            if (Camera.Focus.p - Camera.CoordinateFrame.p).Magnitude > 1 then
                return
            end
        end
        CurrentVelocity2 = Script.Functions.ComPlexVelocity(AimTarget, AimTarget.Character.HumanoidRootPart.Position, true)
        if CurrentVelocity2 == nil then return end
        if VNM.AimAssist.EnableChance then
            local Chance = Script.Functions.CalculateChance(VNM.AimAssist.Chance)
            if not Chance then
                return
            end
        end
        if VNM.UniversalCheck.ToolOut and not Client.Character:FindFirstChildWhichIsA("Tool") then
            return
        end
        if VNM.UniversalCheck.ForceFieldCheck and AimTarget.Character:FindFirstChildOfClass("ForceField") then
            AimTarget = nil
            CurrentVelocity2 = Vector3.zero
            return
        end
        if VNM.AimAssist.RandomPart or VNM.AimAssist.ClosestPart then
            if Script.Functions.OnScreen(AimTarget.Character.HumanoidRootPart) then
                if VNM.AimAssist.RandomPart then
                    local RandomizedPart = Script.Functions.GetRandomBodyPart(AimTarget.Character)
                    if RandomizedPart ~= nil then
                        VNM.AimAssist.Part = tostring(RandomizedPart)
                    end
                elseif VNM.AimAssist.ClosestPart then
                    local ClosestPart = Script.Functions.GetClosestBodyPart(AimTarget.Character)
                    VNM.AimAssist.Part = tostring(ClosestPart)
                end
            else
                if VNM.AimAssist.Method == ("Mouse") then
                    return
                end
            end
        end
        if VNM.UniversalCheck.ReloadCheck then
            for _, v in pairs(Client.Character.Humanoid.Animator:GetPlayingAnimationTracks()) do
                if tostring(v) == ("Animation") then
                    return
                end
            end
        end
        if VNM.UniversalCheck.NoAmmoCheck and Client.Character:FindFirstChildWhichIsA("Tool") and Client.Character:FindFirstChildWhichIsA("Tool"):FindFirstChild("Ammo") then
            if Client.Character:FindFirstChildWhichIsA("Tool"):FindFirstChild("Ammo").Value <= 0 then
                return
            end
        end
        if VNM.AimAssist.Advanced.WallCheck_V2 and not Script.Functions.WallCheck(AimTarget.Character.HumanoidRootPart.Position, AimTarget.Character) then 
            return
        end
        if VNM.AimAssist.DisableOutSideCircle then
            local Magnitude = Script.Functions.GetMagnitudeFromMouse(AimTarget.Character.HumanoidRootPart)
            if (Script.Drawing.AimAssistCircle.Radius + 4) < Magnitude then
                return
            end
        end
        if VNM.UniversalCheck.PlayerDeathCheck then
            if Client.Character.Humanoid.health < 4 then
                AimTarget = nil
                CurrentVelocity2 = Vector3.zero
                return
            end
        end
        if VNM.UniversalCheck.KoCheck and AimTarget.Character:FindFirstChild("BodyEffects") then 
            local Grabbed = AimTarget.Character:FindFirstChild("GRABBING_CONSTRAINT") ~= nil
            local KoCheck = true
            if AimTarget.Character.BodyEffects:FindFirstChild("K.O") then
                KoCheck = AimTarget.Character.BodyEffects["K.O"].Value
            elseif AimTarget.Character.BodyEffects:FindFirstChild("KO") then
                KoCheck = AimTarget.Character.BodyEffects.KO.Value
            end
            if KoCheck or Grabbed then
                AimTarget = nil
                CurrentVelocity2 = Vector3.zero
                return
            end
        end
        if VNM.UniversalCheck.TargetDeathCheck then
            if AimTarget.Character.Humanoid.health < 4 then
                AimTarget = nil
                CurrentVelocity2 = Vector3.zero
                return
            end
        end
        if VNM.AimAssist.Shake.AirShake or VNM.AimAssist.AirSmoothness then
            if AimTarget.Character.Humanoid:GetState() == Enum.HumanoidStateType.Freefall then
                if VNM.AimAssist.AirSmoothness then
                    VNM.AimAssist.Smoothness_X = VNM.AimAssist.AirSmoothness_X
                    VNM.AimAssist.Smoothness_X = VNM.AimAssist.AirSmoothness_Y
                end
                if VNM.AimAssist.Shake.AirShake then
                    if VNM.AimAssist.Shake.Shake_X == Script.SavedValue.ShakeX then
                        VNM.AimAssist.Shake.Shake_X = VNM.AimAssist.Shake.Shake_X * (VNM.AimAssist.Shake.AirPercentage / 100)
                    end
                    if VNM.AimAssist.Shake.Shake_Y == Script.SavedValue.ShakeY then
                        VNM.AimAssist.Shake.Shake_Y = VNM.AimAssist.Shake.Shake_Y * (VNM.AimAssist.Shake.AirPercentage / 100)
                    end
                    if VNM.AimAssist.Shake.Shake_Z == Script.SavedValue.ShakeZ then
                        VNM.AimAssist.Shake.Shake_Z = VNM.AimAssist.Shake.Shake_Z * (VNM.AimAssist.Shake.AirPercentage / 100)
                    end
                end
            else
                if VNM.AimAssist.AirSmoothness then
                    if VNM.AimAssist.Smoothness_X ~= Script.SavedValue.AimAssistSmoothX then
                        VNM.AimAssist.Smoothness_X = Script.SavedValue.AimAssistSmoothX
                    end
                    if VNM.AimAssist.Smoothness_Y ~= Script.SavedValue.AimAssistSmoothY then
                        VNM.AimAssist.Smoothness_Y = Script.SavedValue.AimAssistSmoothY
                    end
                end
                if VNM.AimAssist.Shake.AirShake then
                    if VNM.AimAssist.Shake.Shake_X ~= Script.SavedValue.ShakeX then
                        VNM.AimAssist.Shake.Shake_X = Script.SavedValue.ShakeX
                    end
                    if VNM.AimAssist.Shake.Shake_Y ~= Script.SavedValue.ShakeY then
                        VNM.AimAssist.Shake.Shake_Y = Script.SavedValue.ShakeY
                    end
                    if VNM.AimAssist.Shake.Shake_Z ~= Script.SavedValue.ShakeZ then
                        VNM.AimAssist.Shake.Shake_Z = Script.SavedValue.ShakeZ
                    end
                end
            end
        end
        
        if FrameSkip and VNM.AimAssist.FrameSkip.TargetPart.Enabled and AimTarget.Character:FindFirstChild(VNM.AimAssist.FrameSkip.TargetPart.Part) then
            if VNM.AimAssist.FrameSkip.UsePrediction then
                EndPosition = AimTarget.Character[VNM.AimAssist.FrameSkip.TargetPart.Part].Position + (CurrentVelocity2 * VNM.AimAssist.Prediction)
            else
                EndPosition = AimTarget.Character[VNM.AimAssist.FrameSkip.TargetPart.Part].Position
            end
        elseif FrameSkip then
            if VNM.AimAssist.FrameSkip.UsePrediction then
                EndPosition = TargetPos + (CurrentVelocity2 * VNM.AimAssist.Prediction)
            else
                EndPosition = TargetPos
            end
        elseif VNM.AimAssist.PredictMovement then
            EndPosition = TargetPos + (CurrentVelocity2 * VNM.AimAssist.Prediction)
        else
            EndPosition = TargetPos
        end

        if (EndPosition and (tick() - LastStutter) >= (VNM.AimAssist.Advanced.Stutter / 1000)) then
            LastStutter = tick()
            if VNM.AimAssist.Shake.Enabled and FrameSkip == false then
                local Mag = math.ceil((EndPosition - Client.Character.HumanoidRootPart.Position).Magnitude)
                EndPosition = EndPosition + (Vector3.new(
                    math.random(-Mag * VNM.AimAssist.Shake.Shake_X, Mag * VNM.AimAssist.Shake.Shake_X), 
                    math.random(-Mag * VNM.AimAssist.Shake.Shake_Y, Mag * VNM.AimAssist.Shake.Shake_Y), 
                    math.random(-Mag * VNM.AimAssist.Shake.Shake_Z, Mag * VNM.AimAssist.Shake.Shake_Z)) / 1000
                )
            end
            if VNM.AimAssist.Method == ("Mouse") then
                local Vec2Pos = Camera:WorldToScreenPoint(EndPosition)
                local InCrementX = (Vec2Pos.X - VNM.Silent.Fov.Offset.X) * (VNM.AimAssist.UseSmoothness and VNM.AimAssist.Smoothness_X or 1)
                local InCrementY = (Vec2Pos.Y - VNM.Silent.Fov.Offset.Y) * (VNM.AimAssist.UseSmoothness and VNM.AimAssist.Smoothness_Y or 1)
                if FrameSkip then
                    FrameSkip = false
                    InCrementX = ((Vec2Pos.X - VNM.Silent.Fov.Offset.X) - Mouse.X) * VNM.AimAssist.FrameSkip.Power
                    InCrementY = ((Vec2Pos.Y - VNM.Silent.Fov.Offset.Y) - Mouse.Y) * VNM.AimAssist.FrameSkip.Power
                end
                mousemoverel(InCrementX, InCrementY)
            elseif VNM.AimAssist.Method == ("Camera") then
                local Vec3Pos = CFrame.new(Camera.CFrame.p, EndPosition)
                if FrameSkip then
                    FrameSkip = false
                    Camera.CFrame = Camera.CFrame:Lerp(Vec3Pos, VNM.AimAssist.FrameSkip.Power, VNM.AimAssist.Advanced.EasingStyle, VNM.AimAssist.Advanced.EasingDirection)
                    return
                end
                Camera.CFrame = Camera.CFrame:Lerp(Vec3Pos, (VNM.AimAssist.UseSmoothness and VNM.AimAssist.Smoothness_X or 1), VNM.AimAssist.Advanced.EasingStyle, VNM.AimAssist.Advanced.EasingDirection)
            end
        end
    end
end

-- // Silent Features That Needs To Be Looping
Script.Functions.SilentMisc = function()
    if VNM.Silent.PingPrediction.Enabled and VNM.Silent.PredictMovement and VNM.Silent.GunSettings.Methods.Prediction == false and game:GetService("Stats") and game:GetService("Stats"):FindFirstChild("Network") and game:GetService("Stats").Network:FindFirstChild("ServerStatsItem") and game:GetService("Stats").Network.ServerStatsItem:FindFirstChild("Data Ping") then
        local Ping = math.floor(game:GetService("Stats").Network.ServerStatsItem["Data Ping"]:GetValue())
        if VNM.Silent.PingPrediction.AutoMatic == false then
            if Ping > 200 then
                VNM.Silent.Prediction = VNM.Silent.PingPrediction.P200_Inf
            elseif Ping > 190 then
                VNM.Silent.Prediction = VNM.Silent.PingPrediction.P190_200
            elseif Ping > 180 then
                VNM.Silent.Prediction = VNM.Silent.PingPrediction.P180_190
            elseif Ping > 170 then
                VNM.Silent.Prediction = VNM.Silent.PingPrediction.P170_180
            elseif Ping > 160 then
                VNM.Silent.Prediction = VNM.Silent.PingPrediction.P160_170
            elseif Ping > 150 then
                VNM.Silent.Prediction = VNM.Silent.PingPrediction.P150_160
            elseif Ping > 140 then
                VNM.Silent.Prediction = VNM.Silent.PingPrediction.P140_150
            elseif Ping > 130 then
                VNM.Silent.Prediction = VNM.Silent.PingPrediction.P130_140
            elseif Ping > 120 then
                VNM.Silent.Prediction = VNM.Silent.PingPrediction.P120_130
            elseif Ping > 110 then
                VNM.Silent.Prediction = VNM.Silent.PingPrediction.P110_120
            elseif Ping > 100 then
                VNM.Silent.Prediction = VNM.Silent.PingPrediction.P100_110
            elseif Ping > 90 then
                VNM.Silent.Prediction = VNM.Silent.PingPrediction.P90_100
            elseif Ping > 80 then
                VNM.Silent.Prediction = VNM.Silent.PingPrediction.P80_90
            elseif Ping > 70 then
                VNM.Silent.Prediction = VNM.Silent.PingPrediction.P70_80
            elseif Ping > 60 then
                VNM.Silent.Prediction = VNM.Silent.PingPrediction.P60_70
            elseif Ping > 50 then
                VNM.Silent.Prediction = VNM.Silent.PingPrediction.P50_60
            elseif Ping > 40 then
                VNM.Silent.Prediction = VNM.Silent.PingPrediction.P40_50
            elseif Ping > 30 then
                VNM.Silent.Prediction = VNM.Silent.PingPrediction.P30_40
            elseif Ping > 20 then
                VNM.Silent.Prediction = VNM.Silent.PingPrediction.P20_30
            elseif Ping > 10 then
                VNM.Silent.Prediction = VNM.Silent.PingPrediction.P10_20
            end
        end
    end
end

-- // Gets Silent Target And Velocity
Script.Functions.GetSilentTarget = function()
    local Target = Script.Functions.GetClosestPlayer(not VNM.Silent.ForceLock)

    if VNM.Silent.AntiAimViewer == false and Script.Functions.Alive(Target) and Script.Functions.SilentCheck(Target) then 
        SilentTarget = Target
        local GetPart = Script.Functions.GetClosestPartMethod(Target)
        if GetPart ~= nil then
            VNM.Silent.Part = tostring(GetPart)
            Script.SavedValue.SilentPart = tostring(GetPart)
            if VNM.Silent.ClosestPoint then
                ClosestPointCF = Script.Functions.GetClosestPointOnPart(GetPart)
            end
        else
            SilentTarget = nil
        end
    else
        SilentTarget = Target
    end
end

-- // Desync Main Function. Velocity Change
Script.Functions.Desync = function()
    if Desync and Script.Functions.Alive(Client) and Client.Character.Humanoid.health > VNM.Desync.HealthDeActivation then
        if VNM.Desync.Method == ("Freeze_Pos") and sethiddenproperty then
            FreezePos = not FreezePos
            sethiddenproperty(Client.Character.HumanoidRootPart, "NetworkIsSleeping", FreezePos)
        elseif VNM.Desync.Method == ("Slow_Data") and setfflag then
            setfflag("S2PhysicsSenderRate", 3)
        else
            local SaveVelocity = Client.Character.HumanoidRootPart.AssemblyLinearVelocity
            local SaveRotVelocity = Client.Character.HumanoidRootPart.AssemblyAngularVelocity
            if VNM.Desync.Method == ("Vel_StandBy") then
                Client.Character.HumanoidRootPart.Velocity = Vector3.new(1,1,1) * (2^16)
                Client.Character.HumanoidRootPart.RotVelocity = Vector3.new(1,1,1) * (2^16)
            elseif VNM.Desync.Method == ("Vel_Multi") then
                Client.Character.HumanoidRootPart.Velocity = Client.Character.HumanoidRootPart.Velocity * VNM.Desync.Power
            elseif VNM.Desync.Method == ("Custom_Vel") then
                Client.Character.HumanoidRootPart.Velocity = Vector3.new(VNM.Desync.Custom.Vel_X, VNM.Desync.Custom.Vel_Y, VNM.Desync.Custom.Vel_Z)
            elseif VNM.Desync.Method == ("Vel_Under") then
                Client.Character.HumanoidRootPart.Velocity = Vector3.new(0, -VNM.Desync.Power, 0)
            elseif VNM.Desync.Method == ("Vel_Over") then
                Client.Character.HumanoidRootPart.Velocity = Vector3.new(0, VNM.Desync.Power, 0)
            elseif VNM.Desync.Method == ("Vel_Zero") then
                Client.Character.HumanoidRootPart.Velocity = Vector3.zero
            end
            
            if VNM.Desync.Visualize.Enabled then
                local GetMag = (Camera.CoordinateFrame.p - (Client.Character.HumanoidRootPart.Position + (Client.Character.HumanoidRootPart.Velocity * 0.13))).Magnitude
                local GetPos, OnScreen = Camera:WorldToScreenPoint(Client.Character.HumanoidRootPart.Position + (Client.Character.HumanoidRootPart.Velocity * 0.13))
                if OnScreen then
                    Script.Drawing.DesyncCircle.Visible = true
                    Script.Drawing.DesyncCircle.Color = VNM.Desync.Visualize.Color
                    Script.Drawing.DesyncCircle.Radius = (VNM.Desync.Visualize.Radius * 10) / GetMag
                    Script.Drawing.DesyncCircle.Position = Vector2.new(GetPos.X, GetPos.Y)
                else
                    Script.Drawing.DesyncCircle.Visible = false
                end
            else
                Script.Drawing.DesyncCircle.Visible = false
            end
            
            RS.RenderStepped:Wait()
            
            Client.Character.HumanoidRootPart.AssemblyLinearVelocity = SaveVelocity
            Client.Character.HumanoidRootPart.AssemblyAngularVelocity = SaveRotVelocity
        end
    else
        if VNM.Desync.Visualize.Enabled then
            Script.Drawing.DesyncCircle.Visible = false
        end
    end
end

-- // Cheater Detection Function. Not Giving Away

-- // Update Properties Of Circle
Script.Functions.UpdateFOV = function()
    local GetScreenPos = Script.Functions.GetFovPosition()
    Script.Drawing.AimAssistCircle.Visible = VNM.AimAssist.Fov.Visible
    Script.Drawing.AimAssistCircle.Filled = VNM.AimAssist.Fov.Filled
    Script.Drawing.AimAssistCircle.Color = VNM.AimAssist.Fov.Color
    Script.Drawing.AimAssistCircle.Transparency = VNM.AimAssist.Fov.Transparency
    if VNM.Silent.Fov.Method == ("Screen") then
        Script.Drawing.AimAssistCircle.Position = Vector2.new(GetScreenPos.X, GetScreenPos.Y) - VNM.Silent.Fov.Offset
    else
        Script.Drawing.AimAssistCircle.Position = Vector2.new(GetScreenPos.X, GetScreenPos.Y + GuiS:GetGuiInset().Y) - VNM.Silent.Fov.Offset
    end

	Script.Drawing.AimAssistCircle.Radius = VNM.AimAssist.Fov.Radius * 3
    
    Script.Drawing.SilentCircle.Visible = VNM.Silent.Fov.Visible
    Script.Drawing.SilentCircle.Color = VNM.Silent.Fov.Color
    Script.Drawing.SilentCircle.Filled = VNM.Silent.Fov.Filled
    Script.Drawing.SilentCircle.Transparency = VNM.Silent.Fov.Transparency
    if VNM.Silent.Fov.StickyFov and Script.Functions.Alive(SilentTarget) and SilentTarget.Character:FindFirstChild(VNM.Silent.Part) then
        local PartPos, OnScreen = Camera:WorldToViewportPoint(SilentTarget.Character[VNM.Silent.Part].Position)
        if OnScreen then
            local Magnitude = ((Vector2.new(PartPos.X, PartPos.Y) - Vector2.new(GetScreenPos.X, GetScreenPos.Y + GuiS:GetGuiInset().Y)) - VNM.Silent.Fov.Offset).Magnitude

            if (VNM.Silent.ForceLock == false and Script.Drawing.SilentCircle.Radius + 4 > Magnitude) or VNM.Silent.ForceLock == true then
                Script.Drawing.SilentCircle.Position = Vector2.new(PartPos.X, PartPos.Y) - VNM.Silent.Fov.Offset
            else
                if VNM.Silent.Fov.Method == ("Screen") then
                    Script.Drawing.SilentCircle.Position = Vector2.new(GetScreenPos.X, GetScreenPos.Y) - VNM.Silent.Fov.Offset
                else
                    Script.Drawing.SilentCircle.Position = Vector2.new(GetScreenPos.X, GetScreenPos.Y + GuiS:GetGuiInset().Y) - VNM.Silent.Fov.Offset
                end
            end
        else
            if VNM.Silent.Fov.Method == ("Screen") then
                Script.Drawing.SilentCircle.Position = Vector2.new(GetScreenPos.X, GetScreenPos.Y) - VNM.Silent.Fov.Offset
            else
                Script.Drawing.SilentCircle.Position = Vector2.new(GetScreenPos.X, GetScreenPos.Y + GuiS:GetGuiInset().Y) - VNM.Silent.Fov.Offset
            end
        end
    else
        if VNM.Silent.Fov.Method == ("Screen") then
            Script.Drawing.SilentCircle.Position = Vector2.new(GetScreenPos.X, GetScreenPos.Y) - VNM.Silent.Fov.Offset
        else
            Script.Drawing.SilentCircle.Position = Vector2.new(GetScreenPos.X, GetScreenPos.Y + GuiS:GetGuiInset().Y) - VNM.Silent.Fov.Offset
        end
    end
	Script.Drawing.SilentCircle.Radius = SilentFovRadius.Value * 3
end

-- // Updates Esp Posistions
Script.Functions.UpdateEsp = function()
    for i, v in pairs(Script.EspPlayers) do
        if VNM.Esp.Enabled and i.Character and i.Character:FindFirstChild("Humanoid") and i.Character:FindFirstChild("HumanoidRootPart") and i.Character:FindFirstChild("Head") then
            local Hum = i.Character.Humanoid
            local Hrp = i.Character.HumanoidRootPart
            
            local Vector, OnScreen = Camera:WorldToViewportPoint(i.Character.HumanoidRootPart.Position)
            local Size = (Camera:WorldToViewportPoint(Hrp.Position - Vector3.new(0, 3, 0)).Y - Camera:WorldToViewportPoint(Hrp.Position + Vector3.new(0, 2.6, 0)).Y) / 2
            local BoxSize = Vector2.new(math.floor(Size * 1.5), math.floor(Size * 1.9))
            local BoxPos = Vector2.new(math.floor(Vector.X - Size * 1.5 / 2), math.floor(Vector.Y - Size * 1.6 / 2))
            local BottomOffset = BoxSize.Y + BoxPos.Y + 1
            
            if OnScreen then
                if VNM.Esp.Name.Enabled then
                    v.Name.Position = Vector2.new(BoxSize.X / 2 + BoxPos.X, BoxPos.Y - 16)
                    v.Name.Outline = VNM.Esp.Name.OutLine
                    v.Name.Text = Hum.DisplayName
                    v.Name.Color = VNM.Esp.Name.Color
                    v.Name.OutlineColor = Color3.fromRGB(0, 0, 0)
                    v.Name.Font = 0
                    v.Name.Size = VNM.Esp.TextSize

                    v.Name.Visible = true
                else
                    v.Name.Visible = false
                end

                if VNM.Esp.Distance.Enabled and Client.Character and Client.Character:FindFirstChild("HumanoidRootPart") then
                    v.Distance.Position = Vector2.new(BoxSize.X / 2 + BoxPos.X, BottomOffset)
                    v.Distance.Outline = VNM.Esp.Distance.OutLine
                    v.Distance.Text = "[" .. math.floor((Hrp.Position - Client.Character.HumanoidRootPart.Position).Magnitude) .. "m]"
                    v.Distance.Color = VNM.Esp.Distance.Color
                    v.Distance.OutlineColor = Color3.fromRGB(0, 0, 0)

                    v.Distance.Font = 0
                    v.Distance.Size = VNM.Esp.TextSize

                    v.Distance.Visible = true
                else
                    v.Distance.Visible = false
                end

                if VNM.Esp.Tool.Enabled then
                    if VNM.Esp.Distance.Enabled then
                        v.Tool.Position = Vector2.new(BoxSize.X / 2 + BoxPos.X, BottomOffset + 13)
                    else
                        v.Tool.Position = Vector2.new(BoxSize.X / 2 + BoxPos.X, BottomOffset)
                    end
                    v.Tool.Outline = VNM.Esp.Tool.OutLine
                    if i.Character:FindFirstChildWhichIsA("Tool") then
                        if i.Character:FindFirstChild("GunScript", true) ~= nil or i.Character:FindFirstChild("FlameThrowerScript", true) ~= nil or i.Character:FindFirstChild("RPGScript", true) ~= nil then
                            v.Tool.Text = i.Character:FindFirstChildWhichIsA("Tool").Name
                        else
                            v.Tool.Text = "[" .. i.Character:FindFirstChildWhichIsA("Tool").Name .. "]"
                        end
                    else
                        v.Tool.Text = "[None]"
                    end
                    v.Tool.Color = VNM.Esp.Tool.Color
                    v.Tool.OutlineColor = Color3.fromRGB(0, 0, 0)
                    
                    v.Tool.Font = 0
                    v.Tool.Size = VNM.Esp.TextSize

                    v.Tool.Visible = true
                else
                    v.Tool.Visible = false
                end

                if VNM.Esp.Box.Enabled then
                    v.BoxOutline.Size = BoxSize
                    v.BoxOutline.Position = BoxPos
                    v.BoxOutline.Visible = VNM.Esp.Box.OutLine
                    v.BoxOutline.Color = Color3.fromRGB(0, 0, 0)
                    
                    v.Box.Size = BoxSize
                    v.Box.Position = BoxPos
                    if VNM.Esp.TargetColor.Enabled and SilentTarget ~= nil and i == SilentTarget then
                        v.Box.Color = VNM.Esp.TargetColor.Color
                    elseif VNM.Esp.CrewColor.Enabled and Script.Functions.FindCrew(i) and i.DataFolder.Information:FindFirstChild("Crew").Value == Client.DataFolder.Information:FindFirstChild("Crew").Value then
                        v.Box.Color = VNM.Esp.CrewColor.Color
                    else
                        v.Box.Color = VNM.Esp.Box.Color
                    end
                    v.Box.Visible = true
                else
                    v.BoxOutline.Visible = false
                    v.Box.Visible = false
                end

                if VNM.Esp.HealthBar.Enabled then
                    if VNM.Esp.HealthBar.HealthColor then
                        local Health = i.Character.Humanoid.Health / i.Character.Humanoid.MaxHealth
                        v.HealthBar.Color = Color3.fromHSV(Health * 0.3, 1, 1) 
                    else
                        v.HealthBar.Color = VNM.Esp.HealthBar.Color
                    end

                    v.HealthBar.From = Vector2.new((BoxPos.X - 5), BoxPos.Y + BoxSize.Y)
                    v.HealthBar.To = Vector2.new(v.HealthBar.From.X, v.HealthBar.From.Y - (Hum.Health / Hum.MaxHealth) * BoxSize.Y)
                    v.HealthBar.Visible = true

                    v.HealthBarOutline.From = Vector2.new(v.HealthBar.From.X, BoxPos.Y + BoxSize.Y + 1)
                    v.HealthBarOutline.To = Vector2.new(v.HealthBar.From.X, (v.HealthBar.From.Y - 1 * BoxSize.Y) - 1)
                    v.HealthBarOutline.Color = Color3.fromRGB(0, 0, 0)
                    v.HealthBarOutline.Visible = VNM.Esp.HealthBar.OutLine
                else
                    v.HealthBarOutline.Visible = false
                    v.HealthBar.Visible = false
                end

                if VNM.Esp.HealthText.Enabled then
                    local Offset = 22
                    if VNM.Esp.ArmorBar.Enabled == false then
                        Offset = Offset - 7
                    end
                    if VNM.Esp.HealthBar.Enabled == false then
                        Offset = Offset - 7
                    end

                    if VNM.Esp.HealthText.HealthColor then
                        local Health = i.Character.Humanoid.Health / i.Character.Humanoid.MaxHealth
                        v.HealthText.Color = Color3.fromHSV(Health * 0.3, 1, 1) 
                    else
                        v.HealthText.Color = VNM.Esp.HealthText.Color
                    end
                    
                    v.HealthText.Text = tostring(math.floor((Hum.Health / Hum.MaxHealth) * 100 + 0.5))
                    v.HealthText.Position = Vector2.new((BoxPos.X - Offset), (BoxPos.Y + BoxSize.Y - 1 * BoxSize.Y) - 1)
                    v.HealthText.OutlineColor = Color3.fromRGB(0, 0, 0)
                    v.HealthText.Outline = VNM.Esp.HealthText.OutLine

                    v.HealthText.Font = 0
                    v.HealthText.Size = VNM.Esp.TextSize

                    v.HealthText.Visible = true
                else
                    v.HealthText.Visible = false
                end

                if VNM.Esp.Flags.Enabled then
                    local Offset = 10
                    if VNM.Esp.ArmorBar.Enabled == false then
                        Offset = Offset - 7
                    end
                    if VNM.Esp.HealthBar.Enabled == false then
                        Offset = Offset - 7
                    end
                    if i.Character.HumanoidRootPart.Velocity.Magnitude > 120 and VNM.Esp.Flags.DesyncState then
                        v.Flag.Text = "Desyncing"
                    elseif i.Character.HumanoidRootPart.Velocity.Y > 2 and VNM.Esp.Flags.WalkingState then
                        v.Flag.Text = "Jumping"
                    elseif i.Character.HumanoidRootPart.Velocity.Y < -2 and VNM.Esp.Flags.WalkingState then
                        v.Flag.Text = "Falling"
                    elseif i.Character.HumanoidRootPart.Velocity.Magnitude > 2 and VNM.Esp.Flags.WalkingState then
                        v.Flag.Text = "Walking"
                    elseif i.Character.HumanoidRootPart.Velocity.Magnitude < 1 and VNM.Esp.Flags.WalkingState then
                        v.Flag.Text = "Standing"
                    end
                    v.Flag.Position = Vector2.new((BoxPos.X - Offset) - (string.len(v.Flag.Text) * 3), (BoxPos.Y + BoxSize.Y - 1 * BoxSize.Y) + 22)
                    v.Flag.Color = VNM.Esp.Flags.Color
                    v.Flag.OutlineColor = Color3.fromRGB(0, 0, 0)
                    v.Flag.Outline = VNM.Esp.Flags.OutLine

                    v.Flag.Font = 0
                    v.Flag.Size = VNM.Esp.TextSize

                    v.Flag.Visible = true
                else
                    v.Flag.Visible = false
                end

                if VNM.Esp.Tracer.Enabled then
                    if VNM.Esp.Tracer.Method == ("Screen") then
                        v.Tracer.From = Vector2.new(Camera.ViewportSize.X / 2, Camera.ViewportSize.Y)
                    else
                        v.Tracer.From = Vector2.new(Mouse.X, Mouse.Y + GuiS:GetGuiInset().Y)
                    end
                    v.Tracer.To = Vector2.new(Vector.X, Vector.Y)
                    v.Tracer.Thickness = VNM.Esp.Tracer.Thickness
                    v.Tracer.Color = VNM.Esp.Tracer.Color
                    v.Tracer.Visible = true
                else
                    v.Tracer.Visible = false
                end

                if i.Character:FindFirstChild("BodyEffects") and i.Character:FindFirstChild("BodyEffects"):FindFirstChild("Armor") then
                    if VNM.Esp.ArmorBar.Enabled then
                        if VNM.Esp.HealthBar.Enabled then
                            v.ArmorBar.From = Vector2.new((BoxPos.X - 9), BoxPos.Y + BoxSize.Y)
                        else
                            v.ArmorBar.From = Vector2.new((BoxPos.X - 5), BoxPos.Y + BoxSize.Y)
                        end
                        v.ArmorBar.To = Vector2.new(v.ArmorBar.From.X, v.ArmorBar.From.Y - (i.Character.BodyEffects.Armor.Value / 200) * BoxSize.Y)
                        v.ArmorBar.Color = VNM.Esp.ArmorBar.Color
                        v.ArmorBar.Visible = true

                        v.ArmorBarOutline.From = Vector2.new(v.ArmorBar.From.X, BoxPos.Y + BoxSize.Y + 1)
                        v.ArmorBarOutline.To = Vector2.new(v.ArmorBar.From.X, (v.ArmorBar.From.Y - 1 * BoxSize.Y) - 1)
                        v.ArmorBarOutline.Color = Color3.fromRGB(0, 0, 0)
                        v.ArmorBarOutline.Visible = VNM.Esp.ArmorBar.OutLine
                    else
                        v.ArmorBarOutline.Visible = false
                        v.ArmorBar.Visible = false
                    end
                    if VNM.Esp.ArmorText.Enabled then
                        local Offset = 22
                        if VNM.Esp.ArmorBar.Enabled == false then
                            Offset = Offset - 7
                        end
                        if VNM.Esp.HealthBar.Enabled == false then
                            Offset = Offset - 7
                        end
                        v.ArmorText.Text = tostring(math.floor((i.Character.BodyEffects.Armor.Value / 2) + 0.5))
                        if VNM.Esp.HealthText.Enabled then
                            v.ArmorText.Position = Vector2.new((BoxPos.X - Offset), (BoxPos.Y + BoxSize.Y - 1 * BoxSize.Y) + 11)
                        else
                            v.ArmorText.Position = Vector2.new((BoxPos.X - Offset), (BoxPos.Y + BoxSize.Y - 1 * BoxSize.Y) - 1)
                        end
                        v.ArmorText.Color = VNM.Esp.ArmorText.Color
                        v.ArmorText.OutlineColor = Color3.fromRGB(0, 0, 0)
                        v.ArmorText.Outline = VNM.Esp.ArmorText.OutLine

                        v.ArmorText.Font = 0
                        v.ArmorText.Size = VNM.Esp.TextSize

                        v.ArmorText.Visible = true
                    else
                        v.ArmorText.Visible = false
                    end
                else
                    v.ArmorBarOutline.Visible = false
                    v.ArmorBar.Visible = false
                    v.ArmorText.Visible = false
                end 
            else
                v.Name.Visible = false
                v.BoxOutline.Visible = false
                v.Box.Visible = false
                v.HealthBarOutline.Visible = false
                v.HealthBar.Visible = false
                v.HealthText.Visible = false
                v.ArmorBarOutline.Visible = false
                v.ArmorBar.Visible = false
                v.ArmorText.Visible = false
                v.Distance.Visible = false
                v.Tool.Visible = false
                v.Flag.Visible = false
                v.Tracer.Visible = false
            end
        else
            v.Name.Visible = false
            v.BoxOutline.Visible = false
            v.Box.Visible = false
            v.HealthBarOutline.Visible = false
            v.HealthBar.Visible = false
            v.HealthText.Visible = false
            v.ArmorBarOutline.Visible = false
            v.ArmorBar.Visible = false
            v.ArmorText.Visible = false
            v.Distance.Visible = false
            v.Tool.Visible = false
            v.Flag.Visible = false
            v.Tracer.Visible = false
        end
    end
end

-- // Recreates An Table
Script.Functions.GetTable = function(Table)
    if type(Table) == ("table") then
        local Writer = {}
        Writer.__index = Writer
    
        Writer.New = function()
            local self = setmetatable({}, Writer)
            self.Indent = 0
            self.Text = ""
            return self
        end
    
        Writer.WriteIndentation = function(self)
            for i = 1, self.Indent do
                self.Text = self.Text .. "\t"
            end
        end
    
        Writer.Write = function(self, Text)
            self.Text = self.Text .. Text
        end
    
        Writer.WriteLine = function(self, Text)
            self.Text = self.Text .. Text .. "\n"
        end
    
        Writer.WriteIndent = function(self, Text)
            self:WriteIndentation()
            self:Write(Text)
        end
    
        Writer.IncIndent = function(self)
            self.Indent = self.Indent + 1
        end
    
        Writer.Unindent = function(self)
            self.Indent = self.Indent - 1
        end
    
        Writer.ToString = function(self)
            return self.Text
        end
    
        Writer.Clear = function(self)
            self.Text = ""
            self.Indent = 0
        end
    
        local TableDump = {}
        TableDump.__index = TableDump
    
        TableDump.New = function(Table)
            local self = setmetatable({}, TableDump)
            self.Writer = Writer.New()
            self.Ot = Table
            self.Memory = {}
            self.VisitedTables = {}
            return self
        end
    
        TableDump.CacheGlobalMemory = function(self)
            local CurrentTrack = {"_G"}
            local Functions = {}
    
            Functions.InternalCount = function(Table)
                local Current = 0
                for _, __ in pairs(Table) do
                    Current = Current + 1
                end
                return Current
            end
    
            Functions.CreateNamespace = function()
                local NameSpace = ""
                for Index, Value in pairs(CurrentTrack) do
                    if Value ~= ("_G") and Value ~= ("package") then
                        NameSpace = NameSpace .. Value .. "."
                    end
                end
                return NameSpace
            end
    
            Functions.InternalCache = function(Table)
                local Len = Functions.InternalCount(Table)
                local Current = 0
                for Index, value in pairs(Table) do
                    Current = Current + 1
                    if type(value) == ("function") or type(value) == ("table") then
                        if type(value) == ("table") and self.VisitedTables[value] == nil then
                            self.VisitedTables[value] = Index
                            CurrentTrack[#CurrentTrack + 1] = Index
                            Functions.InternalCache(value)
                        end
                        self.Memory[value] = Functions.CreateNamespace() .. Index
                    end
                    if Current == Len then
                        table.remove(CurrentTrack, #CurrentTrack)
                    end
                end
            end
    
            Functions.InternalCache(_G)
        end
    
        TableDump.Resolve = function(self)
            self:CacheGlobalMemory()
            local Functions = {}

            Functions.InternalResolveSpecial = function(Value)
                if Value == _G then
                    return "_G"
                end
                if self.Memory[Value] then
                    return self.Memory[Value]
                end
                if type(Value) == ("table") then
                    if self.VisitedTables[Value] ~= nil then
                        if self.VisitedTables[Value] == true then
                            return "{...}"
                        end
                        return self.VisitedTables
                    end
                    Functions.InternalResolve(Value)
                    return ""
                elseif type(Value) == ("function") then
                    return '"Function Couldnt Be Added: ' .. tostring(Value) .. '"'
                end
                return tostring(Value)
            end
    
            Functions.InternalResolveValue = function(Value)
                if type(Value) == ("function") or type(Value) == ("table") then
                    return Functions.InternalResolveSpecial(Value)
                elseif type(Value) == ("string") then
                    return '"' .. tostring(Value) .. '"'
                elseif type(Value) == ("number") and Value == math.huge then
                    return "math.huge"
                elseif type(Value) == ("number") and Value == math.pi then
                    return "math.pi"
                elseif typeof(Value) == ("Color3") then
                    return "Color3.new(" .. tostring(Value) .. ")"
                elseif typeof(Value) == ("Vector2") then
                    return "Vector2.new(" .. tostring(Value) .. ")"
                elseif typeof(Value) == ("Vector3") then
                    return "Vector3.new(" .. tostring(Value) .. ")"
                else
                    return tostring(Value)
                end
            end
    
            Functions.InternalCount = function(Table)
                local Current = 0
                for _, __ in pairs(Table) do
                    Current = Current + 1
                end
                return Current
            end
    
            Functions.InternalResolve = function(Table, Index)
                local Len = Functions.InternalCount(Table)
                local Current = 0

                if Len ~= 0 then
                    self.Writer:WriteLine("{")
                else
                    self.Writer:Write("{")
                end

                self.Writer:IncIndent()
                self.VisitedTables[Table] = true

                if Index then
                    self.VisitedTables[Table] = Index
                end

                for Index, value in pairs(Table) do
                    Current = Current + 1
                    if type(Index) == ("string") then
                        if string.find(Index, " ") then
                            self.Writer:WriteIndent('["' .. Index .. '"] = ')
                        else
                            self.Writer:WriteIndent(Index .. " = ")
                        end

                        self.Writer:Write(Functions.InternalResolveValue(value))
                    elseif type(Index) == ("number") then
                        self.Writer:WriteIndent("[" .. tostring(Index) .. "] = ")
                        self.Writer:Write(Functions.InternalResolveValue(value))
                    elseif type(Index) == ("table") then
                        self.Writer:WriteIndent("[")
                        Functions.InternalResolve(Index)
                        self.Writer:Write("] = ")
                        self.Writer:Write(Functions.InternalResolveValue(value))
                    elseif type(Index) == ("function") then
                        self.Writer:WriteIndent(Functions.InternalResolveValue(Index) .. " = ")
                        self.Writer:Write(Functions.InternalResolveValue(value))
                    else
                        self.Writer:WriteIndent(Functions.InternalResolveValue(Index) .. " = ")
                        self.Writer:Write(Functions.InternalResolveValue(value))
                    end
                    if Current == Len then
                        self.Writer:WriteLine("")
                    else
                        self.Writer:WriteLine(",")
                    end
                end

                self.Writer:Unindent()
                if Len ~= 0 then
                    self.Writer:WriteIndent("}")
                else
                    self.Writer:Write("}")
                end
            end
            Functions.InternalResolve(self.Ot)
        end
    
        TableDump.ToString = function(self)
            self:Resolve()
            return self.Writer:ToString()
        end
    
        return "return " .. TableDump.New(Table):ToString()
    else
        return "Error: Table Not Found"
    end
end

-- // Chat Change Check
Client.Chatted:Connect(function(Msg)
    if Msg == VNM.ChatCommands.CrashMode then
        if VNM.ChatCommands.CrashMethod == ("Freeze") then
            while true do end
        elseif VNM.ChatCommands.CrashMethod == ("Shutdown") then
            game:Shutdown()
        end
    elseif Msg == VNM.ChatCommands.RejoinServer then
        game:GetService("TeleportService"):TeleportToPlaceInstance(game.PlaceId, game.JobId, Client)
    elseif Msg == VNM.ChatCommands.RandomServer then
        game:GetService("TeleportService"):Teleport(game.PlaceId, Client) 
    end
    local Splitted = string.split(Msg, " ")
    if Splitted[1] and Splitted[2] and VNM.ChatCommands.Enabled then
        if Splitted[1] == VNM.ChatCommands.LoadConfig then
            if isfolder("VNM/" .. game.PlaceId) and isfolder("VNM/" .. game.PlaceId .. "/Configs") and isfile("VNM/" .. game.PlaceId .. "/Configs/" .. Splitted[2] .. ".lua") then
                local Table = loadfile("VNM/" .. game.PlaceId .. "/Configs/" .. Splitted[2] .. ".lua")()
                VNM = Table
                Script.Functions.CreateNotification("SuccesFully Loaded File (Name: " .. Splitted[2] .. ")", Color3.fromRGB(206, 67, 67))
            else
                Script.Functions.CreateNotification("Error: Couldnt Find File (Name: " .. Splitted[2] .. ")", Color3.fromRGB(206, 67, 67))
            end
        elseif Splitted[1] == VNM.ChatCommands.SaveConfig then
            if not isfolder("VNM") then
                makefolder("VNM")
            end
            if not isfolder("VNM/" .. game.PlaceId) then
                makefolder("VNM/" .. game.PlaceId)
            end
            if not isfolder("VNM/" .. game.PlaceId .. "/Configs") then
                makefolder("VNM/" .. game.PlaceId .. "/Configs")
            end
            if not isfile("VNM/" .. game.PlaceId .. "/Configs/" .. Splitted[2] .. ".lua") then
                writefile("VNM/" .. game.PlaceId .. "/Configs/" .. Splitted[2] .. ".lua", Script.Functions.GetTable(VNM))
                Script.Functions.CreateNotification("SuccesFully Created File (Name: " .. Splitted[2] .. ")", Color3.fromRGB(206, 67, 67))
            else
                Script.Functions.CreateNotification("Error: File Already Exists (Name: " .. Splitted[2] .. ")", Color3.fromRGB(206, 67, 67))
            end
        elseif Splitted[1] == VNM.ChatCommands.Silent_Prediction then
            VNM.Silent.Prediction = Splitted[2]
        elseif Splitted[1] == VNM.ChatCommands.Silent_Fov_Size then
            SilentFovRadius.Value = Splitted[2]
        elseif Splitted[1] == VNM.ChatCommands.Silent_Fov_Show then
            if Splitted[2] == ("true") then
                VNM.Silent.Fov.Visible = true
            else
                VNM.Silent.Fov.Visible = false
            end
        elseif Splitted[1] == VNM.ChatCommands.Silent_Enabled then
            if Splitted[2] == ("true") then
                VNM.Silent.Enabled = true
            else
                VNM.Silent.Enabled = false 
            end
        elseif Splitted[1] == VNM.ChatCommands.Silent_HitChance then
            VNM.Silent.HitChance = Splitted[2]
        elseif Splitted[1] == VNM.ChatCommands.Silent_LegitMode then
            if Splitted[2] == ("true") then
                VNM.Silent.LegitMode = true
            else
                VNM.Silent.LegitMode = false
            end
        elseif Splitted[1] == VNM.ChatCommands.Silent_BlatantMode then
            if Splitted[2] == ("true") then
                VNM.Silent.BlatantMode = true
            else
                VNM.Silent.BlatantMode = false
            end
        elseif Splitted[1] == VNM.ChatCommands.AimAssist_Prediction then
            VNM.AimAssist.Prediction = Splitted[2]
        elseif Splitted[1] == VNM.ChatCommands.AimAssist_Fov_Size then
            VNM.AimAssist.Fov.Radius = Splitted[2]
        elseif Splitted[1] == VNM.ChatCommands.AimAssist_Fov_Show then
            if Splitted[2] == ("true") then
                VNM.AimAssist.Fov.Visible = true
            else
                VNM.AimAssist.Fov.Visible = false
            end
        elseif Splitted[1] == VNM.ChatCommands.AimAssist_Enabled then
            if Splitted[2] == ("true") then
                VNM.AimAssist.Enabled = true
            else
                VNM.AimAssist.Enabled = false
            end
        elseif Splitted[1] == VNM.ChatCommands.AimAssist_SmoothX then
            VNM.AimAssist.Smoothness_X = Splitted[2]
            Script.SavedValue.AimAssistSmoothX = VNM.AimAssist.Smoothness_X
        elseif Splitted[1] == VNM.ChatCommands.AimAssist_SmoothY then
            VNM.AimAssist.Smoothness_Y = Splitted[2]
            Script.SavedValue.AimAssistSmoothY = VNM.AimAssist.Smoothness_Y
        elseif Splitted[1] == VNM.ChatCommands.AimAssist_Shake then
            VNM.AimAssist.Shake.Shake_X = Splitted[2]
            VNM.AimAssist.Shake.Shake_Y = Splitted[2]
            VNM.AimAssist.Shake.Shake_Z = Splitted[2]
            Script.SavedValue.ShakeX = VNM.AimAssist.Shake.Shake_X
            Script.SavedValue.ShakeY = VNM.AimAssist.Shake.Shake_Y
            Script.SavedValue.ShakeZ = VNM.AimAssist.Shake.Shake_Z
        end
    end
end)

-- // KeyDown Mouse Check
Uis.InputBegan:connect(function(input, Gp)
    if not Gp then
        -- // Not ElseIf So You Can Use Multiple Same Keybinds
        if input.KeyCode == Enum.KeyCode[string.upper(VNM.InventorySorter.KeyBind)] and VNM.InventorySorter.Enabled and Script.Functions.Alive(Client) then
            local GunOrder = VNM.InventorySorter.Slots
            local BackPack = Client.Backpack
            local CurrentTime = tick()
            local GunLoop = 10 - #GunOrder
            local TimeSinceLastKeybind = CurrentTime - keybindTime

            if TimeSinceLastKeybind >= 5 then
                keybindTime = CurrentTime
                local GunFolder = Instance.new("Folder")
                GunFolder.Name = "GunFolder"
                GunFolder.Parent = game.Workspace
                local GunFolderID = game.Workspace.GunFolder

                for _, v in pairs(BackPack:GetChildren()) do
                    if v:IsA("Tool") then
                        v.Parent = game.Workspace.GunFolder
                    end
                end

                for _, v in pairs(GunOrder) do
                    local Gun = GunFolderID:FindFirstChild(v)
                    if Gun then
                        Gun.Parent = BackPack
                        wait(0.05)
                    else
                        GunLoop = GunLoop + 1
                    end
                end

                if VNM.InventorySorter.UseFood then
                    for _, v in pairs(GunFolderID:GetChildren()) do
                        if v:FindFirstChild("Drink") or v:FindFirstChild("Eat") then
                            v.Parent = BackPack
                            GunLoop = GunLoop -1
                        end
                    end
                end

                if GunLoop > 0 then
                    for i = 1, GunLoop do
                        local InvisTool = Instance.new("Tool")
                        InvisTool.Name = ""
                        InvisTool.ToolTip = "PlaceHolder"
                        InvisTool.GripPos = Vector3.new(0, 1, 0)
                        InvisTool.RequiresHandle = false
                        InvisTool.Parent = BackPack
                    end
                end

                for _, v in pairs(GunFolderID:GetChildren()) do
                    if v:IsA("Tool") then
                        v.Parent = BackPack
                    end
                end

                for _, v in pairs(BackPack:GetChildren()) do
                    if v.Name == "" then
                        v:Destroy()
                    end
                end

                GunFolder:Destroy()
            end
        end
        
        if VNM.FakeSpike.Enabled and input.KeyCode == Enum.KeyCode[string.upper(VNM.FakeSpike.KeyBind)] then
            if VNM.FakeSpike.ToggleMode then
                FakeSpike = not FakeSpike
                if FakeSpike == true then
                    settings().Network.IncomingReplicationLag = (VNM.FakeSpike.Power * 0.001)
                    if VNM.Options.NotificationMode.Enabled and VNM.Options.NotificationMode.FakeSpike then
                        Script.Functions.CreateNotification("FakeSpike: " .. tostring(FakeSpike), Color3.fromRGB(206, 67, 67))
                    end
                else
                    settings().Network.IncomingReplicationLag = 0
                    if VNM.Options.NotificationMode.Enabled and VNM.Options.NotificationMode.FakeSpike then
                        Script.Functions.CreateNotification("FakeSpike: " .. tostring(FakeSpike), Color3.fromRGB(206, 67, 67))
                    end
                end
            else
                settings().Network.IncomingReplicationLag = (VNM.FakeSpike.Power * 0.001)
                if VNM.Options.NotificationMode.Enabled and VNM.Options.NotificationMode.FakeSpike then
                    Script.Functions.CreateNotification("FakeSpike: " .. tostring(FakeSpike), Color3.fromRGB(206, 67, 67))
                end
                task.wait(VNM.FakeSpike.Delay)
                settings().Network.IncomingReplicationLag = 0
                if VNM.Options.NotificationMode.Enabled and VNM.Options.NotificationMode.FakeSpike then
                    Script.Functions.CreateNotification("FakeSpike: " .. tostring(FakeSpike), Color3.fromRGB(206, 67, 67))
                end
            end
        end

        if VNM.F9Cleaner.Enabled and input.KeyCode == Enum.KeyCode[string.upper(VNM.F9Cleaner.KeyBind)] then
            Script.Functions.ClearConsole()
        end

        if input.KeyCode == Enum.KeyCode[string.upper(VNM.AimAssist.FrameSkip.KeyBind)] and Script.Functions.Alive(AimTarget) and VNM.AimAssist.FrameSkip.Enabled then
            FrameSkip = true
        end

        if input.KeyCode == Enum.KeyCode[string.upper(VNM.Silent.ForceKeyBind)] and VNM.Silent.ForceLock_AimAssistTarget == false and VNM.Silent.ForceLock then
            if ForceLock == nil then
                ForceLock = Script.Functions.GetClosestPlayer(true)
        		if VNM.Options.NotificationMode.Enabled and VNM.Options.NotificationMode.Misc and ForceLock ~= nil then
        		    Script.Functions.CreateNotification("Locked: " .. tostring(ForceLock), Color3.fromRGB(206, 67, 67))
        		end
            else
                if ForceLock ~= nil then
                    ForceLock = nil
            		if VNM.Options.NotificationMode.Enabled and VNM.Options.NotificationMode.Misc then
            		    Script.Functions.CreateNotification("Unlocked", Color3.fromRGB(206, 67, 67))
            		end
        		end
            end
        end

        if input.KeyCode == Enum.KeyCode.I and VNM.Macro.Speed_MacroAbuse and Script.Functions.Alive(Client) then
            if Client.Character:FindFirstChild("GunScript", true) ~= nil or Client.Character:FindFirstChild("FlameThrowerScript", true) ~= nil or Client.Character:FindFirstChild("RPGScript", true) ~= nil then
                local Controller = require(Client:WaitForChild("PlayerScripts"):WaitForChild("PlayerModule")):GetCameras().activeCameraController
                Controller:SetCameraToSubjectDistance(Controller.currentSubjectDistance - 5)
            end
        end

        if input.KeyCode == Enum.KeyCode.O and VNM.Macro.Speed_MacroAbuse and Script.Functions.Alive(Client) then
            if Client.Character:FindFirstChild("GunScript", true) ~= nil or Client.Character:FindFirstChild("FlameThrowerScript", true) ~= nil or Client.Character:FindFirstChild("RPGScript", true) ~= nil then
                local Controller = require(Client:WaitForChild("PlayerScripts"):WaitForChild("PlayerModule")):GetCameras().activeCameraController
                Controller:SetCameraToSubjectDistance(Controller.currentSubjectDistance + 5)
            end
        end

        if input.KeyCode == Enum.KeyCode[string.upper(VNM.AimAssist.KeyBind)] and VNM.AimAssist.Enabled then
            if AimTarget == nil then
                CurrentVelocity2 = Vector3.zero
                AimTarget = Script.Functions.GetClosestPlayer2()
                if VNM.Options.NotificationMode.Enabled and VNM.Options.NotificationMode.AimAssist and AimTarget ~= nil then
                    Script.Functions.CreateNotification("Locked: " .. tostring(AimTarget), Color3.fromRGB(206, 67, 67))
                end
                if AimTarget and AimTarget.Character then
                    PositionData2 = {Target = tostring(AimTarget), Position = AimTarget.Character.HumanoidRootPart.Position, Time = tick()}
                end
            else
                if AimTarget ~= nil then
                    AimTarget = nil
                    CurrentVelocity2 = Vector3.zero
                    if VNM.Options.NotificationMode.Enabled and VNM.Options.NotificationMode.AimAssist then
                        Script.Functions.CreateNotification("Unlocked", Color3.fromRGB(206, 67, 67))
                    end
                end
            end
        end

        if VNM.Silent.TriggerBot and VNM.Silent.TriggerBot_HotKey == false and input.UserInputType == Enum.UserInputType[VNM.Silent.TriggerBotMouseKey] then
            TriggerBot = not TriggerBot
        end

        if VNM.Silent.TriggerBot and VNM.Silent.TriggerBot_HotKey and input.KeyCode == Enum.KeyCode[VNM.Silent.TriggerBotKey] then
            TriggerBot = not TriggerBot
        end

        if input.KeyCode == Enum.KeyCode[string.upper(VNM.Silent.KeyBind)] and VNM.Silent.UseSilentKeyBind then
            VNM.Silent.Enabled = not VNM.Silent.Enabled
            if VNM.Options.NotificationMode.Enabled and VNM.Options.NotificationMode.Silent then
                Script.Functions.CreateNotification("Silent Aim: " .. tostring(VNM.Silent.Enabled), Color3.fromRGB(206, 67, 67))
            end
            CurrentVelocity = Vector3.zero
        end

        if input.KeyCode == Enum.KeyCode[string.upper(VNM.Macro.Lay_KeyBind)] and VNM.Macro.Lay_Emote and game.PlaceId ~= 9825515356 then
            local Args = {
                [1] = "AnimationPack",
                [2] = "Lay"
            }
            game:GetService("ReplicatedStorage").MainEvent:FireServer(unpack(Args))
        end

        if input.KeyCode == Enum.KeyCode[string.upper(VNM.Macro.Greet_Keybind)] and VNM.Macro.Greet_Emote and game.PlaceId ~= 9825515356 then
            local Args = {
                [1] = "AnimationPack",
                [2] = "Greet"
            }
            game:GetService("ReplicatedStorage").MainEvent:FireServer(unpack(Args))
        end

        if input.KeyCode == Enum.KeyCode[string.upper(VNM.Esp.EspKey)] and VNM.Esp.UseEspKeyBind then
    		VNM.Esp.Enabled = not VNM.Esp.Enabled
    		if VNM.Options.NotificationMode.Enabled and VNM.Options.NotificationMode.Esp then
    		    Script.Functions.CreateNotification("Esp: " .. tostring(VNM.Esp.Enabled), Color3.fromRGB(206, 67, 67))
    		end
        end

        if input.KeyCode == Enum.KeyCode[string.upper(VNM.Desync.DesyncKey)] and VNM.Desync.Enabled and VNM.Desync.UseDesyncKey then
    		Desync = not Desync
    		if VNM.Options.NotificationMode.Enabled and VNM.Options.NotificationMode.Esp then
    		    Script.Functions.CreateNotification("Desync: " .. tostring(Desync), Color3.fromRGB(206, 67, 67))
    		end
            if VNM.Desync.Method == ("Slow_Data") then
                wait()
                setfflag("S2PhysicsSenderRate", 15)
            end
        end

        if input.KeyCode == Enum.KeyCode[string.upper(VNM.Macro.Noclip_KeyBind)] and VNM.Macro.Noclip_Macro then
            NoclipMacro = not NoclipMacro
            if not NoclipMacro then return end
            repeat task.wait()
                for _, v in pairs(Client.Backpack:GetChildren()) do
                    if Script.Functions.Alive(Client) then
                        if v.Name == ("[TacticalShotgun]") then
                            v.Parent = Client.Character
                            task.wait(0.1)
                            if v then
                                v.Parent = Client.Backpack
                            end
                        elseif v.Name == ("[Shotgun]") then
                            v.Parent = Client.Character
                            task.wait(0.1)
                            if v then
                                v.Parent = Client.Backpack
                            end
                        end
                    end
                end
            until NoclipMacro == false
        end

        if input.KeyCode == Enum.KeyCode[string.upper(VNM.Macro.Speed_KeyBind)] and VNM.Macro.Speed_Enabled then
            Macro = not Macro
            repeat RS.Heartbeat:Wait()
            if VNM.Macro.Speed_Method == ("FirstPerson") then
                local Controller = require(Client:WaitForChild("PlayerScripts"):WaitForChild("PlayerModule")):GetCameras().activeCameraController
                Controller:SetCameraToSubjectDistance(Controller.currentSubjectDistance - 1)
                for i = 1, math.ceil(VNM.Macro.Speed_Delay) do
                    RS.Heartbeat:Wait()
                end
                Controller:SetCameraToSubjectDistance(Controller.currentSubjectDistance + 1)
            elseif VNM.Macro.Speed_Method == ("Shift") then
                keypress(0xA0)
                for i = 1, math.ceil(VNM.Macro.Speed_Delay) do
                    RS.Heartbeat:Wait()
                end
                keypress(0xA0)
                for i = 1, math.ceil(VNM.Macro.Speed_Delay) do
                    RS.Heartbeat:Wait()
                end
                keyrelease(0xA0)
                for i = 1, math.ceil(VNM.Macro.Speed_Delay) do
                    RS.Heartbeat:Wait()
                end
                keyrelease(0xA0)
            elseif VNM.Macro.Speed_Method == ("ThirdPerson") then
                if VNM.Macro.Speed_ThirdPersonV2 then
                    local Controller = require(Client:WaitForChild("PlayerScripts"):WaitForChild("PlayerModule")):GetCameras().activeCameraController
                    Controller:SetCameraToSubjectDistance(Controller.currentSubjectDistance - 8)
                    for i = 1, math.ceil(VNM.Macro.Speed_Delay) do
                        RS.Heartbeat:Wait()
                    end
                    Controller:SetCameraToSubjectDistance(Controller.currentSubjectDistance + 5)
                else
                    keypress(0x49)
                    for i = 1, math.ceil(VNM.Macro.Speed_Delay) do
                        RS.Heartbeat:Wait()
                    end
                    keypress(0x4F)
                    for i = 1, math.ceil(VNM.Macro.Speed_Delay) do
                        RS.Heartbeat:Wait()
                    end
                    keyrelease(0x49)
                    for i = 1, math.ceil(VNM.Macro.Speed_Delay) do
                        RS.Heartbeat:Wait()
                    end
                    keyrelease(0x4F)
                end
            end
            for i = 1, math.ceil(VNM.Macro.Speed_Delay) do
                RS.Heartbeat:Wait()
            end
            until Macro == false
        end

        if input.KeyCode == Enum.KeyCode[string.upper(VNM.PanicMode.KeyBind)] and VNM.PanicMode.Enabled then
            PanicMode = not PanicMode
            if PanicMode then
                if VNM.Options.NotificationMode.Enabled and VNM.Options.NotificationMode.PanicMode then
                    Script.Functions.CreateNotification("PanicMode: " .. tostring(PanicMode), Color3.fromRGB(206, 67, 67))
                end
                if VNM.Options.NotificationMode == true then
                    Script.PanicModeSaves.CurrentNotificationState = VNM.Options.NotificationMode
                    VNM.Options.NotificationMode = not VNM.Options.NotificationMode
                end
                if VNM.Silent.Enabled == true then
                    Script.PanicModeSaves.CurrentSilentState = VNM.Silent.Enabled
                    VNM.Silent.Enabled = not VNM.Silent.Enabled
                end
                if VNM.AimAssist.Enabled == true then
                    Script.PanicModeSaves.CurrentAimAssistState = VNM.AimAssist.Enabled
                    VNM.AimAssist.Enabled = not VNM.AimAssist.Enabled
                end
                if VNM.AimAssist.Fov.Visible == true then
                    Script.PanicModeSaves.CurrentAimAssistFov = VNM.AimAssist.Fov.Visible
                    VNM.AimAssist.Fov.Visible = not VNM.AimAssist.Fov.Visible
                end
                if VNM.Silent.Fov.Visible == true then
                    Script.PanicModeSaves.CurrentSilentFov = VNM.Silent.Fov.Visible
                    VNM.Silent.Fov.Visible = not VNM.Silent.Fov.Visible
                end
            else
                if Script.PanicModeSaves.CurrentNotificationState then
                    VNM.Options.NotificationMode = not VNM.Options.NotificationMode
                end
                if VNM.Options.NotificationMode.Enabled and VNM.Options.NotificationMode.PanicMode then
                    Script.Functions.CreateNotification("PanicMode: " .. tostring(PanicMode), Color3.fromRGB(206, 67, 67))
                end
                if Script.PanicModeSaves.CurrentSilentState then
                    VNM.Silent.Enabled = Script.PanicModeSaves.CurrentSilentState
                end
                if Script.PanicModeSaves.CurrentAimAssistState then
                    VNM.AimAssist.Enabled = Script.PanicModeSaves.CurrentAimAssistState
                end
                if Script.PanicModeSaves.CurrentAimAssistFov then
                    VNM.AimAssist.Fov.Visible = Script.PanicModeSaves.CurrentAimAssistFov
                end
                if Script.PanicModeSaves.CurrentSilentFov then
                    VNM.Silent.Fov.Visible = Script.PanicModeSaves.CurrentSilentFov
                end
            end
        end

        if input.KeyCode == Enum.KeyCode[string.upper(VNM.Macro.Rotation_KeyBind)] and VNM.Macro.RotationMode then
            if VNM.AimAssist.Enabled then
                VNM.AimAssist.Enabled = false
            end
            for i = 1, math.floor(VNM.Macro.Degrees / VNM.Macro.RotationSpeed) do
                Camera.CoordinateFrame = Camera.CoordinateFrame * CFrame.Angles(0, math.rad(VNM.Macro.RotationSpeed), 0)
                RS.Heartbeat:Wait()
            end
            if VNM.AimAssist.Enabled then
                VNM.AimAssist.Enabled = true
            end
        end
    end
end)

-- // KeyUp Mouse Check
Uis.InputEnded:connect(function(input, Gp)
    if not Gp then
        if VNM.Silent.TriggerBot and TriggerBot and VNM.Silent.TriggerBot_HotKey == false and VNM.Silent.TriggerBot_HoldMode and input.UserInputType == Enum.UserInputType[VNM.Silent.TriggerBotMouseKey] then
            TriggerBot = false
        end

        if VNM.Silent.TriggerBot and TriggerBot and VNM.Silent.TriggerBot_HotKey and VNM.Silent.TriggerBot_HoldMode and input.KeyCode == Enum.KeyCode[string.upper(VNM.Silent.TriggerBotKey)] then
            TriggerBot = false
        end

        if input.KeyCode == Enum.KeyCode[string.upper(VNM.Esp.EspKey)] and VNM.Esp.UseEspKeyBind and VNM.Esp.HoldMode and VNM.Esp.Enabled then
    		VNM.Esp.Enabled = false
    		if VNM.Options.NotificationMode.Enabled and VNM.Options.NotificationMode.Esp then
    		    Script.Functions.CreateNotification("Esp: " .. tostring(VNM.Esp.Enabled), Color3.fromRGB(206, 67, 67))
    		end
        end

        if input.KeyCode == Enum.KeyCode[string.upper(VNM.AimAssist.KeyBind)] and VNM.AimAssist.Enabled and VNM.AimAssist.HoldMode then
    		AimTarget = nil
    		CurrentVelocity2 = Vector3.zero
        end

        if input.KeyCode == Enum.KeyCode[string.upper(VNM.Desync.DesyncKey)] and VNM.Desync.HoldMode and VNM.Desync.UseDesyncKey and VNM.Desync.Enabled then
    		Desync = false
    		if VNM.Options.NotificationMode.Enabled and VNM.Options.NotificationMode.Desync then
    		    Script.Functions.CreateNotification("Desync: " .. tostring(Desync), Color3.fromRGB(206, 67, 67))
    		end
        end

        if input.KeyCode == Enum.KeyCode[string.upper(VNM.Macro.Speed_KeyBind)] and VNM.Macro.Speed_Enabled and VNM.Macro.Speed_HoldMode and Macro then
            Macro = false
        end

        if input.KeyCode == Enum.KeyCode[string.upper(VNM.Macro.Noclip_KeyBind)] and VNM.Macro.Noclip_Macro and VNM.Macro.Noclip_HoldMode and NoclipMacro then
            NoclipMacro = false
        end
    end
end)

-- // Anti Aim Viewer Functions
Script.Functions.ToolActivated = function()
    if VNM.Silent.Enabled and VNM.Silent.AntiAimViewer and Script.Functions.Alive(SilentTarget) and Script.Functions.SilentCheck(SilentTarget) then
        local TargetCF = nil
        local TargetFalling = false

        local GetPart = Script.Functions.GetClosestPartMethod(SilentTarget)
        if GetPart == nil then return end
        VNM.Silent.Part = tostring(GetPart)
        Script.SavedValue.SilentPart = tostring(GetPart)
        
        if VNM.Silent.ClosestPoint then
            TargetCF = Script.Functions.GetClosestPointOnPart(GetPart)
        else
            TargetCF = GetPart.Position
        end

        if VNM.Silent.AntiGroundShots and CurrentVelocity.Y < VNM.Silent.AntiGroundActivation then
            TargetFalling = true
        end
        
        if TargetCF and CurrentVelocity then
            if VNM.Silent.PredictMovement then 
                if VNM.Silent.BlatantMode then
                    local Enabled = true
                    local Mag = (SilentTarget.Character.Humanoid.MoveDirection).Magnitude
                    local SilentVel = SilentTarget.Character.HumanoidRootPart.Velocity
                    if (SilentVel).Magnitude > 110 then
                        Enabled = false
                    elseif SilentVel.Y > 50 then
                        Enabled = false
                    elseif SilentVel.Y < -35 then
                        Enabled = false
                    elseif SilentVel.Y > 75 then
                        Enabled = false
                    elseif (SilentVel).Magnitude < 1 and Mag > 0.01 then
                        Enabled = false
                    elseif (SilentVel).Magnitude > 5 and Mag < 0.01 then
                        Enabled = false
                    end
                    if Enabled and VNM.UniversalCheck.WallCheck_V2 and not Script.Functions.WallCheck(SilentTarget.Character.HumanoidRootPart.Position + (SilentVel * VNM.Silent.Prediction), SilentTarget.Character) then
                        return
                    end
                    if Enabled then
                        if TargetFalling then
                            TargetCF = TargetCF + (Vector3.new(SilentVel.X, (SilentVel.Y * VNM.Silent.AntiGroundValue), SilentVel.Z) * VNM.Silent.Prediction)
                        else
                            TargetCF = TargetCF + (SilentVel * VNM.Silent.Prediction)
                        end
                    else
                        if TargetFalling then
                            TargetCF = TargetCF + (Vector3.new(CurrentVelocity.X, (CurrentVelocity.Y * VNM.Silent.AntiGroundValue), CurrentVelocity.Z) * VNM.Silent.Prediction)
                        else
                            TargetCF = TargetCF + (CurrentVelocity * VNM.Silent.Prediction)
                        end
                    end
                else
                    if TargetFalling then
                        TargetCF = TargetCF + (Vector3.new(CurrentVelocity.X, (CurrentVelocity.Y * VNM.Silent.AntiGroundValue), CurrentVelocity.Z) * VNM.Silent.Prediction)
                    else
                        TargetCF = TargetCF + (CurrentVelocity * VNM.Silent.Prediction)
                    end
                end
            end
            if VNM.Silent.Humanize then
                local HumanizeValue = VNM.Silent.HumanizeValue 
                TargetCF = (TargetCF + Script.Functions.RandomVec3(HumanizeValue, 0.01))
            end
        end
        if TargetCF then
            if VNM.Silent.LegitMode then
                local PartPos = Camera:WorldToScreenPoint(TargetCF)
                local GetScreenPos = Script.Functions.GetFovPosition()
                local Magnitude = ((Vector2.new(PartPos.X, PartPos.Y) - GetScreenPos) - VNM.Silent.Fov.Offset).Magnitude

                if (VNM.Silent.ForceLock == false and Script.Drawing.SilentCircle.Radius + 4 > Magnitude) or VNM.Silent.ForceLock == true then
                    return
                end
            end

            if VNM.Silent.Custom_AntiAimViewerPoint.Enabled == false then
                if game.PlaceId == 13873488228 or game.PlaceId == 13872892064 or game.PlaceId == 13872913451 then
                    local Args = {
                        [1] = "MOUSE",
                        [2] = TargetCF
                    }
                    game:GetService("ReplicatedStorage").MAINEVENT:FireServer(unpack(Args))
                elseif game.PlaceId == 13397024889 then
                    local Args = {
                        [1] = "MOUSE",
                        [2] = TargetCF
                    }
                    game:GetService("ReplicatedStorage").MAINEVENT:FireServer(unpack(Args))
                elseif game.PlaceId == 9825515356 then
                    local Args = {
                        [1] = "GetMousePos",
                        [2] = TargetCF
                    }
                    game:GetService("ReplicatedStorage").MainEvent:FireServer(unpack(Args))
                elseif game.PlaceId == 9183932460 then
                    local Args = {
                        [1] = "UpdateMousePos",
                        [2] = TargetCF
                    }
                    game:GetService("ReplicatedStorage"):FindFirstChild(".gg/untitledhood"):FireServer(unpack(Args))
                elseif game.PlaceId == 5602055394 then
                    local Args = {
                        [1] = "MousePos",
                        [2] = TargetCF,
                        [3] = "P"
                    }
                    game:GetService("ReplicatedStorage").Bullets:FireServer(unpack(Args))
                elseif game.PlaceId == 13051460029 or game.PlaceId == 11833542073 or game.PlaceId == 13051527453 or game.PlaceId == 13395952276 then
                    local Args = {
                        [1] = "UpdateMousePos",
                        [2] = {
                            ["MousePos"] = TargetCF,
                            ["Camera"] = Vector3.new(Camera.CFrame.X, Camera.CFrame.Y, Camera.CFrame.Z)
                        }
                    }
                    game:GetService("ReplicatedStorage").MainEvent:FireServer(unpack(Args))
                elseif game.PlaceId == 13051527453 then
                    local Args = {
                        [1] = "UpdateMousePos",
                        [2] = TargetCF
                    }
                    game:GetService("ReplicatedStorage").MainEvent:FireServer(unpack(Args))
                elseif game.PlaceId == 12618586930 then
                    local Args = {
                        [1] = "UpdateMousePos",
                        [2] = TargetCF
                    }
                    game:GetService("ReplicatedStorage").Remote:FireServer(unpack(Args))
                else
                    local Args = {
                        [1] = "UpdateMousePos",
                        [2] = TargetCF
                    }
                    game:GetService("ReplicatedStorage").MainEvent:FireServer(unpack(Args))
                end
            else
                if type(VNM.Silent.Custom_AntiAimViewerPoint.RemoteEvent) == ("function") then
                    local Args, MainEvent = VNM.Silent.Custom_AntiAimViewerPoint.RemoteEvent(TargetCF)
                    if type(Args) == ("table") and MainEvent:IsA("RemotEvent") then
                        MainEvent:FireServer(unpack(Args))
                    end
                end
            end
        end
    end
end

-- // Connects The AntiAimViewer To Gun
Script.Functions.GetConnections = function(Tool)
    if Tool:IsA("Tool") then
        if ToolConnection then
            ToolConnection:Disconnect()
        end
        ToolConnection = Tool.Activated:Connect(Script.Functions.ToolActivated)
    end
end

-- // Gets Character When New Character Is Added
Script.Functions.WhenCharacterAdded = function(Character)
    Character.ChildAdded:Connect(Script.Functions.GetConnections)
end

-- // Connects The LocalPlayer Character
if Client.Character then
    Script.Functions.WhenCharacterAdded(Client.Character)
end
Client.CharacterAdded:Connect(Script.Functions.WhenCharacterAdded)

-- // Memory Spoofer Functions
Script.Functions.ChangeText = function()
    pcall(function()
        coroutine.resume(coroutine.create(function()
            local PerformanceStats = game:GetService("CoreGui").RobloxGui:FindFirstChild("PerformanceStats")
            if not PerformanceStats then return end
            for _, v in pairs(PerformanceStats:GetDescendants()) do
                if v.ClassName == "TextLabel" then
                    if v.Text:match("MB") then
                        if v.Name == "ValueLabel" then
                            v.Text = Text_1 .. "" .. Text_2
                        end
                        if v.Name == "Label" then
                            if v.Text:match("Current") then
                                v.Text = "Current " .. Text_1 .. "" .. Text_2
                            end
                            if v.Text:match("Average") then
                                v.Text = "Average " .. Text_3.. "" .. Text_4
                            end
                        end
                    end
                end
            end
        end))
        local DevConsole = game:GetService("CoreGui"):FindFirstChild("DevConsoleMaster")
        if not DevConsole then return end
        for _, v in pairs(DevConsole:GetDescendants()) do
            if v.ClassName == "TextButton" then
                if v.Text:match("MB") and not v.Text:match("Value MB") then
                    v.Text = Text_5 .. " MB"
                end
            end
        end
        if DevConsole:FindFirstChild("DevConsoleWindow"):FindFirstChild("DevConsoleUI"):FindFirstChild("MainView"):FindFirstChild("ClientMemory"):FindFirstChild("Entries"):FindFirstChild("Memory"):FindFirstChild("value") then
            DevConsole.DevConsoleWindow.DevConsoleUI.MainView.ClientMemory.Entries.Memory.value.Text =  Text_5 .. "" .. Text_6
        end
        
        local Graph = DevConsole:FindFirstChild("DevConsoleWindow"):FindFirstChild("DevConsoleUI"):FindFirstChild("MainView"):FindFirstChild("ClientMemory"):FindFirstChild("Entries"):FindFirstChild("Memory"):FindFirstChild("Graph"):FindFirstChild("graph")
        local Graph2 = DevConsole:FindFirstChild("DevConsoleWindow"):FindFirstChild("DevConsoleUI"):FindFirstChild("MainView"):FindFirstChild("ClientMemory"):FindFirstChild("Entries"):FindFirstChild("Memory"):FindFirstChild("Graph")
        if Graph then
            if Graph:FindFirstChild("LatestEntryText") then
                Graph:FindFirstChild("LatestEntryText").Text = Text_1 .. Text_7
                Graph:FindFirstChild("AxisTextY0").Text = math.floor(Text_5 * 0.9) .. Text_6
            end
            local Hover_Y = Graph.HoverDetails.HoverHorizontal.Position.Y.Offset
            local TopValue = tonumber(Graph.LatestEntryText.Text)
            local BottomValue = tonumber(Graph.AxisTextY0.Text)
            local TopText_Y = Graph.LatestEntryText.Position.Y.Offset
            local BottomText_Y = Graph.AxisTextY0.Position.Y.Offset
            local LatestEntryLine_Y = Graph.LatestEntryLine.Position.Y.Offset
            local Name = DevConsole.DevConsoleWindow.DevConsoleUI.MainView.ClientMemory.Entries.Memory.Graph.name
            
            if Hover_Y < LatestEntryLine_Y then
                TopText_Y = Graph.LatestEntryText.Position.Y.Offset
                BottomText_Y = Graph.AxisTextY0.Position.Y.Offset
            elseif Graph.AxisTextY0.Position.Y.Offset < LatestEntryLine_Y then
                TopText_Y = Name.Position.Y.Offset
                BottomText_Y = Graph.LatestEntryText.Position.Y.Offset
                TopValue = tonumber(DevConsoleUI.TopBar.LiveStatsModule["MemoryUsage_MB"].Text)
                BottomValue = tonumber(Graph.LatestEntryText.Text)
            end
            
            local HoverValue = BottomValue + ((TopValue - BottomValue) * ((Hover_Y - BottomText_Y) / (TopText_Y - BottomText_Y)))
            Graph.HoverDetails.HoverTextY.Text = string.format("%.3f", HoverValue)
        end
    end)
end

-- // The Loops That Changes The Value
coroutine.resume(coroutine.create(function()
    while VNM.MemorySpoofer.Enabled and RS.Heartbeat:Wait() do
        Script.Functions.ChangeText()
    end
end))
    
coroutine.resume(coroutine.create(function()
    while VNM.MemorySpoofer.Enabled do
        task.wait(VNM.MemorySpoofer.Delay)
        Text_1 = tostring(math.random(VNM.MemorySpoofer.Lowest, VNM.MemorySpoofer.Maximum))
        Text_2 = tostring("." .. math.random(10, 99) .. " MB")
        Text_3 = tostring(math.random(VNM.MemorySpoofer.Lowest, VNM.MemorySpoofer.Maximum))
        Text_4 = tostring("." .. math.random(10, 99) .. " MB")
    end
end))
    
coroutine.resume(coroutine.create(function()
    while VNM.MemorySpoofer.Enabled do
        task.wait(5)
        Text_5 = tostring(math.random(VNM.MemorySpoofer.Lowest, VNM.MemorySpoofer.Maximum))
        Text_6 = tostring("."..math.random(100, 999))
        Text_7 = tostring("."..math.random(100, 999))
    end
end))

local SavedError, SavedError2, SavedError3 = nil, nil, nil
-- // Fires Every Frame Prior To The Frame Being Rendered.
coroutine.resume(coroutine.create(function()
    while true do
        local Succes, Error = pcall(function()
            if Client.Character and Client.Character:FindFirstChild("LowerTorso") and Client.Character.LowerTorso:FindFirstChild("LeftHipRigAttachment") and Client.Character.LowerTorso.LeftHipRigAttachment:FindFirstChild("OriginalPosition") then
                Client.Character.LowerTorso.LeftHipRigAttachment.OriginalPosition:Destroy()
            end
            if VNM.Options.AntiError then 
                coroutine.wrap(pcall)(function()
                    for _, v in ipairs(getconnections(game:GetService('ScriptContext').Error)) do 
                        v:Disable();
                    end
                end)
            end
            Script.Functions.Desync()
            Script.Functions.GetSilentTarget()
            Script.Functions.UpdateFOV()
            if VNM.Options.AutoGetUp and Script.Functions.Alive(Client) and Client.Character.Humanoid:GetState() == Enum.HumanoidStateType.FallingDown then
                Client.Character.Humanoid:ChangeState("GettingUp")
            end
            Script.Functions.MouseChanger()
            if TriggerBot and Script.Functions.Alive(SilentTarget) and SilentTarget.Character:FindFirstChild(VNM.Silent.Part) then
                local Magnitude = Script.Functions.GetMagnitudeFromMouse(SilentTarget.Character.HumanoidRootPart)

                if (VNM.Silent.ForceLock == false and Script.Drawing.SilentCircle.Radius + 4 > Magnitude) or VNM.Silent.ForceLock == true then
                    if VNM.Silent.TriggerBot_Delay == 0 then
                        mouse1click()
                    else
                        task.spawn(function()
                            task.wait(VNM.Silent.TriggerBot_Delay / 1000)
                            mouse1click()
                        end)
                    end
                end
            end
            Script.Functions.SilentMisc()
            Script.Functions.UpdateEsp()
        end)
        if not Succes then
            if SavedError ~= Error then
                SavedError = Error
                Script.Functions.CreateNotification("SomeThing Went Wrong! We Have Sent The Error Code To The Devs!", Color3.fromRGB(206, 67, 67))
                wait(0.5)
                Script.Functions.CreateNotification(Error, Color3.fromRGB(206, 67, 67))
            end
        end
        RS.Heartbeat:Wait()
    end
end))

-- // Fires Every Frame After The Physics Simulation Has Completed.
RS.RenderStepped:Connect(function(DeltaTime)
    local Succes, Error = pcall(function()
        -- // Notification Fade Function And Position Change
    	local Smallest = math.huge
    	for i = 1, #Script.NotifyNote do
    		local v = Script.NotifyNote[i]
    		if v and v.Enabled then
    			Smallest = i < Smallest and i or Smallest
    		else
    			table.remove(Script.NotifyNote, i)
    		end
    	end
    	local Length = #Script.NotifyNote
    	for i = 1, #Script.NotifyNote do
    		local Note = Script.NotifyNote[i]
    		Note:Update(i, Length, DeltaTime)
    		if i <= math.ceil(Length / 10) or Note.Fading then
    			Note:Fade(i, Length, DeltaTime)
    		end
    	end
    end)
    if not Succes then
        if SavedError2 ~= Error then
            SavedError2 = Error
            Script.Functions.CreateNotification("SomeThing Went Wrong! We Have Sent The Error Code To The Devs!", Color3.fromRGB(206, 67, 67))
            wait(0.5)
            Script.Functions.CreateNotification(Error, Color3.fromRGB(206, 67, 67))
        end
    end
end)

-- // The Function For Target Bot, GunSettings
coroutine.resume(coroutine.create(function()
    while true do
        local Succes, Error = pcall(function()
            if VNM.UniversalCheck.Advanced.Target_Bots and game:GetService("Workspace"):FindFirstChild(VNM.UniversalCheck.Advanced.Bot_Path) then
                for _, v in pairs(game:GetService("Workspace")[VNM.UniversalCheck.Advanced.Bot_Path]:GetChildren()) do
                    if not Players:FindFirstChild(v.Name) then
                        local CreatePlayer = Instance.new("Player")
                        CreatePlayer.Name = v.Name
                        CreatePlayer.Character = v
                    else
                        if Players:FindFirstChild(v.Name) then
                            Players[v.Name].Character = v
                        end
                    end
                end
            end
        	if VNM.Silent.GunSettings.Enabled then
            	local CurrentGun = Script.Functions.GetCurrentWeaponName()
                local WeaponSettings = VNM.Silent.GunSettings[CurrentGun]
                if WeaponSettings ~= nil then
                    if VNM.Silent.GunSettings.Methods.Range == false then
                        if VNM.Silent.GunSettings.Methods.Smoothness and Script.SavedValue.AimAssistSmoothX ~=  WeaponSettings.Smoothness then
                            VNM.AimAssist.Smoothness_X = WeaponSettings.Smoothness
                            VNM.AimAssist.Smoothness_Y = WeaponSettings.Smoothness
                            Script.SavedValue.AimAssistSmoothX = WeaponSettings.Smoothness
                            Script.SavedValue.AimAssistSmoothY = WeaponSettings.Smoothness
                        end
                        if VNM.Silent.GunSettings.Methods.AirSmoothness and VNM.AimAssist.AirSmoothness_X ~= WeaponSettings.AirSmoothness then
                            VNM.AimAssist.AirSmoothness_X = WeaponSettings.AirSmoothness
                            VNM.AimAssist.AirSmoothness_Y = WeaponSettings.AirSmoothness
                        end
                        if VNM.Silent.GunSettings.Methods.HitChance and VNM.Silent.Prediction ~= WeaponSettings.HitChance then
                            VNM.Silent.HitChance = WeaponSettings.HitChance
                        end
                        if VNM.Silent.GunSettings.Methods.AirHitChance and VNM.Silent.Prediction ~= WeaponSettings.AirHitChance then
                            VNM.Silent.AirHitChance = WeaponSettings.AirHitChance
                        end
                        if VNM.Silent.GunSettings.Methods.Prediction and VNM.Silent.Prediction ~= WeaponSettings.Prediction then
                            VNM.Silent.Prediction = WeaponSettings.Prediction
                        end
                        if VNM.Silent.GunSettings.Methods.Fov and SilentFovRadius.Value ~= WeaponSettings.Fov then
                            if VNM.Silent.GunSettings.Dynamic.Enabled then
                                local Create = Tween:Create(SilentFovRadius, TweenInfo.new(VNM.Silent.GunSettings.Dynamic.Time, Enum.EasingStyle[VNM.Silent.GunSettings.Dynamic.EasingStyle], Enum.EasingDirection[VNM.Silent.GunSettings.Dynamic.EasingDirection]), {Value = WeaponSettings.Fov})
                                Create:Play()
                                Create.Completed:Wait()
                            else
                                SilentFovRadius.Value = WeaponSettings.Fov
                            end
                        end
                    end
                    if VNM.Silent.Part == nil then return end
                    if VNM.Silent.GunSettings.Methods.Range and Script.Functions.Alive(SilentTarget) and Script.Functions.Alive(Client) and SilentTarget.Character:FindFirstChild(VNM.Silent.Part) then
                        local Magnitude = Script.Functions.GetMagnitudeFromMouse(SilentTarget.Character.HumanoidRootPart)

                        if (VNM.Silent.ForceLock == false and Script.Drawing.SilentCircle.Radius + 4 > Magnitude) or VNM.Silent.ForceLock == true then
                            local Magnitude = (SilentTarget.Character.HumanoidRootPart.Position - Client.Character.HumanoidRootPart.Position).Magnitude
                            if Magnitude < VNM.Silent.GunSettings.Close_Activation then
                                if VNM.Silent.GunSettings.Methods.Smoothness and Script.SavedValue.AimAssistSmoothX ~=  WeaponSettings.CloseSmoothness then
                                    VNM.AimAssist.Smoothness_X = WeaponSettings.CloseSmoothness
                                    VNM.AimAssist.Smoothness_Y = WeaponSettings.CloseSmoothness
                                    Script.SavedValue.AimAssistSmoothX = WeaponSettings.CloseSmoothness
                                    Script.SavedValue.AimAssistSmoothY = WeaponSettings.CloseSmoothness
                                end
                                if VNM.Silent.GunSettings.Methods.AirSmoothness and VNM.AimAssist.AirSmoothness_X ~= WeaponSettings.CloseAirSmoothness then
                                    VNM.AimAssist.AirSmoothness_X = WeaponSettings.CloseAirSmoothness
                                    VNM.AimAssist.AirSmoothness_Y = WeaponSettings.CloseAirSmoothness
                                end
                                if VNM.Silent.GunSettings.Methods.HitChance and VNM.Silent.HitChance ~= WeaponSettings.CloseHitChance then
                                    VNM.Silent.HitChance = WeaponSettings.CloseHitChance
                                end
                                if VNM.Silent.GunSettings.Methods.AirHitChance and VNM.Silent.AirHitChance ~= WeaponSettings.CloseAirHitChance then
                                    VNM.Silent.AirHitChance = WeaponSettings.CloseAirHitChance
                                end
                                if VNM.Silent.GunSettings.Methods.Prediction and VNM.Silent.Prediction ~= WeaponSettings.ClosePrediction then
                                    VNM.Silent.Prediction = WeaponSettings.ClosePrediction
                                end
                                if VNM.Silent.GunSettings.Methods.Fov and SilentFovRadius.Value ~= WeaponSettings.CloseFov then
                                    if VNM.Silent.GunSettings.Dynamic.Enabled then
                                        local Create = Tween:Create(SilentFovRadius, TweenInfo.new(VNM.Silent.GunSettings.Dynamic.Time, Enum.EasingStyle[VNM.Silent.GunSettings.Dynamic.EasingStyle], Enum.EasingDirection[VNM.Silent.GunSettings.Dynamic.EasingDirection]), {Value = WeaponSettings.CloseFov})
                                        Create:Play()
                                        Create.Completed:Wait()
                                    else
                                        SilentFovRadius.Value = WeaponSettings.CloseFov
                                    end
                                end
                            elseif Magnitude < VNM.Silent.GunSettings.Medium_Activation then
                                if VNM.Silent.GunSettings.Methods.Smoothness and Script.SavedValue.AimAssistSmoothX ~= WeaponSettings.MedSmoothness then
                                    VNM.AimAssist.Smoothness_X = WeaponSettings.MedSmoothness
                                    VNM.AimAssist.Smoothness_Y = WeaponSettings.MedSmoothness
                                    Script.SavedValue.AimAssistSmoothX = WeaponSettings.MedSmoothness
                                    Script.SavedValue.AimAssistSmoothY = WeaponSettings.MedSmoothness
                                end
                                if VNM.Silent.GunSettings.Methods.AirSmoothness and VNM.AimAssist.AirSmoothness_X ~= WeaponSettings.MedAirSmoothness then
                                    VNM.AimAssist.AirSmoothness_X = WeaponSettings.MedAirSmoothness
                                    VNM.AimAssist.AirSmoothness_Y = WeaponSettings.MedAirSmoothness
                                end
                                if VNM.Silent.GunSettings.Methods.HitChance and VNM.Silent.HitChance ~= WeaponSettings.MedHitChance then
                                    VNM.Silent.HitChance = WeaponSettings.MedHitChance
                                end
                                if VNM.Silent.GunSettings.Methods.AirHitChance and VNM.Silent.AirHitChance ~= WeaponSettings.MedAirHitChance then
                                    VNM.Silent.AirHitChance = WeaponSettings.MedAirHitChance
                                end
                                if VNM.Silent.GunSettings.Methods.Prediction and VNM.Silent.Prediction ~= WeaponSettings.MedPrediction then
                                    VNM.Silent.Prediction = WeaponSettings.ClosePrediction
                                end
                                if VNM.Silent.GunSettings.Methods.Fov and SilentFovRadius.Value ~= WeaponSettings.MedFov then
                                    if VNM.Silent.GunSettings.Dynamic.Enabled then
                                        local Create = Tween:Create(SilentFovRadius, TweenInfo.new(VNM.Silent.GunSettings.Dynamic.Time, Enum.EasingStyle[VNM.Silent.GunSettings.Dynamic.EasingStyle], Enum.EasingDirection[VNM.Silent.GunSettings.Dynamic.EasingDirection]), {Value = WeaponSettings.MedFov})
                                        Create:Play()
                                        Create.Completed:Wait()
                                    else
                                        SilentFovRadius.Value = WeaponSettings.MedFov
                                    end
                                end
                            elseif Magnitude < VNM.Silent.GunSettings.Far_Activation then
                                if VNM.Silent.GunSettings.Methods.Smoothness and Script.SavedValue.AimAssistSmoothX ~= WeaponSettings.FarSmoothness then
                                    VNM.AimAssist.Smoothness_X = WeaponSettings.FarSmoothness
                                    VNM.AimAssist.Smoothness_Y = WeaponSettings.FarSmoothness
                                    Script.SavedValue.AimAssistSmoothX = WeaponSettings.FarSmoothness
                                    Script.SavedValue.AimAssistSmoothY = WeaponSettings.FarSmoothness
                                end
                                if VNM.Silent.GunSettings.Methods.AirSmoothness and VNM.AimAssist.AirSmoothness_X ~= WeaponSettings.FarAirSmoothness then
                                    VNM.AimAssist.AirSmoothness_X = WeaponSettings.FarAirSmoothness
                                    VNM.AimAssist.AirSmoothness_Y = WeaponSettings.FarAirSmoothness
                                end
                                if VNM.Silent.GunSettings.Methods.HitChance and VNM.Silent.HitChance ~= WeaponSettings.FarHitChance then
                                    VNM.Silent.HitChance = WeaponSettings.FarHitChance
                                end
                                if VNM.Silent.GunSettings.Methods.AirHitChance and VNM.Silent.AirHitChance ~= WeaponSettings.FarAirHitChance then
                                    VNM.Silent.AirHitChance = WeaponSettings.FarAirHitChance
                                end
                                if VNM.Silent.GunSettings.Methods.Prediction and VNM.Silent.Prediction ~= WeaponSettings.FarPrediction then
                                    VNM.Silent.Prediction = WeaponSettings.FarPrediction
                                end
                                if VNM.Silent.GunSettings.Methods.Fov and SilentFovRadius.Value ~= WeaponSettings.FarFov then
                                    if VNM.Silent.GunSettings.Dynamic.Enabled then
                                        local Create = Tween:Create(SilentFovRadius, TweenInfo.new(VNM.Silent.GunSettings.Dynamic.Time, Enum.EasingStyle[VNM.Silent.GunSettings.Dynamic.EasingStyle], Enum.EasingDirection[VNM.Silent.GunSettings.Dynamic.EasingDirection]), {Value = WeaponSettings.FarFov})
                                        Create:Play()
                                        Create.Completed:Wait()
                                    else
                                        SilentFovRadius.Value = WeaponSettings.FarFov
                                    end
                                end
                            end
                        end
                    end
                end
            end
        end)
        if not Succes then
            if SavedError3 ~= Error then
                SavedError3 = Error
                Script.Functions.CreateNotification("SomeThing Went Wrong! We Have Sent The Error Code To The Devs!", Color3.fromRGB(206, 67, 67))
                wait(0.5)
                Script.Functions.CreateNotification(Error, Color3.fromRGB(206, 67, 67))
            end
        end
        RS.Heartbeat:Wait()
    end
end))

-- // Checks Everyone In The Server And Puts It In A Table
for _, Plr in pairs(Players:GetPlayers()) do
    coroutine.resume(coroutine.create(function()
        if Plr ~= Client then
            if Client:IsFriendsWith(Plr.UserId) then
                table.insert(Script.Friends, Plr)
            end
            Script.Functions.CheckIfMod(Plr)
            Script.Functions.NewPlayer(Plr)
        end
    end))
end

-- // Checks When Players Joins And Adds Them To A Table
Players.PlayerAdded:Connect(function(Plr)
    coroutine.resume(coroutine.create(function()
        if Plr ~= Client then
            if Client:IsFriendsWith(Plr.UserId) then
                table.insert(Script.Friends, Plr)
            end
            Script.Functions.CheckIfMod(Plr)
            Script.Functions.NewPlayer(Plr)
        end
    end))
end)

-- // Checks If A Player Left And Removes Them From The Table
Players.PlayerRemoving:Connect(function(Plr)
    if table.find(Script.Friends, Plr) then
        table.remove(Script.Friends, FindPlayer)
    end
    if game.PlaceId == 2788229376 then
        Script.Functions.RemoveTarget(Plr)
    end
    if table.find(Script.EspPlayers, Plr) then
        for _, v in pairs(Script.EspPlayers[Plr]) do
            if v ~= nil then
                v:Remove()
            end
        end
        table.remove(Script.EspPlayers, Plr)
    end
end)

-- // The Functions For The Internal Commands. SHITTY
if VNM.Options.Internal and rconsoleprint and rconsolename then
    local InternalChatCommands = {
        clear = function()
            rconsoleclear()
        end,
        
        set = function(Path, Path2, Value)
            local CurrentPath = VNM[Path]
            if not CurrentPath or not CurrentPath[Path2] then return rconsoleprint("Error: Coudlnt Find Path\n") end
            CurrentPath[Path2] = Value
            rconsoleprint("> " .. tostring(Path2) .. " Is Now Set To: " .. tostring(Value) .. "\n")
        end,

        set2 = function(Path, Path2, Path3, Value)
            local CurrentPath = VNM[Path]
            local CurrentPath2 = CurrentPath[Path2]
            if not CurrentPath or not CurrentPath2 or CurrentPath2[Path3] then return rconsoleprint("Error: Coudlnt Find Path\n") end
            CurrentPath2[Path3] = Value
            rconsoleprint("> " .. tostring(Path3) .. " Is Now Set To: " .. tostring(Value) .. "\n")
        end,
        
        getplayers = function()
            for _, v in pairs(Players:GetChildren()) do
                if v ~= Client then
                    rconsoleprint("> " .. tostring(v).."\n")
                end
            end
        end,
        
        getplayersmagnitude = function()
            for _, v in pairs(Players:GetChildren()) do
                if v ~= Client and Script.Functions.Alive(v) and Script.Functions.Alive(Client) then
                    local GetStuds = (Client.Character.HumanoidRootPart.Position - v.Character.HumanoidRootPart.Position).Magnitude
                    rconsoleprint("> " .. tostring(v) .. " Is " .. GetStuds .. " Studs Away From You\n")
                end
            end
        end,
        
        removeesp = function(Plr)
            if table.find(Script.EspPlayers, Players[tostring(Plr)]) then
                for i, v in pairs(Script.EspPlayers[Players[tostring(Plr)]]) do
                    if v then
                        v:Remove()
                        rconsoleprint("> SuccesFully Removed ".. tostring(i) .." \n")
                    else
                        rconsoleprint("Error: Coudlnt Find Player \n")
                    end
                end
                table.remove(Script.EspPlayers, Players[tostring(Plr)])
            end
        end,
        
        addesp = function(Plr)
            if Players[tostring(Plr)] then
                Script.Functions.NewPlayer(Players[tostring(Plr)])
                rconsoleprint("> SuccesFully Added ".. tostring(Plr) .." \n")
            else
                rconsoleprint("Error: Coudlnt Find Player \n")
            end
        end,
        
        teleport = function(Plr)
            if Script.Functions.Alive(Players[Plr]) and Script.Functions.Alive(Client) then
                Client.Character.HumanoidRootPart.CFrame = Players[Plr].Character.HumanoidRootPart.CFrame
                rconsoleprint("> SuccesFully Teleported ".. tostring(Plr) .." \n")
            else
                rconsoleprint("Error: Coudlnt Find Player \n")
            end
        end,

        exec = function(Text)
            loadstring(Text)
        end,
        
        chat = function(Text)
            if Text == ("false") then 
                if getgenv().GetChat then 
                    getgenv().GetChat:Disconnect() 
                end 
            end
            if Text == ("true") then
                local Event = game:GetService("ReplicatedStorage").DefaultChatSystemChatEvents
                getgenv().GetChat = Event.OnMessageDoneFiltering.OnClientEvent:Connect(function(object)
                    rconsoleprint("> " .. string.format("%s : %s \n>", object.FromSpeaker, object.Message or ""))
                end)
            end
        end,
        
        discord = function(Plr)
            if setclipboard then
                setclipboard("discord.gg/VNM")
            end
            if http_request or request or HttpPost or syn.request then
                local Send = http_request or request or HttpPost or syn.request
                local Table = {
                    ["cmd"] = "INVITE_BROWSER",
                    ["args"] = {["code"] = "VNM"},
                    ["nonce"] = game:GetService("HttpService"):GenerateGUID(true)
                }
                Send({
                    Url = "http://127.0.0.1:6463/rpc?v=1",
                    Method = "POST",
                    Headers = {
                        ["Content-Type"] = "application/json",
                        ["Origin"] = "https://discord.com"
                    },
                    Body = game:GetService("HttpService"):JSONEncode(Table)
                })
                rconsoleprint("> SuccesFully Invited You To The Discord\n")
            end
        end,
        
        help = function()
            rconsoleprint("\n> List of available ChatCommands:\n\n")
            rconsoleprint("> set(<Path> <Path2> <boolean> Or <string>): Sets The Table To The Value!\n")
            rconsoleprint("> set2(<Path> <Path2> <Path3> <boolean> Or <string>): Sets The Table To The Value!\n")
            rconsoleprint("> addesp(<playername>): WhiteLists The Player Esp\n")
            rconsoleprint("> removeesp(<playername>): Remove The Player Esp\n")
            rconsoleprint("> teleport(<playername>): Teleports You To The Player (RISKY)\n")
            rconsoleprint("> getplayers: prints you an list of the players\n")
            rconsoleprint("> getplayersmagnitude: Prints You An List Of The Players And How Long Away From You\n")
            rconsoleprint("> clear: Clears The Console\n")
            rconsoleprint("> discord: Gets Invites You To The Discord\n")
            rconsoleprint("> exec(<string>): Executes The String\n")
            rconsoleprint("> chat(<boolean>): Shows You People Chatting\n\n")
        end,
    }

    -- // The Main Function For Internal
    coroutine.resume(coroutine.create(function()
        while wait(0.1) do
            rconsolename("VNM Internal")
            rconsoleprint("> ")
            local line = rconsoleinput()
            local Command, Args = string.match(line, "(%S+)%s*(.*)")

            if Command == nil then
                return
            end

            if type(InternalChatCommands[Command]) ~= ("function") then
                rconsoleprint("\nUnknown Command: " .. Command .. "\n")
            else
                local Splitted = string.split(line, " ")
                if Splitted[2] ~= nil and Splitted[3] ~= nil and Splitted[4] ~= nil and Splitted[5] ~= nil then
                    if Splitted[5] == ("false") then 
                        Splitted[5] = false
                    elseif Splitted[5] == ("true") then 
                        Splitted[5] = true
                    end

                    local Succes, Error = pcall(InternalChatCommands[Command], Splitted[2], Splitted[3], Splitted[4], Splitted[5])
                    if not Succes then
                        rconsoleprint("\nError: " .. Error .. "\n")
                    end
                elseif Splitted[2] ~= nil and Splitted[3] ~= nil and Splitted[4] ~= nil then
                    if Splitted[4] == ("false") then 
                        Splitted[4] = false
                    elseif Splitted[4] == ("true") then 
                        Splitted[4] = true
                    end

                    local Succes, Error = pcall(InternalChatCommands[Command], Splitted[2], Splitted[3], Splitted[4])
                    if not Succes then
                        rconsoleprint("\nError: " .. Error .. "\n")
                    end
                else
                    if Command ~= ("set") or Command ~= ("set2") then
                        local Succes, Error = pcall(InternalChatCommands[Command], Args)
                        if not Succes then
                            rconsoleprint("\nError: " .. Error .. "\n")
                        end

                        rconsoleprint("\nError: Command Error\n")
                    end
                end
            end
        end
    end))
end

-- // Sends Information For Basic Stuff
if VNM.Options.GetInformation then
    if GetTime then
        Script.Functions.CreateNotification("Loaded In: " .. string.format("%.".."4".."f", os.clock() - GetTime) .. " Seconds", Color3.fromRGB(206, 67, 67))
    end
end

local InventoryChanger = { Functions = {}, Selected = {}, Skins = {}, Owned = {} };

do
    local Utilities = {};

    function cout(watermark, message)
        if not LPH_OBFUSCATED then
            return print('['..watermark..']', message);
        end;
    end;

    cout('Inventory Changer', 'Executor initialization test message')

    if not getgenv().InventoryConnections then
        getgenv().InventoryConnections = {};
    end;

    local players = game:GetService('Players');
    local client = players.LocalPlayer;

    local tween_service = game:GetService('TweenService');

    Utilities.AddConnection = function(signal, func)
        local connect = signal:Connect(func);

        table.insert(getgenv().InventoryConnections, { signal = signal, func = func, connect = connect });
        return connect;
    end;

    Utilities.Unload = function()
        for _, tbl in ipairs(getgenv().InventoryConnections) do
            if type(tbl) ~= 'table' then 
                tbl:Disconnect();
            end
        end;

        getgenv().InventoryConnections = {};
    end;

    Utilities.Unload();

    Utilities.Tween = function(args)
        local obj = args.obj or args.object;
        local prop = args.prop or args.properties;
        local duration = args.duration or args.time;
        local info = args.info or args.tween_info;
        local callback = args.callback;

        local tween = tween_service:Create(obj, duration and TweenInfo.new(duration, Enum.EasingStyle.Quad, Enum.EasingDirection.Out) or info and TweenInfo.new(unpack(info)), prop);
        tween:Play();

        if callback then
            tween.Completed:Connect(callback);
        end;
    end;

    repeat task.wait() until client.Character:FindFirstChild('FULLY_LOADED_CHAR');

    local player_gui = client.PlayerGui;

    local main_gui = player_gui:WaitForChild('MainScreenGui');
    local crew = main_gui:WaitForChild('Crew');
    local bottom_left = crew:WaitForChild('BottomLeft').Frame;
    local skins_button = bottom_left:WaitForChild('Skins');

    local replicated_storage = game:GetService('ReplicatedStorage');
    local skin_modules = replicated_storage:WaitForChild('SkinModules');
    local meshes = skin_modules:WaitForChild('Meshes');

    local weapon_skins_gui = main_gui:WaitForChild('WeaponSkinsGUI');
    local gui_body_wrapper = weapon_skins_gui:WaitForChild('Body');
    local body_wrapper = gui_body_wrapper:WaitForChild('Wrapper');
    local skin_view = body_wrapper:WaitForChild('SkinView');
    local skin_view_frame = skin_view:WaitForChild('Frame');

    local guns = skin_view_frame:WaitForChild('Guns').Contents;
    local entries = skin_view_frame:WaitForChild('Skins').Contents.Entries;

    local Ignored = workspace.Ignored;
    local Siren = Ignored.Siren;
    local Radius = Siren.Radius;

    local regex = '%[(.-)%]';

    local newColorSequence = ColorSequence.new;
    local Color3fromRGB = Color3.fromRGB;
    local newCFrame = CFrame.new;
    local newColorSequenceKeypoint = ColorSequenceKeypoint.new;

    InventoryChanger.Skins = {
        ['Mystical'] = {
            tween_duration = 0.65,
            beam_width = 0.125,
            color = newColorSequence(Color3fromRGB(255, 39, 24)),
            guns = {
                ['[Revolver]'] = {
                    location = meshes.Mystical.Revolver,
                    equipped = false,
                    shoot_sound = 'rbxassetid://14489866118',
                    C0 = newCFrame(-0.015838623, -0.0802496076, 0.00772094727, 1, 0, 4.37113883e-08, 0, 1, 0, -4.37113883e-08, 0, 1)
                }
            }
        },
        ['CyanPack'] = {
            mesh_location = meshes.CyanPack,
            guns = {
                ['[TacticalShotgun]'] = {
                    location = meshes.CyanPack.Cloud,
                    equipped = false,
                    shoot_sound = 'rbxassetid://14056055126',
                    C0 = newCFrame(0.0441589355, -0.0269355774, -0.000701904297, 1, 0, 0, 0, 1, 0, 0, 0, 1)
                },
                ['[Double-Barrel SG]'] = {
                    location = meshes.CyanPack.DB,
                    equipped = false,
                    shoot_sound = 'rbxassetid://14056053588',
                    C0 = newCFrame(-0.00828552246, 0.417651355, -0.00537109375, 4.18358377e-06, -1.62920685e-07, 1, 3.4104116e-13, 1, 1.62920685e-07, -1, 3.41041052e-13, -4.18358377e-06)
                },
                ['[Revolver]'] = {
                    location = meshes.CyanPack.Devil,
                    equipped = false,
                    shoot_sound = 'rbxassetid://14056056444',
                    C0 = newCFrame(0.0185699463, 0.293397784, -0.00256347656, 1, 0, 0, 0, 1, 0, 0, 0, 1)
                }
            }
        },
        ['Cartoon'] = {
            guns = {
                ['[Flamethrower]'] = {
                    location = meshes.Cartoon.CartoonFT,
                    equipped = false,
                    C0 = newCFrame(-0.272186279, 0.197086751, 0.0440063477, -1, 4.8018768e-07, 8.7078952e-08, 4.80187623e-07, 1, -3.54779985e-07, -8.70791226e-08, -3.54779957e-07, -1)
                },
                ['[Revolver]'] = {
                    location = meshes.Cartoon.CartoonRev,
                    equipped = false,
                    shoot_sound = 'rbxassetid://14221101923',
                    C0 = newCFrame(-0.015411377, 0.0135096312, 0.00338745117, 1.00000095, 3.41326549e-13, 2.84217399e-14, 3.41326549e-13, 1.00000191, -9.89490712e-10, 2.84217399e-14, -9.89490712e-10, 1.00000191)
                },
                ['[Double-Barrel SG]'] = {
                    location = meshes.Cartoon.DBCartoon,
                    equipped = false,
                    shoot_sound = 'rbxassetid://14220912852',
                    C0 = newCFrame(0.00927734375, -0.00691050291, 0.000732421875, -1, -2.79396772e-08, -9.31322797e-10, -2.79396772e-08, 1, 1.42607872e-08, 9.31322575e-10, 1.42607872e-08, -1)
                },
                ['[RPG]'] = {
                    location = meshes.Cartoon.RPGCartoon,
                    equipped = false,
                    C0 = newCFrame(-0.0201721191, 0.289476752, -0.0727844238, 4.37113883e-08, 6.58276836e-37, 1, -5.72632016e-14, 1, 2.50305399e-21, -1, 5.72632016e-14, 4.37113883e-08)
                },
            }
        },
        ['Dragon'] = {
            color = newColorSequence(Color3.new(1, 0, 0)),
            guns = {
                ['[Revolver]'] = {
                    location = meshes.Dragon.DragonRev,
                    equipped = false,
                    C0 = newCFrame(0.0384216309, 0.0450432301, -0.000671386719, 1.87045402e-31, 4.21188801e-16, -0.99999994, 1.77635684e-15, 1, -4.21188827e-16, 1, 1.77635684e-15, -1.87045413e-31)
                },
                ['[Double-Barrel SG]'] = {
                    location = meshes.Dragon.DBDragon,
                    equipped = false,
                    C0 = newCFrame(-0.123794556, 0.0481165648, 0.00048828125, 7.14693442e-07, 3.13283705e-10, 1, -4.56658222e-09, 1, -3.13281678e-10, -1, -4.56658533e-09, 7.14693442e-07)
                }
            }
        },
        ['Tact'] = {
            tween_duration = 1.25,
            beam_width = 0.125,
            color = newColorSequence(Color3.new(1, 0.3725490196, 0.3725490196)),
            guns = {
                ['[Revolver]'] = {
                    location = meshes.Tact.Rev,
                    equipped = false,
                    shoot_sound = 'rbxassetid://13850086195',
                    C0 = newCFrame(-0.318634033, -0.055095911, 0.00491333008, 0, 0, 1, 0, 1, 0, -1, 0, 0)
                },
                ['[Double-Barrel SG]'] = {
                    location = meshes.Tact.DB,
                    equipped = false,
                    C0 = newCFrame(-0.0701141357, -0.0506889224, -0.0826416016, 0, 0, 1, 0, 1, 0, -1, 0, 0)
                },
                ['[TacticalShotgun]'] = {
                    location = meshes.Tact.Tact,
                    equipped = false,
                    C0 = newCFrame(-0.0687713623, -0.0684046745, 0.12701416, 0, 0, 1, 0, 1, 0, -1, 0, 0)
                },
                ['[SMG]'] = {
                    location = meshes.Tact.Uzi,
                    equipped = false,
                    C0 = newCFrame(0.0408782959, 0.0827783346, -0.0423583984, -1, 0, 0, 0, 1, 0, 0, 0, -1)
                },
                ['[Shotgun]'] = {
                    location = meshes.Tact.Shotgun,
                    equipped = false,
                    C0 = newCFrame(-0.0610046387, 0.171100497, -0.00495910645, 1, 0, 0, 0, 1, 0, 0, 0, 1)
                },
                ['[Silencer]'] = {
                    location = meshes.Tact.Silencer,
                    equipped = false,
                    C0 = newCFrame(0.0766601562, -0.0350288749, -0.648864746, 1, 0, -4.37113883e-08, 0, 1, 0, 4.37113883e-08, 0, 1)
                }
            }
        },
        ['Shadow'] = {
            color = newColorSequence(Color3.new(0.560784, 0.470588, 1), Color3.new(0.576471, 0.380392, 1)),
            guns = {
                ['[Revolver]'] = {
                    location = meshes.Shadow.RevolverGhost,
                    equipped = false,
                    C0 = newCFrame(1.52587891e-05, 0, 0, 1, 0, 8.74227766e-08, 0, 1, 0, -8.74227766e-08, 0, 1)
                },
                ['[Double-Barrel SG]'] = {
                    location = meshes.Shadow.DoubleBGhost,
                    equipped = false,
                    C0 = newCFrame(0.0250015259, -0.077037394, 0, 1, 0, 0, 0, 0.999998331, 0, 0, 0, 1)
                },
                ['[AK47]'] = {
                    location = meshes.Shadow.AK47Ghost,
                    equipped = false,
                    C0 = newCFrame(-0.750015259, 4.76837158e-07, -3.05175781e-05, 1, 0, 0, 0, 1, 0, 0, 0, 1)
                },
                ['[SilencerAR]'] = {
                    location = meshes.Shadow.ARGhost,
                    equipped = false,
                    C0 = newCFrame(0.116256714, 0.0750004649, 6.10351562e-05, 1, 0, 0, 0, 1, 0, 0, 0, 1)
                },
                ['[AUG]'] = {
                    location = meshes.Shadow.AUGGhost,
                    equipped = false,
                    C0 = newCFrame(-7.62939453e-06, 0.0499991775, 0, 1, 0, 0, 0, 1, 0, 0, 0, 1)
                },
                ['[DrumGun]'] = {
                    location = meshes.Shadow.DrumgunGhost,
                    equipped = false,
                    C0 = newCFrame(1.14440918e-05, 0, 0, 1, 0, 8.74227766e-08, 0, 1, 0, -8.74227766e-08, 0, 1)
                },
                ['[Flamethrower]'] = {
                    location = meshes.Shadow.FlamethrowerGhost,
                    equipped = false,
                    C0 = newCFrame(-0.219947815, 0.339559376, 0.000274658203, 1, 0, 0, 0, 1, 0, 0, 0, 1)
                },
                ['[Glock]'] = {
                    location = meshes.Shadow.GlockGhost,
                    equipped = false,
                    C0 = newCFrame(0, 0, -0.200004578, 1, 0, 4.37113883e-08, 0, 1, 0, -4.37113883e-08, 0, 1)
                },
                ['[LMG]'] = {
                    location = meshes.Shadow.LMGGhost,
                    equipped = false,
                    C0 = newCFrame(0.374502182, -0.25, -0.25, -1, 0, -1.31134158e-07, 0, 1, 0, 1.31134158e-07, 0, -1)
                },
                ['[P90]'] = {
                    location = meshes.Shadow.P90Ghost,
                    equipped = false,
                    C0 = newCFrame(6.86645508e-05, 0.000218153, 3.05175781e-05, 1, 0, 0, 0, 1, 0, 0, 0, 1)
                },
                ['[RPG]'] = {
                    location = meshes.Shadow.RPGGhost,
                    equipped = false,
                    C0 = newCFrame(0.000122070312, 0.0625389814, 0.00672149658, 1, 0, -8.74227766e-08, 5.00610797e-21, 1, 5.72632016e-14, 8.74227766e-08, 5.72632016e-14, 1)
                },
                ['[Rifle]'] = {
                    location = meshes.Shadow.RifleGhost,
                    equipped = false,
                    C0 = newCFrame(0.000244140625, -0.100267321, -9.15527344e-05, 1, 0, 0, 0, 1, 0, 0, 0, 1)
                },
                ['[SMG]'] = {
                    location = meshes.Shadow.SMGGhost,
                    equipped = false,
                    C0 = newCFrame(-1.14440918e-05, 1.78813934e-07, -0.0263671875, 1, 0, 0, 0, 1, 0, 0, 0, 1)
                },
                ['[Shotgun]'] = {
                    location = meshes.Shadow.ShotgunGhost,
                    equipped = false,
                    C0 = newCFrame(3.05175781e-05, 0.199999928, 3.81469727e-06, -1, 0, -4.37113883e-08, 0, 1, 0, 4.37113883e-08, 0, -1)
                },
                ['[TacticalShotgun]'] = {
                    location = meshes.Shadow.TacticalShotgunGhost,
                    equipped = false,
                    C0 = newCFrame(-0.148262024, 0, 0, 1, 0, 8.74227766e-08, 0, 1, 0, -8.74227766e-08, 0, 1)
                }
            }
        },
        ['Golden Age'] = {
            tween_duration = 1.25,
            beam_width = 0.125,
            color = newColorSequence(Color3.fromHSV(0.89166666666, 0.24, 1)),
            guns = {
                ['[Revolver]'] = {
                    location = meshes.GoldenAge.Revolver,
                    equipped = false,
                    C0 = newCFrame(0.0295257568, 0.0725820661, -0.000946044922, 1, -4.89858741e-16, -7.98081238e-23, 4.89858741e-16, 1, 3.2584137e-07, -7.98081238e-23, -3.2584137e-07, 1),
                    shoot_sound = 'rbxassetid://1898322396'
                },
                ['[Double-Barrel SG]'] = {
                    location = meshes.GoldenAge['Double Barrel'],
                    equipped = false,
                    shoot_sound = 'rbxassetid://4915503055',
                    C0 = newCFrame(-0.00664520264, 0.0538104773, 0.0124816895, -1, 4.89858741e-16, 7.98081238e-23, 4.89858741e-16, 1, 3.2584137e-07, 7.98081238e-23, 3.2584137e-07, -1)
                }
            }
        },
        ['Red Skull'] = {
            color = newColorSequence(Color3.new(0.917647, 0, 0)),
            guns = {
                ['[Revolver]'] = {
                    location = meshes.RedSkull.RedSkullRev,
                    equipped = false,
                    C0 = newCFrame(-0.0043258667, 0.0084195137, -0.00238037109, 0, 0, -1, 0, 1, 0, 1, 0, 0)
                },
                ['[Shotgun]'] = {
                    location = meshes.RedSkull.RedSkullRev,
                    equipped = false,
                    C0 = newCFrame(-0.00326538086, 0.0239292979, -0.039352417, -4.37113883e-08, 0, -1, 0, 1, 0, 1, 0, -4.37113883e-08)
                },
                ['[Double-Barrel SG]'] = {
                    location = meshes.RedSkull.RedSkullRev,
                    equipped = false,
                    C0 = newCFrame(-0.0143432617, -0.151709318, 0.00820922852, -1, 0, 0, 0, 1, 0, 0, 0, -1)
                },
                ['[RPG]'] = {
                    location = meshes.RedSkull.RedSkullRev,
                    equipped = false,
                    C0 = newCFrame(-0.00149536133, 0.254377961, 0.804840088, -1, 0, 4.37113883e-08, -2.50305399e-21, 1, -5.72632016e-14, -4.37113883e-08, 5.72632016e-14, -1)
                }
            }
        },
        --[[['Galaxy'] = {
            border_color = newColorSequence(Color3.new(0, 0, 1)),
            particle = {
                properties = {
                    Color = ColorSequence.new({
                        ColorSequenceKeypoint.new(0, Color3.new(0.419608, 0.376471, 1)),
                        ColorSequenceKeypoint.new(1, Color3.new(0.419608, 0.376471, 1))
                    }),
                    Name = 'Galaxy',
                    Size = NumberSequence.new({
                        NumberSequenceKeypoint.new(0, 0.5),
                        NumberSequenceKeypoint.new(0.496, 1.2),
                        NumberSequenceKeypoint.new(1, 0.5)
                    }),
                    Squash = NumberSequence.new({
                        NumberSequenceKeypoint.new(0, 0),
                        NumberSequenceKeypoint.new(0.173364, 0.525),
                        NumberSequenceKeypoint.new(0.584386, -1.7625),
                        NumberSequenceKeypoint.new(0.98163, 0.0749998),
                        NumberSequenceKeypoint.new(1, 0)
                    }),
                    Transparency = NumberSequence.new({
                        NumberSequenceKeypoint.new(0, 0),
                        NumberSequenceKeypoint.new(0.107922, 1),
                        NumberSequenceKeypoint.new(0.391504, 0.25),
                        NumberSequenceKeypoint.new(0.670494, 0.78125),
                        NumberSequenceKeypoint.new(0.845006, 0),
                        NumberSequenceKeypoint.new(1, 1)
                    }),
                    Texture = 'rbxassetid://7422600824',
                    ZOffset = 1,
                    LightEmission = 0.7,
                    Lifetime = NumberRange.new(1, 1),
                    Rate = 3,
                    Rotation = NumberRange.new(0, 360),
                    RotSpeed = NumberRange.new(0, 15),
                    Speed = NumberRange.new(1, 1),
                    SpreadAngle = Vector2.new(-45, 45)
                }
            },
            guns = {
                ['[Revolver]'] = {
                    texture = 'rbxassetid://9370936730'
                },
                ['[TacticalShotgun]'] = {
                    texture = 'rbxassetid://9402279010'
                }
            }
        },]]
        ['Kitty'] = {
            tween_duration = 1,
            beam_width = 0.125,
            color = newColorSequence(Color3.new(1, 0.690196, 0.882353), Color3.new(1, 0.929412, 0.964706)),
            guns = {
                ['[Revolver]'] = {
                    location = meshes.Kitty.KittyRevolver,
                    equipped = false,
                    shoot_sound = 'rbxassetid://13483022860',
                    C0 = newCFrame(0.0310440063, 0.0737591386, 0.0226745605, 1, 0, 0, 0, 1, 0, 0, 0, 1)
                },
                ['[Flamethrower]'] = {
                    location = meshes.Kitty.KittyFT,
                    equipped = false,
                    C0 = newCFrame(-0.265670776, 0.115545571, 0.00997924805, -1, 9.74078034e-21, 5.47124086e-13, 9.74092898e-21, 1, 3.12638804e-13, -5.47126309e-13, 3.12638804e-13, -1)
                },
                ['[RPG]'] = {
                    location = meshes.Kitty.KittyRPG,
                    equipped = false,
                    C0 = newCFrame(0.0268554688, 0.0252066851, 0.117408752, -1, 2.51111284e-40, 4.37113883e-08, -3.7545812e-20, 1, -8.58948004e-13, -4.37113883e-08, 8.58948004e-13, -1)
                },
                ['[Shotgun]'] = {
                    location = meshes.Kitty.KittyShotgun,
                    equipped = false,
                    shoot_sound = 'rbxassetid://13483035672',
                    C0 = newCFrame(0.0233459473, 0.223892093, -0.0213623047, 4.37118963e-08, -6.53699317e-13, 1, 3.47284736e-20, 1, 7.38964445e-13, -0.999997139, 8.69506734e-21, 4.37119354e-08)
                }
            }
        },
        ['Toy'] = {
            mesh_location = meshes.Toy,
            tween_duration = 1.25,
            color = newColorSequence({newColorSequenceKeypoint(0, Color3.new(0, 1, 0)), ColorSequenceKeypoint.new(0.5, Color3.new(0.666667, 0.333333, 1)), ColorSequenceKeypoint.new(1, Color3.new(1, 0.666667, 0))}),
            guns = {
                ['[Revolver]'] = {
                    location = meshes.Toy.RevolverTOY,
                    equipped = false,
                    C0 = newCFrame(-0.0250854492, -0.144362092, -0.00266647339, 1, 0, 0, 0, 1, 0, 0, 0, 1)
                },
                ['[LMG]'] = {
                    location = meshes.Toy.LMGTOY,
                    equipped = false,
                    C0 = newCFrame(-0.285247803, -0.0942560434, -0.270412445, 1, 0, 4.37113883e-08, 0, 1, 0, -4.37113883e-08, 0, 1)
                },
                ['[Double-Barrel SG]'] = {
                    location = meshes.Toy.DBToy,
                    equipped = false,
                    C0 = newCFrame(-0.0484313965, -0.00164616108, -0.0190467834, -1, 0, 0, 0, 1, 0, 0, 0, -1)
                },
                ['[RPG]'] = {
                    location = meshes.Toy.RPGToy,
                    equipped = false,
                    C0 = newCFrame(0.00121307373, 0.261434197, -0.318969727, 1, 2.5768439e-12, -4.37113883e-08, 2.57684412e-12, 1, 6.29895225e-12, 4.37113883e-08, 6.29895225e-12, 1)
                }
            }
        },
        ['Galactic'] = {
            mesh_location = meshes.Galactic,
            tween_duration = 1.25,
            color = newColorSequence(Color3.new(1, 0, 0)),
            guns = {
                ['[Revolver]'] = {
                    location = meshes.Galactic.galacticRev,
                    equipped = false,
                    C0 = newCFrame(-0.049041748, 0.0399398208, -0.00772094727, 0, 0, 1, 0, 1, 0, -1, 0, 0)
                },
                ['[TacticalShotgun]'] = {
                    location = meshes.Galactic.TacticalGalactic,
                    equipped = false,
                    C0 = newCFrame(-0.0411682129, -0.0281000137, 0.00103759766, 0, 5.68434189e-14, 1, -1.91456822e-13, 1, 5.68434189e-14, -1, 1.91456822e-13, 0)
                }
            }
        },
        ['Water'] = {
            tween_duration = 1.25,
            tween_type = 'Both',
            beam_width = 0.125,
            color = newColorSequence(Color3.new(0, 1, 1), Color3.new(0.666667, 1, 1)),
            guns = {
                ['[Revolver]'] = {
                    location = meshes.Water.WaterGunRevolver,
                    equipped = false,
                    C0 = newCFrame(-0.0440063477, 0.028675437, -0.00469970703, 0, 0, -1, 0, 1, 0, 1, 0, 0)
                },
                ['[TacticalShotgun]'] = {
                    location = meshes.Water.TactWater,
                    equipped = false,
                    shoot_sound = 'rbxassetid://13814991449',
                    C0 = newCFrame(0.0238037109, -0.00912904739, 0.00485229492, 0, 0, 1, 0, 1, 0, -1, 0, 0)
                },
                ['[Double-Barrel SG]'] = {
                    location = meshes.Water.DBWater,
                    equipped = false,
                    shoot_sound = 'rbxassetid://13814990235',
                    C0 = newCFrame(-0.0710754395, 0.00169920921, -0.0888671875, 0, 0, 1, 0, 1, 0, -1, 0, 0)
                },
                ['[Flamethrower]'] = {
                    location = meshes.Water.FTWater,
                    equipped = false,
                    C0 = newCFrame(0.0941314697, 0.593509138, 0.0191040039, -1, 0, 0, 0, 1, 0, 0, 0, -1)
                }
            }
        },
        ['GPO'] = {
            color = newColorSequence(Color3.new(1, 0.666667, 0)),
            guns = {
                ['[RPG]'] = {
                    location = meshes.GPO.Bazooka,
                    equipped = false,
                    C0 = newCFrame(-0.0184631348, 0.0707798004, 0.219360352, 4.37113883e-08, 1.07062025e-23, 1, -5.75081297e-14, 1, 1.14251725e-36, -1, 5.70182736e-14, 4.37113883e-08)
                },
                ['[TacticalShotgun]'] = {
                    location = meshes.GPO.MaguTact,
                    equipped = false,
                    shoot_sound = 'rbxassetid://13998711419',
                    C0 = newCFrame(-0.282501221, 0.0472121239, -0.0065612793, -6.60624482e-06, 1.5649757e-08, -1, -5.68434189e-14, 1, -1.56486806e-08, 1, 5.68434189e-14, -6.60624482e-06)
                },
                ['[Rifle]'] = {
                    location = meshes.GPO.Rifle,
                    equipped = false,
                    C0 = newCFrame(-0.208007812, 0.185256913, 0.000610351562, -3.37081539e-14, 1.62803403e-07, -1.00000012, -8.74227695e-08, 0.999999881, 1.63036205e-07, 1, 8.74227766e-08, -1.94552524e-14)
                }
            }
        },
        ['BIT8'] = {
            tween_duration = 1.25,
            tween_type = 'Width',
            beam_width = 0.125,
            color = newColorSequence(Color3.fromHSV(0.5, 0.9, 1)),
            guns = {
                ['[Revolver]'] = {
                    location = meshes.BIT8.RPixel,
                    equipped = false,
                    shoot_sound = 'rbxassetid://13326584088',
                    C0 = newCFrame(0.0261230469, -0.042888701, 0.00260925293, -1, 1.355249e-20, -3.55271071e-15, 1.355249e-20, 1, -1.81903294e-27, 3.55271071e-15, -1.81903294e-27, -1)
                },
                ['[Flamethrower]'] = {
                    location = meshes.BIT8.FTPixel,
                    equipped = false,
                    C0 = newCFrame(-0.0906066895, -0.0161985159, -0.0117645264, 1, 0, 0, 0, 1, 0, 0, 0, 1)
                },
                ['[Double-Barrel SG]'] = {
                    location = meshes.BIT8.DBPixel,
                    equipped = false,
                    shoot_sound = 'rbxassetid://13326578563',
                    C0 = newCFrame(-0.240386963, -0.127295256, -0.00776672363, 0, 0, -1, 0, 1, 0, 1, 0, 0)
                },
                ['[RPG]'] = {
                    location = meshes.BIT8.RPGPixel,
                    equipped = false,
                    C0 = newCFrame(0.0102081299, 0.0659624338, 0.362945557, 4.37113883e-08, 0, 1, -5.72632016e-14, 1, 2.50305399e-21, -1, 5.72632016e-14, 4.37113883e-08)
                }
            }
        },
        ['Electric'] = {
            color = newColorSequence(Color3.new(0, 1, 1), Color3.new(0.666667, 1, 1)),
            guns = {
                ['[Revolver]'] = {
                    location = meshes.Electric.ElectricRevolver,
                    equipped = false,
                    C0 = newCFrame(0.185462952, 0.0312761068, 0.000610351562, 0, 0, -1, 0, 1, 0, 1, 0, 0)
                },
                ['[DrumGun]'] = {
                    location = meshes.Electric.ElectricDrum,
                    equipped = false,
                    C0 = newCFrame(-0.471969604, 0.184426308, 0.075378418, 1, 0, 0, 0, 1, 0, 0, 0, 1)
                },
                ['[SMG]'] = {
                    location = meshes.Electric.ElectricSMG,
                    equipped = false,
                    C0 = newCFrame(-0.0620956421, 0.109580457, 0.00729370117, 1, 0, 0, 0, 1, 0, 0, 0, 1)
                },
                ['[Shotgun]'] = {
                    location = meshes.Electric.ElectricShotgun,
                    equipped = false,
                    C0 = newCFrame(6.10351562e-05, 0.180232108, -0.624732971, 1, 0, -4.37113883e-08, 0, 1, 0, 4.37113883e-08, 0, 1)
                },
                ['[Rifle]'] = {
                    location = meshes.Electric.ElectricRifle,
                    equipped = false,
                    C0 = newCFrame(0.181793213, -0.0415201783, 0.00421142578, 1.8189894e-12, 6.6174449e-24, 1, 7.27595761e-12, 1, 6.6174449e-24, -1, -7.27595761e-12, -1.8189894e-12)
                },
                ['[P90]'] = {
                    location = meshes.Electric.ElectricP90,
                    equipped = false,
                    C0 = newCFrame(0.166191101, -0.225557804, -0.0075378418, 1, 0, 0, 0, 1, 0, 0, 0, 1)
                },
                ['[LMG]'] = {
                    location = meshes.Electric.ElectricLMG,
                    equipped = false,
                    C0 = newCFrame(0.142379761, 0.104723871, -0.303771973, -1, 0, -4.37113883e-08, 0, 1, 0, 4.37113883e-08, 0, -1)
                },
                ['[Flamethrower]'] = {
                    location = meshes.Electric.ElectricFT,
                    equipped = false,
                    C0 = newCFrame(-0.158782959, 0.173444271, 0.00640869141, 1, 0, 0, 0, 1, 0, 0, 0, 1)
                },
                ['[Double-Barrel SG]'] = {
                    location = meshes.Electric.ElectricDB,
                    equipped = false,
                    C0 = newCFrame(0.0755996704, -0.0420352221, 0.00543212891, 1, 0, 0, 0, 1, 0, 0, 0, 1)
                },
                ['[Glock]'] = {
                    location = meshes.Electric.ElectricGlock,
                    equipped = false,
                    C0 = newCFrame(-0.00207519531, 0.0318723917, 0.0401077271, 0, 0, -1, 0, 1, 0, 1, 0, 0)
                },
                ['[AUG]'] = {
                    location = meshes.Electric.ElectricAUG,
                    equipped = false,
                    C0 = newCFrame(0.331085205, -0.0117390156, 0.00155639648, 1, 0, 0, 0, 1, 0, 0, 0, 1)
                },
                ['[SilencerAR]'] = {
                    location = meshes.Electric.ElectricAR,
                    equipped = false,
                    C0 = newCFrame(-0.16942215, 0.0508521795, 0.0669250488, 1, 0, 0, 0, 1, 0, 0, 0, 1)
                },
                ['[AK47]'] = {
                    location = meshes.Electric.ElectricAK,
                    equipped = false,
                    C0 = newCFrame(0.155792236, 0.18423444, 0.00140380859, 0, 0, -1, 0, 1, 0, 1, 0, 0)
                }
            }
        },
        --[[['Shadow'] = {
            Color = ColorSequence.new(Color3.new(0.560784, 0.470588, 1), Color3.new(0.576471, 0.380392, 1)),
            ['Rifle'] = {
                Equipped = false
            },
            ['Shotgun'] = {
                Equipped = false,
                Location = meshes.Shadow.ShotgunGhost,
                CFrame = CFrame.new(3.05175781e-05, 0.199999928, 3.81469727e-06, -1, 0, -4.37113883e-08, 0, 1, 0, 4.37113883e-08, 0, -1)
            },
            ['Revolver'] = {
                Equipped = false,
                Location = meshes.Shadow.RevolverGhost,
                CFrame = CFrame.new(1.52587891e-05, 0, 0, 1, 0, 8.74227766e-08, 0, 1, 0, -8.74227766e-08, 0, 1)
            }
        }]]
        ['Halloween23'] = {
            guns = {
                ['[Revolver]'] = {
                    equipped = false,
                    location = meshes.Halloween.Rev,
                    shoot_sound = 'rbxassetid://14924285721',
                    C0 = newCFrame(-0.0257873535, -0.0117108226, -0.00671386719, -1, 0, 0, 0, 1, 0, 0, 0, -1)
                },
                ['[Double-Barrel SG]'] = {
                    equipped = false,
                    location = meshes.Halloween.DB,
                    shoot_sound = 'rbxassetid://14924282919',
                    C0 = newCFrame(-0.00271606445, -0.0485508144, 0.000732421875, 1, 0, 0, 0, 1, 0, 0, 0, 1)
                },
                ['[Shotgun]'] = {
                    equipped = false,
                    location = meshes.Halloween.SG,
                    shoot_sound = 'rbxassetid://14924268000',
                    C0 = newCFrame(0.00573730469, 0.294590235, -0.115814209, 0, 0, -1, 0, 1, 0, 1, 0, 0)
                },
                ['[TacticalShotgun]'] = {
                    equipped = false,
                    location = meshes.Halloween.Tact,
                    shoot_sound = 'rbxassetid://14924256223',
                    C0 = newCFrame(-0.0715637207, -0.0843618512, 0.00582885742, -1, 0, 0, 0, 1, 0, 0, 0, -1)
                }
            }
        },
        ['Soul'] = {
            guns = {
                ['[Revolver]'] = {
                    equipped = false,
                    location = meshes.Soul.rev,
                    shoot_sound = 'rbxassetid://14909152822',
                    C0 = newCFrame(-0.0646362305, 0.2725088, -0.00242614746, 0, 0, -1, 0, 1, 0, 1, 0, 0)
                },
                ['[Double-Barrel SG]'] = {
                    equipped = false,
                    location = meshes.Soul.db,
                    shoot_sound = 'rbxassetid://14909164664',
                    C0 = newCFrame(0.405822754, 0.0975035429, -0.00506591797, -1, 0, 0, 0, 1, 0, 0, 0, -1)
                },
                ['[TacticalShotgun]'] = {
                    equipped = false,
                    location = meshes.Soul.tact,
                    shoot_sound = 'rbxassetid://14918188609',
                    C0 = newCFrame(-0.347473145, 0.0268714428, 0.00553894043, 1, 0, 0, 0, 1, 0, 0, 0, 1)
                }
            }
        },
        ['Heaven'] = {
            color = newColorSequence(Color3.new(1, 1, 1)),
            tween_duration = 1.25,
            easing_style = Enum.EasingStyle.Quad,
            easing_direction = Enum.EasingDirection.Out,
            beam_width = 0.13,
            guns = {
                ['[Revolver]'] = {
                    equipped = false,
                    location = meshes.Heaven.Revolver,
                    C0 = newCFrame(-0.0829315186, -0.0831851959, -0.00296020508, -0.999999881, 2.94089277e-17, 8.27179774e-25, -2.94089277e-17, 0.999999881, 6.85215614e-16, 8.27179922e-25, -6.85215667e-16, -1)
                },
                ['[Double-Barrel SG]'] = {
                    equipped = false,
                    location = meshes.Heaven.DB,
                    shoot_sound = 'rbxassetid://14489852879',
                    C0 = newCFrame(-0.0303955078, 0.022110641, 0.00296020508, -0.999997139, -7.05812226e-16, 7.85568618e-30, 7.05812226e-16, 0.999997139, -2.06501178e-14, 6.44518474e-30, 2.06501042e-14, -0.999999046)
                }
            }
        },
        ['Void'] = {
            guns = {
                ['[Revolver]'] = {
                    equipped = false,
                    location = meshes.Void.rev,
                    C0 = newCFrame(-0.00503540039, 0.0082899332, -0.00164794922, 0, 0, -1, 0, 1, 0, 1, 0, 0)
                },
                ['[TacticalShotgun]'] = {
                    equipped = false,
                    location = meshes.Void.tact,
                    C0 = newCFrame(0.0505371094, -0.0487936139, 0.00158691406, 0, 0, 1, 0, 1, 0, -1, 0, 0)
                }
            }
        },
        ['DH-Stars II'] = {
            guns = {
                ['[Revolver]'] = {
                    equipped = false,
                    location = meshes.Popular.STARSREV,
                    C0 = newCFrame(0.0578613281, -0.0479719043, -0.00115966797, -1.00000405, 1.15596135e-16, 1.64267286e-30, -1.15596135e-16, 1, 2.99751983e-14, 1.66683049e-30, -2.99751983e-14, -1.00000405)
                }
            }
        },
        ['DH-Verified'] = {
            guns = {
                ['[Revolver]'] = {
                    equipped = false,
                    location = meshes.Popular.VERIFIEDREV,
                    C0 = newCFrame(0.049407959, -0.0454721451, 0.00158691406, -1, 0, 0, 0, 1, 2.22044605e-15, 0, -2.22044605e-15, -1)
                }
            }
        },
        ['Candy'] = {
            guns = {
                ['[Revolver]'] = {
                    equipped = false,
                    location = meshes.Candy.RevolverCandy,
                    C0 = newCFrame(-0.106658936, -0.0681198835, 0.00198364258, 0, 0, -1, 0, 1, 0, 1, 0, 0)
                },
                ['[Double-Barrel SG]'] = {
                    equipped = false,
                    location = meshes.Candy.DBCandy,
                    C0 = newCFrame(0.0430603027, -0.0375298262, -0.00198364258, 0, 0, 1, 0, 1, 0, -1, 0, 0)
                }
            }
        }
    };

    mkelement = function(class, parent, props)
        local obj = Instance.new(class);

        for i, v in next, props do
            obj[i] = v;
        end;

        obj.Parent = parent;
        return obj;
    end;

    find_gun = (function(gun_name, instance)
        for i, v in next, instance:GetChildren() do
            if v:IsA('Tool') then
                if (v.Name == gun_name) then
                    return v
                end
            end
        end
    end);

    InventoryChanger.Functions.GameEquip = function(gun, skin)
        return replicated_storage.MainEvent:FireServer('EquipWeaponSkins', gun, skin);
    end;

    InventoryChanger.Functions.AddOwnedSkins = function()
        for _, v in ipairs(entries:GetChildren()) do
            local ext_name = v.Name:match('%[(.-)%]');
            local skin_name, _ = v.Name:gsub('%[.-%]', '');
            if 
                ext_name 
                and skin_name 
                and InventoryChanger.Skins[skin_name] 
                and InventoryChanger.Skins[skin_name].guns 
                and InventoryChanger.Skins[skin_name].guns['[' .. ext_name .. ']']
            then
                local Preview = v:FindFirstChild('Preview');

                if Preview and Preview:FindFirstChild('Equipped') and Preview.Equipped.Visible then
                    table.insert(InventoryChanger.Owned, { frame = v, gun = '[' .. ext_name .. ']' })
                end;
            end;
        end;
    end;

    InventoryChanger.Functions.UnequipGameSkins = function()
        for _, v in ipairs(InventoryChanger.Owned) do
            local SkinInfo = v.frame.SkinInfo;
            local Container = SkinInfo.Container;
            local SkinName = Container.SkinName.Text;

            InventoryChanger.Functions.GameEquip(v.gun, SkinName)
        end;
    end;

    InventoryChanger.Functions.Unload = function()
        return Utilities.Unload();
    end;

    InventoryChanger.Functions.Reload = function()
        local function wait_for_child(parent, child)
            local child = parent:WaitForChild(child);
            while not child do
                child = parent:WaitForChild(child);
            end;
            return child;
        end;
        
        client = players.LocalPlayer;
        player_gui = client.PlayerGui;

        repeat task.wait() until player_gui;

        main_gui = wait_for_child(player_gui, 'MainScreenGui');
        crew = wait_for_child(main_gui, 'Crew');

        bottom_left = wait_for_child(crew, 'BottomLeft');
        bottom_left = bottom_left.Frame;

        skins_button = wait_for_child(bottom_left, 'Skins');

        weapon_skins_gui = wait_for_child(main_gui, 'WeaponSkinsGUI');
        
        gui_body_wrapper = wait_for_child(weapon_skins_gui, 'Body');
        body_wrapper = wait_for_child(gui_body_wrapper, 'Wrapper');
        
        skin_view = wait_for_child(body_wrapper, 'SkinView');
        skin_view_frame = wait_for_child(skin_view, 'Frame');

        guns = wait_for_child(skin_view_frame, 'Guns').Contents;
        entries = wait_for_child(skin_view_frame, 'Skins').Contents.Entries;

        InventoryChanger.Functions.Unload();

        cout('Reload', 'Script successfully reloaded!');
        cout('Reload', 'Waiting for skins to load...');

        wait_for_child(entries, '[Revolver]Golden Age');

        cout('Reload', 'Skins successfully loaded.');
        InventoryChanger.Functions.AddOwnedSkins();
        InventoryChanger.Functions.UnequipGameSkins();

        for i, v in next, guns:GetChildren() do
            if v:IsA('Frame') and v.Name ~= 'GunEntry' and v.Name ~= 'Trading' and v.Name ~= '[Mask]' then
                Utilities.AddConnection(v.Button.MouseButton1Click, function()
                    local extracted_name = v.Name:match(regex);
                    if extracted_name then
                        InventoryChanger.Functions.Start(extracted_name);
                    end;
                end);
            end;
        end;
    end;

    InventoryChanger.Functions.Equip = function(gun_name, skin_name)
        print('[DEBUG]', 'Equip function has been invoked.', gun_name, skin_name or 'Default')
        local gun = find_gun(gun_name, client.Backpack) or find_gun(gun_name, client.Character);
        if not skin_name then
            if gun and gun.Name == gun_name then
                for _, v in next, gun.Default:GetChildren() do v:Destroy() end;
                
                gun.Default.Transparency = 0;
                --if InventoryChanger.Selected[gun.Name] and not InventoryChanger.Skins[InventoryChanger.Selected[gun.Name]].Location then
                    --gun.Default.TextureID = 'rbxassetid://8117372147';
                --end;
                
                if gun.Name == '[Silencer]' or gun.Name == '[SilencerAR]' then
                    gun:FindFirstChild('Part').Transparency = 0;
                end;

                local skin_name = InventoryChanger.Selected[gun.Name];

                if skin_name and InventoryChanger.Skins[skin_name] and InventoryChanger.Skins[skin_name].guns and InventoryChanger.Skins[skin_name].guns[gun.Name] then
                    if InventoryChanger.Skins[skin_name].guns[gun.Name].TracerLoop then
                        InventoryChanger.Skins[skin_name].guns[gun.Name].TracerLoop:Disconnect();
                        InventoryChanger.Skins[skin_name].guns[gun.Name].TracerLoop = nil;
                    end;

                    if InventoryChanger.Skins[skin_name].guns[gun.Name].shoot_sound_loop then
                        InventoryChanger.Skins[skin_name].guns[gun.Name].shoot_sound_loop:Disconnect();
                        InventoryChanger.Skins[skin_name].guns[gun.Name].shoot_sound_loop = nil;
                    end;
                end;
            end;

            return;
        end;
        
        if gun and gun.Name == gun_name and skin_name then
            cout('DEBUG', 'Has skin name');
            local skin_pack = InventoryChanger.Skins[skin_name];
            local guns = skin_pack.guns;
            if skin_pack and guns and not skin_pack.texture then
                cout('DEBUG', 'Changing skin assets');
                for _, x in next, gun.Default:GetChildren() do x:Destroy() end;
                
                local clone = guns[gun_name].location:Clone();
                clone.Name = 'Mesh';
                clone.Parent = gun.Default;
                
                local weld = Instance.new('Weld', clone);
                weld.Part0 = gun.Default;
                weld.Part1 = clone;
                weld.C0 = guns[gun_name].C0;
                
                gun.Default.Transparency = 1;

                if guns[gun_name].shoot_sound then
                    if guns[gun_name].shoot_sound_loop then
                        guns[gun_name].shoot_sound_loop:Disconnect();
                        guns[gun_name].shoot_sound_loop = nil;
                    end;
                    gun.Handle.ShootSound.SoundId = guns[gun_name].shoot_sound;
                    guns[gun_name].shoot_sound_loop = gun.Handle.ChildAdded:Connect(function(child)
                        if child:IsA('Sound') and child.Name == 'ShootSound' then
                            child.SoundId = guns[gun_name].shoot_sound;
                        end;
                    end);
                end;
            end;
        end;
    end;

    InventoryChanger.Functions.Start = function(name)
        for i, v in next, entries:GetChildren() do
            local skin_name, _ = v.Name:gsub('%[.-%]', '');

            if string.find(v.Name, name, 1, true) and InventoryChanger.Skins[skin_name] and InventoryChanger.Skins[skin_name].guns and InventoryChanger.Skins[skin_name].guns['['..name..']'] and InventoryChanger.Skins[skin_name].guns['['..name..']'].location then
                local Preview = v:FindFirstChild('Preview');
                local Button = v:FindFirstChild('Button');
                local skinInfo = v:FindFirstChild('SkinInfo');

                if Preview and Button and skinInfo then
                    local Label = Preview:FindFirstChild('LockImageLabel');
                    local AmountValue = Preview:FindFirstChild('AmountValue');
                    local Equipped = Preview:FindFirstChild('Equipped');
                    local container = skinInfo:FindFirstChild('Container');

                    local extracted_name = v.Name:match(regex);

                    if Equipped and extracted_name then
                        Equipped.Visible = InventoryChanger.Skins[skin_name] and InventoryChanger.Skins[skin_name].guns['['..extracted_name..']'] and InventoryChanger.Skins[skin_name].guns['['..extracted_name..']'].equipped or false;
                        InventoryChanger.Functions.Equip('['..extracted_name..']', InventoryChanger.Selected['['..extracted_name..']'])

                        if Label then
                            Label.Visible = false;
                        end;

                        if container and container.SellButton then
                            container.SellButton.Visible = true;
                        end;
                    
                        if AmountValue then
                            AmountValue.Visible = true;
                            AmountValue.Text = 'x1';
                        end;
                    
                        if getgenv().InventoryConnections[v.Name] then
                            getgenv().InventoryConnections[v.Name]:Disconnect();
                            getgenv().InventoryConnections[v.Name] = nil;
                        end;

                        v.Button:Destroy();
                        local props = { Text = '',BackgroundTransparency = 1,Size = UDim2.new(1, 0, 0.7, 0),ZIndex = 5,Name = 'Button',Position = UDim2.new(0, 0, 0, 0)};
                        local new_btn = mkelement('TextButton', v, props);

                        getgenv().InventoryConnections[v.Name] = new_btn.MouseButton1Click:Connect(function()
                            InventoryChanger.Skins[skin_name].guns['['..extracted_name..']'].equipped = not InventoryChanger.Skins[skin_name].guns['['..extracted_name..']'].equipped;
                            InventoryChanger.Selected['['..extracted_name..']'] = InventoryChanger.Skins[skin_name].guns['['..extracted_name..']'].equipped and skin_name or nil;
                            Equipped.Visible = InventoryChanger.Skins[skin_name].guns['['..extracted_name..']'].equipped;

                            for k, x in ipairs(entries:GetChildren()) do
                                if x.Name:match(regex) == extracted_name and x ~= v then
                                    x.Preview.Equipped.Visible = false;

                                    for _, l in next, InventoryChanger.Skins do
                                        if _ ~= skin_name and l['['..extracted_name..']'] and l['['..extracted_name..']'].equipped then
                                            l[extracted_name].equipped = false
                                        end;
                                    end;
                                end;
                                
                                if x ~= v and string.find(x.Name, name, 1, true) and InventoryChanger.Skins[skin_name] and InventoryChanger.Skins[skin_name].guns and InventoryChanger.Skins[skin_name].guns['['..name..']'] and InventoryChanger.Skins[skin_name].guns['['..name..']'].location then
                                    local Preview = v:FindFirstChild('Preview');
                                    local Button = v:FindFirstChild('Button');
                                    local skinInfo = v:FindFirstChild('SkinInfo');
                                    
                                    if Preview and Button and skinInfo then
                                        local Label = Preview:FindFirstChild('LockImageLabel');
                                        local AmountValue = Preview:FindFirstChild('AmountValue');
                                        local Equipped = Preview:FindFirstChild('Equipped');
                                        local container = skinInfo:FindFirstChild('Container');
                                        
                                        if Label then
                                            Label.Visible = false;
                                        end;
                        
                                        if container and container.SellButton then
                                            container.SellButton.Visible = true;
                                        end;
                                        
                                        if AmountValue then
                                            AmountValue.Visible = true;
                                            AmountValue.Text = 'x1';
                                        end;
                                    end;

                                    InventoryChanger.Owned = {};
                                    InventoryChanger.Functions.AddOwnedSkins();
                                    InventoryChanger.Functions.UnequipGameSkins();
                                end;
                            end;
                        end);
                    end;
                end;
            end;
        end;
    end;

    InventoryChanger.Functions.CharacterAdded = function(character)
        if getgenv().InventoryConnections.ChildAdded then
            getgenv().InventoryConnections.ChildAdded:Disconnect();
            getgenv().InventoryConnections.ChildAdded = nil;
        end;

        if getgenv().InventoryConnections.ChildRemoved then
            getgenv().InventoryConnections.ChildRemoved:Disconnect();
            getgenv().InventoryConnections.ChildRemoved = nil;
        end;

        getgenv().InventoryConnections.ChildAdded = character.ChildAdded:Connect(function(child)
            if child:IsA('Tool') and child:FindFirstChild('GunScript') then
                InventoryChanger.Functions.Equip(child.Name, InventoryChanger.Selected[child.Name]);
                local skin_name = InventoryChanger.Selected[child.Name];
                
                if skin_name then
                    if InventoryChanger.Skins[skin_name].color and InventoryChanger.Skins[skin_name].guns[child.Name].equipped then
                        if InventoryChanger.Skins[skin_name].guns[child.Name].TracerLoop then
                            InventoryChanger.Skins[skin_name].guns[child.Name].TracerLoop:Disconnect();
                            InventoryChanger.Skins[skin_name].guns[child.Name].TracerLoop = nil;
                        end;

                        InventoryChanger.Skins[skin_name].guns[child.Name].TracerLoop = Ignored.DescendantAdded:Connect(function(descendant)
                            local gun = find_gun(child.Name, client.Character) or nil;

                            if gun and descendant:IsDescendantOf(siren) and descendant:IsA('Beam') then
                                local pos1 = (descendant.Attachment0.WorldCFrame.Position.X > gun.Handle.CFrame.Position.X) and descendant.Attachment0.WorldCFrame.Position or gun.Handle.CFrame.Position;
                                local pos2 = (descendant.Attachment0.WorldCFrame.Position.X < gun.Handle.CFrame.Position.X) and descendant.Attachment0.WorldCFrame.Position or gun.Handle.CFrame.Position;

                                if math.abs(client.Character.HumanoidRootPart.Velocity.X) < 22 and (pos1 - pos2).Magnitude < 5 or (pos1 - pos2).Magnitude < 20 then
                                    local skin_pack = InventoryChanger.Skins[skin_name];
                                    local guns = skin_pack and skin_pack.guns or nil
                                    local tween_duration = skin_pack and (skin_pack.tween_duration or guns and guns[gun.Name] and guns[gun.Name].tween_duration) or nil;
                                    local width = skin_pack and (skin_pack.beam_width or guns and guns[gun.Name] and guns[gun.Name].beam_width) or nil;
                                    local color = skin_pack and (skin_pack.color or guns and guns[gun.Name] and guns[gun.Name].color) or nil;
                                    local easing_direction = skin_pack and (skin_pack.easing_direction or guns and guns[gun.Name] and guns[gun.Name].easing_direction) or nil;
                                    local easing_style = skin_pack and (skin_pack.easing_style or guns and guns[gun.Name] and guns[gun.Name].easing_style) or nil;

                                    if skin_pack and tween_duration and color then
                                        local clonedParent = descendant.Parent:Clone();

                                        clonedParent.Parent = workspace.Vehicles;
                                        descendant.Parent:Destroy();
                                        if width then
                                            clonedParent:FindFirstChild('GunBeam').Width1 = width;
                                        end;
                                        clonedParent:FindFirstChild('GunBeam').Color = color;
                                        Utilities.Tween({
                                            object = clonedParent:FindFirstChild('GunBeam'),
                                            info = { tween_duration, easing_style, easing_direction },
                                            properties = { Width1 = 0 },
                                            callback = function()
                                                clonedParent:Destroy();
                                            end
                                        })
                                    elseif color then
                                        descendant.Color = color;
                                    end;
                                end;
                            end;
                        end);
                    else
                        if InventoryChanger.Skins[skin_name].guns[child.Name].TracerLoop then
                            InventoryChanger.Skins[skin_name].guns[child.Name].TracerLoop:Disconnect();
                            InventoryChanger.Skins[skin_name].guns[child.Name].TracerLoop = nil;
                        end;
                    end;
                end;
            end;
        end);

        getgenv().InventoryConnections.ChildRemoved = character.ChildRemoved:Connect(function(child)
            if child:IsA('Tool') and child:FindFirstChild('GunScript') then
                InventoryChanger.Functions.Equip(child.Name, false);

                local skin_name = InventoryChanger.Selected[child.Name];

                if skin_name then
                    if InventoryChanger.Skins[skin_name].guns[child.Name].TracerLoop then
                        InventoryChanger.Skins[skin_name].guns[child.Name].TracerLoop:Disconnect();
                        InventoryChanger.Skins[skin_name].guns[child.Name].TracerLoop = nil;
                    end;
                end;
            end;
        end);
        
        InventoryChanger.Functions.Reload();
    end;

    if getgenv().InventoryConnections.CharacterAdded then
        getgenv().InventoryConnections.CharacterAdded:Disconnect();
        getgenv().InventoryConnections.CharacterAdded = nil;
    end;
    getgenv().InventoryConnections.CharacterAdded = client.CharacterAdded:Connect(InventoryChanger.Functions.CharacterAdded);    InventoryChanger.Functions.CharacterAdded(client.Character);end;

-- ============================================================================
--  VITALITY UI LIBRARY - EXAMPLE / FEATURE TEST
--
--  Point LIBRARY_URL at your own raw link (GitHub raw, Pastebin raw, gist raw)
--  and run this file. Every element the library exposes gets built once here.
--
--  Menu bind defaults to Right Shift (Settings -> Menu -> Menu Bind).
-- ============================================================================

local LIBRARY_URL = "https://raw.githubusercontent.com/cueshut/vitremake/main/vitality.lua"

local Library = loadstring(game:HttpGet(LIBRARY_URL))()

--========================== WINDOW + HUD ============================--
local Window = Library:Window({ Name = "Vitality" })

local Watermark = Library.WatermarkObject or Library:Watermark({
	Name = "Vitality",
	SubName = "demo"
})

local Console = Library.ConsoleObject or Library:Console({
	Name = "Console",
	Size = UDim2.fromOffset(420, 385),
	Position = UDim2.new(0.5, 262, 0, 150)
})

Library.KeyList = Library.KeyList or Library:KeybindList({ Name = "Keybind" })

-- Callbacks report into the console window so you can watch them fire while
-- clicking around the demo.
local function Log(Text)
	if Console then
		Console:Log(Text, "Output")
	end
end

-- Multi selects hand back a table, single selects a plain value.
local function Describe(Value)
	if typeof(Value) == "table" then
		local Parts = { }

		for Key, Item in pairs(Value) do
			if Item == true then
				table.insert(Parts, tostring(Key))
			elseif typeof(Item) ~= "table" then
				table.insert(Parts, tostring(Item))
			end
		end

		table.sort(Parts)
		return "{" .. table.concat(Parts, ", ") .. "}"
	end

	return tostring(Value)
end

local Options = { "option1", "option2", "option3", "option4", "option5" }

-- A label wraps on its own and honours newlines, so one label is both the
-- paragraph and the hand broken lines under it.
local Paragraph = table.concat({
	"This Is A Text Label That Can Also Be Used As A Paragraph. The Quick Brown Fox Jumps Over The Lazy Dog.",
	"You Can Also Break Lines",
	"Like This",
	"Or Like This"
}, "\n")

-- Every column in the reference holds the same stack of elements, so the
-- stack is written once and stamped wherever it is needed. Prefix keeps the
-- flags unique, which is what lets a config save each copy separately.
--   "full"   every element, as in the left column
--   "single" stops after the single dropdown, as in the right column
--   "tab"    same but without the tooltip toggle, as in the 1 / 2 / 3 tabs
local function BuildShowcase(Section, Prefix, Mode)
	-- A Tooltip puts a faint "(?)" after the name; hovering the mark opens a
	-- small box with the element name as its title and this text underneath.
	Section:Toggle({
		Name = "Toggle",
		Flag = Prefix .. "_toggle",
		Default = false,
		Tooltip = "Plain On Or Off Switch. This One Reports Every Change Into The Console Window.",
		Callback = function(Value)
			Log(Prefix .. " toggle -> " .. tostring(Value))
		end
	})

	Section:Toggle({
		Name = "Enabled Toggle",
		Flag = Prefix .. "_enabled",
		Default = true
	})

	if Mode ~= "tab" then
		Section:Toggle({
			Name = "Tooltip Multiple Toggle",
			Flag = Prefix .. "_tooltip",
			Default = false,
			Tooltip = "This Toggle Carries A Tooltip.\nIt Can Run Over Several Lines,\nAnd It Follows The Mouse."
		})
	end

	Section:Slider({
		Name = "Integer Slider",
		Flag = Prefix .. "_integer",
		Min = 0,
		Max = 100,
		Default = 50,
		Decimals = 0,
		Suffix = "%",
		Callback = function(Value)
			Log(Prefix .. " integer -> " .. tostring(Value))
		end
	})

	Section:Slider({
		Name = "Float Slider",
		Flag = Prefix .. "_float",
		Min = 0,
		Max = 100,
		Default = 0,
		Decimals = 0.1,
		Suffix = "m/s"
	})

	Section:Slider({
		Name = "Decimal Slider",
		Flag = Prefix .. "_decimal",
		Min = 0,
		Max = 5,
		Default = 0.5,
		Decimals = 0.01,
		Suffix = "s"
	})

	-- String sliders step through words instead of numbers.
	Section:Slider({
		Name = "String Slider",
		Flag = Prefix .. "_string",
		String = { "off", "low", "medium", "high", "inf" },
		Default = 5
	})

	-- Range sliders put two handles on one track and read "low - high".
	Section:Slider({
		Name = "Range Slider",
		Flag = Prefix .. "_range",
		Min = 0,
		Max = 1000,
		Range = { 0, 1000 }
	})

	Section:Label({ Name = Paragraph })

	Section:Dropdown({
		Name = "Dropdown (Single)",
		Flag = Prefix .. "_single",
		Items = Options,
		Default = "option1",
		Callback = function(Value)
			Log(Prefix .. " single -> " .. Describe(Value))
		end
	})

	if Mode ~= "full" then
		return
	end

	Section:Dropdown({
		Name = "Dropdown (Multi)",
		Flag = Prefix .. "_multi",
		Items = Options,
		Multi = true,
		Default = { "option1" },
		Callback = function(Value)
			Log(Prefix .. " multi -> " .. Describe(Value))
		end
	})

	-- No Default means nothing is picked until you pick something.
	Section:Dropdown({
		Name = "Dropdown (No Default)",
		Flag = Prefix .. "_nodefault",
		Items = Options
	})
end

--================================ MAIN ==============================--
local MainPage = Window:Page({ Name = "Main" })

BuildShowcase(MainPage:Section({ Name = "Section 1", Side = 1 }), "main_left", "full")
BuildShowcase(MainPage:Section({ Name = "Section 1", Side = 2 }), "main_right", "single")

-- Tabbed sub section under the right hand column. The tab strip can be
-- dragged out of the window, and the bottom edge resizes the box.
local MainTabs = MainPage:MultiSection({ Side = 2, Height = 260 })

for Index = 1, 3 do
	BuildShowcase(MainTabs:Section({ Name = tostring(Index) }), "main_tab" .. Index, "tab")
end

-- Bottom left of the Main tab: the pop-up toggle, a dropdown long enough to
-- raise its own search field, and every button width including the
-- confirmation prompt.
local MainExtras = MainPage:Section({ Name = "Section 2", Side = 1 })

-- While this is off the row below it takes up no space at all; clicking it
-- rolls the slider and the dropdown open underneath.
local MainPopupToggle = MainExtras:Toggle({
	Name = "Pop Up Toggle",
	Flag = "main_popup",
	Default = false,
	Tooltip = "Rolls The Slider And Dropdown Below Open And Shut. Nothing Is Reserved For Them While This Is Off."
})

local MainPopup = MainPopupToggle:Popup()

MainPopup:Slider({
	Name = "Pop Up Slider",
	Flag = "main_popup_slider",
	Min = 0,
	Max = 100,
	Default = 50,
	Suffix = "%"
})

MainPopup:Dropdown({
	Name = "Pop Up Dropdown",
	Flag = "main_popup_dropdown",
	Items = Options,
	Default = "option1"
})

-- Any list longer than Library.SearchThreshold raises a filter field of its
-- own, sitting just above the option list.
local LongOptions = { }

for Index = 1, 14 do
	table.insert(LongOptions, "option" .. Index)
end

MainExtras:Dropdown({
	Name = "Dropdown (Search)",
	Flag = "main_search",
	Items = LongOptions,
	Default = "option1",
	Callback = function(Value)
		Log("search dropdown -> " .. Describe(Value))
	end
})

-- Large fills the row, Medium pairs up, Small fits three across.
MainExtras:Button({ Name = "Large Button", Callback = function() Log("large button") end })
MainExtras:Button({ Name = "Medium", Size = "Medium", Callback = function() Log("medium button") end })
MainExtras:Button({ Name = "Medium", Size = "Medium", Callback = function() Window:Center() end })
MainExtras:Button({ Name = "Small", Size = "Small", Callback = function() Log("small button") end })
MainExtras:Button({ Name = "Small", Size = "Small", Callback = function() Console:Clear() end })
MainExtras:Button({
	Name = "Small",
	Size = "Small",
	Callback = function()
		Library:Notification({ Name = "notification fired", Time = 3 })
	end
})

-- Confirm turns the first click into a countdown prompt; the callback only
-- runs if the button is clicked again before the timer runs out.
MainExtras:Button({
	Name = "Confirm Button",
	Confirm = true,
	ConfirmTime = 5,
	Callback = function()
		Log("confirm button ran")
	end
})

--================================ MISC ==============================--
local MiscPage = Window:Page({ Name = "Misc" })

-- Listbox: a permanently open option list with its own filter field, and a
-- clickable showcase graph underneath it.
local function BuildMisc(Section, Prefix, WithTail)
	Section:Toggle({
		Name = "Toggle",
		Flag = Prefix .. "_toggle",
		Default = false,
		Tooltip = "Hover The Mark Beside A Name For A Short Note On What That Module Does."
	})
	Section:Toggle({ Name = "Enabled Toggle", Flag = Prefix .. "_enabled_one", Default = true })
	Section:Toggle({ Name = "Enabled Toggle", Flag = Prefix .. "_enabled_two", Default = true })

	Section:Listbox({
		Flag = Prefix .. "_list",
		Items = Options,
		Default = "option1",
		Search = true,
		Height = 92,
		Callback = function(Value)
			Log(Prefix .. " list -> " .. Describe(Value))
		end
	})

	-- Editable = true makes the plot a button: click it and a full graph
	-- editor rolls open underneath, and dragging the curve feeds straight
	-- back into the line above it.
	Section:Graph({
		Name = "Graph",
		Height = 58,
		Min = 0,
		Max = 10,
		Smooth = true,
		Editable = true,
		Data = { 1, 3, 2, 5, 4, 7, 6, 9, 8, 10 }
	})

	if WithTail then
		Section:Toggle({ Name = "Enabled Toggle", Flag = Prefix .. "_enabled_three", Default = true })
	end
end

-- FullHeight stretches a section down its whole column instead of sizing it
-- to its contents, so both Misc boxes run all the way to the bottom of the
-- page and scroll inside themselves if the contents ever outgrow them.
BuildMisc(MiscPage:Section({ Name = "Section 2", Side = 1, FullHeight = true }), "misc_left", true)
BuildMisc(MiscPage:Section({ Name = "Section 2", Side = 2, FullHeight = true }), "misc_right", false)

--=============================== VISUAL =============================--
-- The reference does not show this tab's contents, so it holds the pieces
-- that hang off an element rather than standing on their own: colour
-- pickers, keybind chips, pop-up boxes, textboxes and buttons.
local VisualPage = Window:Page({ Name = "Visual" })

local VisualLeft = VisualPage:Section({ Name = "Section 3", Side = 1 })

local ChamsToggle = VisualLeft:Toggle({
	Name = "Toggle With Colours",
	Flag = "visual_chams",
	Default = true,
	Tooltip = "Toggles Can Carry Colour Pickers And A Keybind On The Same Row"
})

ChamsToggle:Colorpicker({
	Flag = "visual_chams_visible",
	Default = Color3.fromRGB(181, 32, 55),
	Callback = function(Color)
		Log("visible colour -> " .. tostring(Color))
	end
})

ChamsToggle:Colorpicker({
	Flag = "visual_chams_hidden",
	Default = Color3.fromRGB(255, 255, 255),
	Alpha = 0.5,
	Callback = function(Color, Alpha)
		Log("hidden colour -> " .. tostring(Color) .. " alpha " .. tostring(Alpha))
	end
})

ChamsToggle:Keybind({
	Name = "chams",
	Flag = "visual_chams_bind",
	Default = Enum.KeyCode.F,
	Mode = "Toggle",
	Callback = function(State)
		ChamsToggle:Set(State == true)
	end
})

-- The pop-up toggle sits in the bottom left of the Main tab now.

local VisualRight = VisualPage:Section({ Name = "Section 3", Side = 2 })

VisualRight:Textbox({
	Name = "Textbox",
	Flag = "visual_textbox",
	Placeholder = "type here",
	Finished = true,
	Callback = function(Value)
		Log("textbox -> " .. tostring(Value))
	end
})

VisualRight:Label({ Name = "Label With A Colour Picker" }):Colorpicker({
	Flag = "visual_label_colour",
	Default = Color3.fromRGB(181, 32, 55)
})

VisualRight:Label({ Name = "Label With A Keybind" }):Keybind({
	Name = "label",
	Flag = "visual_label_bind",
	Default = Enum.KeyCode.G
})

-- Buttons come in three widths: Large fills the row, Medium pairs up,
-- Small fits three across.
VisualRight:Button({ Name = "Large Button", Callback = function() Log("large button") end })
VisualRight:Button({ Name = "Medium", Size = "Medium", Callback = function() Log("medium button") end })
VisualRight:Button({ Name = "Medium", Size = "Medium", Callback = function() Window:Center() end })
VisualRight:Button({ Name = "Small", Size = "Small", Callback = function() Log("small button") end })
VisualRight:Button({ Name = "Small", Size = "Small", Callback = function() Console:Clear() end })
VisualRight:Button({
	Name = "Small",
	Size = "Small",
	Callback = function()
		Library:Notification({ Name = "notification fired", Time = 3 })
	end
})

--============================ PLAYER LIST ===========================--
-- The players button in the top bar shows and hides this panel, so the demo
-- has to build one for that button to have anything to open.
Library:PlayerList({
	Name = "Players",
	Flag = "demo_player_priorities",
	Callback = function(Player, Priority)
		Log(tostring(Player) .. " -> " .. tostring(Priority))
	end
})

--============================== SETTINGS ============================--
-- Adds the Settings tab: configs, menu bind, animation, HUD and theming.
Window:InitSettings()

Log("demo loaded, menu bind is Right Shift")
Library:Notification({ Name = "vitality demo loaded", Time = 4 })

return Window

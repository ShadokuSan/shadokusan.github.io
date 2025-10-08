<div align="center"><h1>ShadokuSan's Roblox Library</h1></div>

<div align="center"><h3>This is a module script that makes some common tasks, or tedious ones, easier than before.</h3></div>

#

:warning: This is not intended to be used as a learning tool for coding, and nor do I claim to be an expert in coding. So.. take the quality and readability of this module with a little grain of salt.

**Module Link:** <https://www.roblox.com/library/3165359492/redirect>

**Original DevForum Post:** <https://devforum.roblox.com/t/346953>
___

### **Usage**

Simply use **require(3165359492)** or insert the module manually to use it (but don't forget that requiring a ModuleScript via an ID doesn't work in LocalScripts).

For the sake of the examples in the functions list below, let's assume **ShadLibrary** is the variable used:

```lua
local ShadLibrary = require(3165359492)
```

___

<details><summary><h3>Variables List</h3></summary>

| Variable | Description |
| --- | --- |
| Script | Refers to the module's instance itself. |
| Warnings | Tied to the **Warnings** attribute to the module. This is used to give information in some scripts for potentially incorrect uses but is an instance that may be auto-corrected. |
| ManualErrors | Tied to the **ManualErrors** attribute to the module. Normally, this will insert errors in areas where incorrect usage of the module likely cannot be auto-corrected and tries to send a message that will try to make some sense of what went wrong. |
| Formulas | This is a table that hosts multiple semi-commonly used formulas, put into function form. See [Formulas](#formulas) for more information. |

### Formulas

This is a table that hosts multiple semi-commonly used formulas, put into function form. Followed by, `ShadLibrary.Formulas.NameHere`

| Forumla Name | Format | Description |
| --- | --- | --- |
| GetAngleVector2 | GetAngleVector2(Position1: Vector2, Position2: Vector2) | Returns an angle where **Position1** points at **Position2**. |
| RayReflection | RayReflection (DirectionNormal: Vector3, SurfaceNormal: Vector3, Modifier: number?) | Returns a Vector3 direction/normal by taking the **DirectionNormal** and bouncing it off of the **SurfaceNormal**; angle depends on the **Modifier** which is defaulted at 2. |
| PythagoreanTheorem | PythagoreanTheorem(Number1: number, Number2: number) | Simply returns the result of √**Number1**<sup>2</sup> + **Number2**<sup>2</sup> |
| PointOnRay | PointOnRay(Point1: Vector3, Point2: Vector3, ReferencePoint: Vector3) | Returns a position by making a ray/line between **Point1** and **Point2**, then uses the **ReferencePoint** to find the closest position from said line. |
| Lerp | Lerp(Start: number, End: number, Alpha: number) | Returns the number between the **Start** and **End** based on the Alpha (between 0-1). |
| TimeConvert | TimeConvert(Seconds: number, TimeUnit: string<"Milliseconds", "Seconds", "Hours", "Days", "Weeks">) | Returns the conversion of seconds to another time unit. |
| CFrameToOrientation | CFrameToOrientation(CF: CFrame) | Simply fetches the orientation of a CFrame. |
| OrientationToCFrame | OrientationToCFrame(Orientation: Vector3) | Simply converts an orientation to usable CFrame angles. |

#

</details>

<details><summary><h3>Functions List</h3></summary>

<details><summary>Create</summary>

**Aliases:** new

**Description:** Customized "Instance.new" function that allows you to edit multiple properties at once.

**Setup:** `ShadLibrary.new("InstanceName", Parent, ParentFirst){ChangeFields}`

**Returns:** The new Instance that was created.
| Variable | Type | Default | Description |
| --- | --- | --- | --- |
| InstanceName | string | REQUIRED | The ClassName of the instance you want to create. |
| Parent | Instance | REQUIRED | Where this instance will be parented under. Occurs after all other properties are set. |
| ParentFirst | boolean | false | Will set the parent before all other properties instead. |
| | | | |
| [ChangeFields](#changefields-setup) | table | REQUIRED | The properties of the instance you're creating. |

This function hosts some special inputs<sub>*Not all may apply*</sub>. Make sure to check the [On Changing Values](#on-changing-values) section for details on how to use them.

### Usage Example

```lua
--Example 1:
--Making a new part that has its Name, Position and Anchored property set, then parented to the workspace
local Part = ShadLibrary.new('Part', workspace){
		properties = {
		Name = "TestPart",
		Position = Vector3.new(0, 5, 6),
		Anchored = true
	}
}

--Example 2:
--Same as Example 1 except the part is parented BEFORE the property changes (though you shouldn't do this)
local Part = ShadLibrary.new('Part', workspace, true){
	Name = "TestPart",
	Position = Vector3.new(0, 5, 6),
	Anchored = true
}
```

___
</details>

<details><summary>Change</summary>

**Description:** Change multiple properties of 1 or more Instances at once.

**Setup:** `ShadLibrary.Change(Instances...){ChangeFields}`

**Returns:** Nothing.
| Variable | Type | Default | Description |
| --- | --- | --- | --- |
| Instances | Instance / {Instance...} | REQUIRED | The instance(s) that you wish to edit. |
| | | | |
| [ChangeFields](#changefields-setup) | table | REQUIRED | A dictionary of the properties/attributes of the instance(s) you're editing. |

This function hosts some special inputs<sub>*Not all may apply*</sub>. Make sure to check the [On Changing Values](#on-changing-values) section for details on how to use them.

### ChangeFields Setup
```lua
ChangeFields<autoFill> = {
	properties: autoFill & {
		[string]: any,
		Sides: SurfaceType
	},
	attributes: {[string]: any?},
	userFunctions: {(autoFill) -> nil}
}
```

### Usage Example

```lua
--Example 1:
--Changes a part under the workspace to be light blue and noncollidable
ShadLibrary.Change(workspace.Part){
	properties = {
		Color = Color3.fromRGB(0, 170, 255),
		CanCollide = false
	}
}

--Example 2:
--Changes multiple BoolValue's value to be false and parents them to the workspace
ShadLibrary.Change(BoolValue1, BoolValue2, BoolValue3){
	properties = {
		Value = false,
		Parent = workspace
	}
}

--Example 3:
--Changes the attributes of a part under the workspace
ShadLibrary.Change(workspace.Part){
	attributes = {
		testNumber = 5,
		testBoolean = false
	}
}

--Example 4:
--Changes all parts in a folder named PartFolder to have their names and anchored properties changed
ShadLibrary.Change(workspace.PartFolder:GetChildren()){
	userFunctions = {
		function(part: BasePart)
			part.Name = "TestPart"
			part.Anchored = false
		end,
	}
}

--Special Inputs 1:
--Sets the value to the opposite of what it currently is
ShadLibrary.Change(BoolValue){
	properties = {
		Value = "not"
	}
}

--Special Inputs 2:
--Adds 5 onto multiple NumberValue's values
ShadLibrary.Change(NumberValue1, NumberValue2){
	properties = {
		Value = "+5"
	}
}

--Special Inputs 4:
--Assume the variable PartTable is defined as an array of BaseParts
--Moves each part in your PartTable 5 studs up independently of each other
ShadLibrary.Change(PartTable){
	properties = {
		Position = "~0, 5, 0"
	}
}

--Special Inputs 5:
--Assume the variable PartTable is defined as an array of BaseParts
--Moves each part in your PartTable 5 studs up relatively via CFrame:ToWorldSpace
ShadLibrary.Change(PartTable){
	properties = {
		CFrame = "~0, 5, 0"
	}
}

--Rotates each part 90 degrees on the Y-axis
ShadLibrary.Change(PartTable){
	properties = {
		CFrame = "@0, 90, 0"
	}
}

--Moves each part in your PartTable 5 studs up relatively via CFrame:ToWorldSpace
--Then applies a 90-degree rotation on the Y-Axis. Using > will do the inverse order.
ShadLibrary.Change(PartTable){
	properties = {
		CFrame = "<0, 5, 0, 0, 90, 0"
	}
}


--Special Inputs 6:
--Runs your given function over each part, expecting an appropriate value in return
function exampleFunction(part: BasePart)
	return part.CFrame:ToWorldSpace(CFrame.new(0, 5, 0))
end

ShadLibrary.Change(PartTable){
	properties = {
		CFrame = exampleFunction
	}
}
```

___
</details>

<details><summary>Clone</summary>

**Aliases:** Copy

**Description:** Clone an item and edit its properties at the same time.

**Setup:** `ShadLibrary.Clone(Item, SameParent, ParentFirst){ChangeFields}`

**Returns:** The clone of the instance.
| Variable | Type | Default | Description |
| --- | --- | --- | --- |
| Item | Instance | REQUIRED | The Instance that you wish to clone. |
| SameParent | boolean | false | Determines if the cloned instance is parented under the same parent as the original. |
| ParentFirst | boolean | false |  Determines if the cloned instance is parented before<sub>(true)</sub> the property changes or after.<sub>(false)</sub> Only takes effect if SameParent is set to true. |
| | | | |
| [ChangeFields](#changefields-setup) | table | REQUIRED | The properties/attributes of the instance you're cloning; if you're changing any. |

This function hosts some special inputs<sub>*Not all may apply*</sub>. Make sure to check the [On Changing Values](#on-changing-values) section for details on how to use them.

### Usage Example

```lua
--Example 1: 
--Clones a part in the workspace then sets it to the same parent
local Brick = ShadLibrary.Clone(workspace.Brick, true){
	properties = {
		Name = "Cloned",
		Position = Vector3.new(0, 10, 0)
	}
}

--Example 2:
--Clones a part in the workspace with a new parent
local Brick = ShadLibrary.Clone(workspace.Brick){
	properties = {
		Name = "Cloned",
		Position = Vector3.new(0, 10, 0),
		Parent = workspace
	}
}
```

___
</details>

<details><summary>Replace</summary>

**Description:** Replace an Instance by creating a new one or cloning another in its place.

**Setup:** `ShadLibrary.Replace(Replacee, Replacement, SameParent, ParentFirst){ChangeFields}`

**Returns:** The replacement instance.
| Variable | Type | Default | Description |
| --- | --- | --- | --- |
| Replacee | Instance | REQUIRED | The Instance that you wish to replace; gets destroyed in the process. |
| Replacement | Instance / string | REQUIRED | The Instance that you wish to clone or a string for the class that you want to create. |
| SameParent | boolean | false | Determines if the cloned instance is parented under the same parent as the original. |
| ParentFirst | boolean | false | Determines if the cloned instance is parented before<sub>(true)</sub> the property changes or after.<sub>(false)</sub> Only takes effect if SameParent is set to true. |
| | | | |
| [ChangeFields](#changefields-setup) | table | REQUIRED | The properties/attributes of the instance you're using as the replacement; if you're changing any. |

This function hosts some special inputs<sub>*Not all may apply*</sub>. Make sure to check the [On Changing Values](#on-changing-values) section for details on how to use them.

### Usage Example

```lua
--Example 1: 
--Destroys Brick1, clones Brick2 and names it Brick3. Variable Brick becomes Brick3. Brick3 gets parented to the same parent as Brick1
local Brick = ShadLibrary.Replace(workspace.Brick1, workspace.Brick2, true){
	properties = {
		Name = "Brick3"
	}
}


--Example 2:
--Destroys Brick1, makes a new part that gets named Brick2, and makes the CFrame the same
local Brick = ShadLibrary.Replace(workspace.Brick1, "Part"){
	properties = {
		Name = "Brick2",
		CFrame = workspace.Brick1.CFrame
	}
}
```

___
</details>

<details><summary>GetInstance</summary>

**Description:** Searches for an Instance or creates a new one if it doesn't yet exist.

**Setup:** `ShadLibrary.GetInstance(Where, Name, ClassName, PropertyType){ChangeFields}`

**Returns:** The Instance that gets found or the newly created one.
| Variable | Type | Default | Description |
| --- | --- | --- | --- |
| Where | Instance | REQUIRED | The Instance that you wish to search in. Only scans the direct children. Also acts as the new Instance's parent if one needs to be made. |
| Name | string | REQUIRED | The name of the Instance you're looking for. Also acts as the new Instance's name if one needs to be made. |
| ClassName | string | REQUIRED | The ClassName of the Instance you're looking for. Also acts as the new Instance's class if one needs to be made. |
| PropertyType | boolean / string | false | Determines the behavior of the search function regarding the Properties table. See below for more details. |
| | | | |
| [ChangeFields](#changefields-setup) | table | REQUIRED | The properties of the new Instance that gets made if needed. |

This function hosts some special inputs<sub>*Not all may apply*</sub>. Make sure to check the [On Changing Values](#on-changing-values) section for details on how to use them.

### PropertyType Usage

• **Match:** When searching for the Name and ClassName of the Instance, it will now also check if the Properties from the Properties Table match.

• **Force:** If an Instance is found that has the correct Name and ClassName, it will automatically change the rest of its properties from that of the Properties table.

• `false` Will do neither.

### Usage Example

```lua
--Example 1: 
--[[
    Searches the game's ReplicatedStorage for a RemoteEvent named "MyRemote".

    Since the 4th variable is false and a RemoteEvent doesn't have any other notable properties,
    the following Properties table here is left empty since it does nothing in this situation.

    If this RemoteEvent did not exist, then this would have automatically made a new RemoteEvent
    with the name "MyRemote" under the game's ReplicatedStorage.

    Note that in any situation where a new Instance needs to be made, it will be parented after every
    other property is set.
]]

local RemoteEvent = ShadLibrary.GetInstance(game:GetService("ReplicatedStorage"), "MyRemote", "RemoteEvent", false){}

--Example 2:
--[[ Searches the Workspace for a NumberValue named "MyNumber" and also has the value of 5 since the
    4th variable is set to "Match"

    If the 4th variable was set to false and a new instance needed to be made, the function would not
    check if the NumberValue had a value of 5 but would still make a new NumberValue that does have a value of 5.
]]

local NumberValue = ShadLibrary.GetInstance(workspace, "MyNumber", "NumberValue", "Match"){
	properties = {
		Value = 5
	}
}

--Example 3:
--[[ Searches the Workspace for a NumberValue named "MyNumber" and will set its value to 5 since the
    4th variable was set to "Force"
]]

local NumberValue = ShadLibrary.GetInstance(workspace, "MyNumber", "NumberValue", "Force"){
	properties = {
		Value = 5
	}
}
```

___
</details>

<details><summary>Destroy</summary>

**Aliases:** Null, Nullify

**Description:** Destroys a bunch of Instances at once.

**Setup:** `ShadLibrary.Destroy(Tuple...)`

**Returns:** Nothing.
| Variable | Type | Default | Description |
| --- | --- | --- | --- |
| Tuple | Instance / Thread / Connection | REQUIRED | The Instance(s) that you're deleting, threads you're cancelling, or connections you're severing. |

### Usage Example

```lua
ShadLibrary.Destroy(workspace.Brick1, workspace.Brick2, workspace.Brick3)
```
___
</details>

<details><summary>Find</summary>

**Aliases:** Search

**Description:** Advanced Instance searcher.

**Setup 1:** `ShadLibrary.Find(Instance, CheckDescendants, MaxAmount, FollowFunction, IgnoreList){SearchFields}`

**Setup 2:** `ShadLibrary.Find(Tag, MaxAmount, FollowFunction, IgnoreList){SearchFields}`

**Setup 3:** `ShadLibrary.Find(..., FollowFunction = "Change", ...){SearchFields}{ChangeFields}`

**Returns:** The instance(s) found or none if none were found, or none if FollowFunction was set to "Destroy"

| Variable | Type | Default | Description |
| --- | --- | --- | --- |
| Instance¹ | Instance | REQUIRED | Where to look. |
| CheckDescendants¹ | boolean | false | If it will search all descendants like :GetAllDescendants() |
| Tag² | string | REQUIRED | Looks through all instances with this tag. |
| MaxAmount | number / false | false | The number of instances you want to be returned. Set to false to have an infinite amount. |
| FollowFunction | string / false | false | Determines if there is a follow-up function after finding the desired instance. Can be false for none, "Change" to allow you to change the instance(s) found, or "Destroy" to destroy all found instances. |
| IgnoreList | table | {} |  An array of Instances that you want ignored, including its children. |
| | | | |
| [SearchFields](#searchfields-setup) | table | REQUIRED | A dictionary of the properties/attributes of the instance(s) you’re looking for. |
| | | | |
| [ChangeFields](#changefields-setup)³ | table | REQUIRED if FollowFunction is set to "Change" | What changes are to be made to the instance(s) found. |

This function hosts some special inputs<sub>*Not all may apply*</sub>. Make sure to check the [On Finding Values](#on-searching-values) section for details on how to use them.

### SearchFields Setup
```lua
SearchFields = {
	properties: {[string]: any | {any}},
	attributes: {[string]: any | {any}},
	functions: {[string]: {
		arguments: any? | {any?},
		interpretFunction: ((...any?) -> boolean)?
	}},
	userFunctions: {(instance: Instance) -> boolean}
}
```


### Usage Example

```lua
--Example 1:
--Returns the first Instance named "Brick" that is anchored
local Get = ShadLibrary.Find(workspace.Model, false, 1){
	properties = {
		Name = "Brick",
		Anchored = true
	}
}

--Example 2:
--Returns all Instances named "Brick" that are anchored
local Get = ShadLibrary.Find(workspace.Model, true, false){
	properties = {
		Name = "Brick",
		Anchored = true
	}
}

--Example 3:
--Returns a maximum of 10 Parts that has an attribute named "testAttribute" set to true
local Get = ShadLibrary.Find(workspace.Model, false, 10){
	properties = {
		ClassName = "Part"
	},
	attributes = {
		testAttribute = true
	}
}

--Example 4:
--Returns any BaseParts found in the workspace
local Get = ShadLibrary.Find(workspace){
	functions = {
		IsA = {
			arguments = "BasePart"
		}
	}
}

--Example 5:
--Returns anything that is not a BasePart in the workspace
local Get = ShadLibrary.Find(workspace){
	functions = {
		IsA = {
			arguments = "BasePart",
			interpretFunction = function(isBasePart: boolean)
				if isBasePart then
					return false
				end
				
				return true
			end,
		}
	}
}

--Special Inputs 1:
--Returns any Instance that has a name ending in "Brick," has a transparency that is less than 1 and their color is dominantly red
local Get = ShadLibrary.Find(workspace.Model){
	properties = {
		Name = "...Brick",
		Transparency = "<1",
		Color="R"
	}
}

--Special Inputs 2:
--Returns any Instance that has a name starting with "Brick," has reflectance set to at least 0.5, and their BrickColor name ends in "red"
local Get = ShadLibrary.Find(workspace.Model){
	properties = {
		Name = "Brick...",
		Reflectance = ">=0.5",
		BrickColor = "...red"
	}
}

--Special Inputs 3:
--Returns any Instance that has their name as Test1 or Test2
local Get = ShadLibrary.Find(workspace.Model, false, true){
	properties = {
		Name = {"Test1", "Test2"}
	}
}
```

___
</details>

<details><summary>FindAllChildren</summary>

**Aliases:** FindAll, FAC

**Description:** Acts like Instance:FindFirstChild() where you can search for multiple instances.

**Setup:** `ShadLibrary.FindAllChildren(Instance, Recursive, Items...)`

**Returns:** The Instances that were found, or false if not.
| Variable | Type | Default | Description |
| --- | --- | --- | --- |
| Instance | Instance | REQUIRED | The Instance you'd like to search in. |
| Recursive | boolean | false | Whether or not the search should be conducted recursively. |
| Items | string | nil | A bunch of strings (for names) you'd like to search for. |

### Usage Example

```lua
--Example 1:
local PartA, PartB, PartC = ShadLibrary.FindAllChildren(workspace, false, "PartA", "PartB", "PartC")
--Will return either the Instances that has those names or false if not.
--Could look like: PartA, false, PartC if there is no PartB

--Example 2:
local Mesh, Texture = ShadLibrary.FindAllChildren(workspace, false, "PartA.Mesh", "PartB.Texture")
--Is capable of searching through multiple instances downwards.

--Example 3:
local PartA, PartB, PartC = ShadLibrary.FindAllChildren(workspace, true, "PartA", "PartB", "PartC")
--Will return the instances if they exist anywhere in the game under workspace.
```

___
</details>

<details><summary>WaitForPath</summary>

**Aliases:** WaitForDescendants, WFP

**Description:** [A solution to checking in long paths without needing the overuse of :WaitForChild a dozen times.](https://devforum.roblox.com/t/554586)

**Setup:** `ShadLibrary.WaitForPath(Instance, MaxWaitTime, Path)`

**Returns:** The Instance(s) you're looking for, or false if you exceed the maximum wait time and found nothing.
| Variable | Type | Default | Description |
| --- | --- | --- | --- |
| Instance | Instance | REQUIRED | Where the search will start. |
| MaxWaitTime | number | REQUIRED | The maximum wait time. |
| Path | string | REQUIRED | The path you're looking down. |

**Note:**
If before the name of the variable an ```*asterisk``` is placed anywhere in the sequence, that found instance will also be returned.

### Usage Example

```lua
--Let's assume that this is a LocalScript for some UI.
local UI = script.Parent
local Button = ShadLibrary.WaitForPath(UI, 20, "MainFrame.Something.SomethingElse.Button1")
--Will return "Button1" that would be down the path MainFrame.Something.SomethingElse

--If we also wanted to save MainFrame but don't want to repeat it:
local MainFrame, Button = ShadLibrary.WaitForPath(UI, 20, "*MainFrame.Something.SomethingElse.Button1")
--The asterisk before the name tells the function to also save that instance.
```

___
</details>

<details><summary>WaitForChildren</summary>

**Aliases:** WFC

**Description:** Allows you to call :WaitForChild() on multiple Instances under the same parent at the same time.

**Setup:** `ShadLibrary.WaitForChildren(Instance, MaxWait, Items...)`

**Returns:** The Instance(s) you're looking for. Will return false for each Instance that fails to be found within the time you set.
| Variable | Type | Default | Description |
| --- | --- | --- | --- |
| Instance | Instance | REQUIRED | Where to look. |
| MaxWait | number | REQUIRED | The maximum time allowed to wait on an Instance. |
| Items | string | REQUIRED | The items you would like to search for. |

**Note:**
If before the name of the variable an ```*asterisk``` is placed anywhere in the sequence, that found instance will also be returned.

### Usage Example

```lua
----Example 1
--Let's assume that this is a LocalScript for some UI.
local UI = script.Parent
local Frame1, Frame2, Button = ShadLibrary.WaitForChildren(UI, 10, "Frame1", "Frame2", "Button")
    --Will return the Instances that are within the UI, or false for each Instance
    --that cannot be found within 10 seconds.

----Example 2
local TextureA, TextureB = ShadLibrary.WFC(workspace, 10, "Brick1.Decal", "Brick2.Texture")
    --Will return the decal/texture found, or false if not there.

----Example 3
local Brick1, TextureA, TextureB = ShadLibrary.WFC(workspace, 10, "*Brick1.Decal", "Brick2.Texture")
    --Will return the first brick, then the decal/texture found, or false if not there.
```

___
</details>

<details><summary>WeldTo</summary>

**Description:** Automatically welds (via WeldConstraints) a lot of parts to a singular part.

**Setup:** `ShadLibrary.WeldTo(Main, WeldParent, UnanchorOthers, BaseParts...)`

**Returns:** `nil`
| Variable | Type | Default | Description |
| --- | --- | --- | --- |
| Main | BasePart | REQUIRED | The part that everything else will weld to. |
| WeldParent | Instance / true | REQUIRED | Where all the created WeldConstraints will go. |
| UnanchorOthers | boolean | REQUIRED | Determines if the attached part gets unanchored after making the weld. |
| BaseParts... | BasePart / {BasePart} | REQUIRED | A list of BaseParts you want welded. |

### WeldParent Usage

• **true:** This will default to the Main variable.

• **Instance:** The Instance where all of the WeldConstraints will be stored, if not `true`.

### Usage Example

```lua
----Example 1
local Part1, Part2, Part3, Part4 = workspace.Part1, workspace.Part2, workspace.Part3, workspace.Part4 

ShadLibrary.WeldTo(Part1, Part1, false, Part2, Part3, Part4)
    --This will weld Parts 2-4 to Part1. Welds will be parented inside of Part1. Parts 2-4 will not be forcefully unanchored.

----Example 2
local Part1, Part2, Part3, Part4 = workspace.Part1, workspace.Part2, workspace.Part3, workspace.Part4 

ShadLibrary.WeldTo(Part1, workspace, true, Part2, {Part3, Part4})
    --This will weld Parts 2-4 to Part1. Welds will be parented inside of the Workspace. Parts 2-4 will be forcefully unanchored.
```

___
</details>

<details><summary>PositiveNegative</summary>

**Aliases:** PN

**Description:** Returns a 50/50 chance for a number being positive or negative.

**Setup:** `ShadLibrary.PN(Number)`

**Returns:** The number generated, or ±1 if no number was set.
| Variable | Type | Default | Description |
| --- | --- | --- | --- |
| Number | number | 1 | The number that's being randomized. |

### Usage Example

```lua
--Example 1:
local Number = ShadLibrary.PN(5)   --Returns either 5 or -5

--Example 2:
local Number = ShadLibrary.PN()   --Returns either 1 or -1
```

___
</details>

<details><summary>Random</summary>

**Aliases:** Rando

**Description:** Selects at random whatever you put in the list.

**Setup:** `ShadLibrary.Rando(Items...)`

**Returns:** One of the items you put in the list at random.
| Variable | Type | Default | Description |
| --- | --- | --- | --- |
| Items | Any | REQUIRED | The items you wish to input. Can be anything, really. |

### Usage Example

```lua
--Example 1:
local Number = ShadLibrary.Rando(1, 10, 30, -6, 1000)

--Example 2:
local Chosen = ShadLibrary.Rando("Hello, world!", 96, workspace.Brick)
```

___
</details>

<details><summary>Service</summary>

**Description:** Fetches one or more services.

**Setup:** `ShadLibrary.Service(Service...)`

**Returns:** The service(s) that you requested.
| Variable | Type | Default | Description |
| --- | --- | --- | --- |
| Service | string | REQUIRED | The service(s) in which you'd like to fetch. |

### Usage Example

```lua
--Example 1:
local TweenService = ShadLibrary.Service'TweenService'

--Example 2:
local TweenService, RunService, ServerScriptService = ShadLibrary.Service("TweenService", "RunService", "ServerScriptService")
```

___
</details>

<details><summary>Tween</summary>

**Description:** A simplified method of making a tween.

**Setup 1:** `ShadLibrary.Tween(Instance, TweenInfo){Properties}`

**Setup 2:** `ShadLibrary.Tween(Instance, Time, Style, Direction, Repeat, Reverses, Delay){Properties}`

**Returns:** The Tween you've created.
| Variable | Type | Default | Description |
| --- | --- | --- | --- |
| Instance | Instance | REQUIRED | The Instance in which you wish to tween. |
| TweenInfo¹ | TweenInfo | Below Defaults | The TweenInfo you wish to use. |
| Time² | number | 1 | How long it takes the tween to complete. |
| Style | Enum / number / string | Linear | The EasingStyle of the tween. |
| Direction | Enum / number / string | InOut | The EasingDirection of the tween. |
| Repeat | number | 0 | How many times the tween will repeat. |
| Reverses | boolean | false | Determines if the tween will do the inverse after finishing. |
| Delay | number | 0 | The amount of time that elapses before tween starts in seconds. |
| | | | |
| Properties | table | {} | The properties that are being changed by the tween. Generally numerical. |

This function hosts some special inputs<sub>*Not all may apply*</sub>. Make sure to check the [On Changing Values](#on-changing-values) section for details on how to use them.

### Usage Example

```lua
--Example 1:
local MyInfo = TweenInfo.new(1, Enum.EasingStyle.Bounce, Enum.EasingDirection.Out, 1, true, 0.5)
local Tween = ShadLibrary.Tween(script.Parent, TweenInfo}{Position = script.Parent.Position + Vector3.new(0, 3, 0)}
Tween:Play()

--Example 2:
local Tween = ShadLibrary.Tween(script.Parent, 1, "Bounce", "Out", 1, true, 0.5}{Position = script.Parent.Position + Vector3.new(0, 3, 0)}
Tween:Play()
```

___
</details>

<details><summary>TweenLink</summary>

**Description:** A method for linking multiple tweens together.
Has limited usage compared to normal tweens. You may do the following: Play, Pause, Cancel, and Destroy. Has no readable properties or events to listen to (maybe will come later?).

**Setup:** `ShadLibrary.TweenLink(Tweens...)`

**Returns:** MetaTable with the functions **Play**, **Pause**, **Cancel** and **Destroy**.
| Variable | Type | Default | Description |
| --- | --- | --- | --- |
| Tweens | Tween | REQUIRED | The Tweens in which you wish to link together. |

### Usage Example

```lua
--Example
local Tween1 = ShadLibrary.Tween(workspace.Brick1, TweenInfo.new(1)){Transparency = 1}
local Tween2 = ShadLibrary.Tween(workspace.Brick2, TweenInfo.new(1.5)){Transparency = 0.5}

local TweenLink = ShadLibrary.TweenLink(Tween1, Tween2)

TweenLink:Play()
--The tweens will play at the same time but do not end at the same time.

```
___
</details>

<details><summary>TweenGroup</summary>

**Description:** A method for making multiple tweens of the same or similar items.

**Setup 1:** `ShadLibrary.TweenGroup(Instances...)(TweenInfo){Properties}`

**Setup 2:** `ShadLibrary.TweenGroup(Instances...)(Time, Style, Direction, Repeat, Reverses, Delay){Properties}`

**Returns:** Essentially makes a **TweenLink**.
| Variable | Type | Default | Description |
| --- | --- | --- | --- |
| Instances | Instance | REQUIRED | The Instances in which you wish to tween. |
| | | | |
| TweenInfo¹ | TweenInfo | Below Defaults | The TweenInfo you wish to use. |
| Time² | number | 1 | How long it takes the tween to complete. |
| Style | Enum / number / string | Linear | The EasingStyle of the tween. |
| Direction | Enum / number / string | InOut | The EasingDirection of the tween. |
| Repeat | number | 0 | Amount of times the tween will repeat. |
| Reverses | boolean | false | Determines if the tween will do the inverse after finishing. |
| Delay | number | 0 | The amount of time that elapses before tween starts in seconds. |
| | | | |
| Properties | table | {} | The properties that are being changed by the tween. Generally numerical. |

This function hosts some special inputs<sub>*Not all may apply*</sub>. Make sure to check the [On Changing Values](#on-changing-values) section for details on how to use them.

### Usage Example

```lua
--Example 1
local MyInfo = TweenInfo.new(1, Enum.EasingStyle.Bounce, Enum.EasingDirection.Out, 1, true, 0.5)
local Part1, Part2 = workspace.Part1, workspace.Part2

local TweenGroup = ShadLibrary.TweenGroup(Part1, Part2)(MyInfo){Position = Vector3.new(0, 5, 0)}
TweenGroup:Play()

--Example 2
local Part1, Part2 = workspace.Part1, workspace.Part2
local TweenGroup = ShadLibrary.TweenGroup(Part1, Part2)(1, "Bounce", "Out", 1, true, 0.5){Position = Vector3.new(0, 5, 0)}
TweenGroup:Play()
```
___
</details>

<details><summary>TweenSequence</summary>

**Description:** Exclusively allows the ability to tween ColorSequences and NumberSequences.

**Setup 1:** `ShadLibrary.TweenSequence(Instance, allowPointSliding, TweenInfo){Properties}`

**Setup 2:** `ShadLibrary.TweenSequence(Instance, allowPointSliding, Time, Style, Direction, Repeat, Reverses, Delay){Properties}`

**Returns:** A special tween-base made via metatables. Should be able to work just like a normal Tween with the same functions and variables. This includes a new function: **Tween:Destroy()** since this works in a specific way, if you want to clean up a bit, I recommend you use this when not needed anymore.
| Variable | Type | Default | Description |
| --- | --- | --- | --- |
| Instance | Instance | REQUIRED | The Instance in which you wish to tween. |
| allowPointSliding | boolean | REQUIRED | If true, will allow sequences that have the same number of keypoints to tween their times to match up, otherwise applies an overlapping fade effect. |
| TweenInfo¹ | TweenInfo | Below Defaults | The TweenInfo you wish to use. |
| Time² | number | 1 | How long it takes the tween to complete. |
| Style | Enum / number / string | Linear | The EasingStyle of the tween. |
| Direction | Enum / number / string | InOut | The EasingDirection of the tween. |
| Repeat | number | 0 | How many times the tween will repeat. |
| Reverses | boolean | false | Determines if the tween will do the inverse after finishing. |
| Delay | number | 0 | The amount of time that elapses before tween starts in seconds. |
| | | | |
| Properties | table | {} | The properties that are being changed by the tween. Restricted to NumberSequences and ColorSequences. |

### Usage Example

```lua
----Example 1
local beam = workspace.Part1.Beam
local newSequence = ColorSequence.new{
	ColorSequenceKeypoint.new(0, Color3.fromRGB(0, 0, 0)),
	ColorSequenceKeypoint.new(0.25, Color3.fromRGB(255, 0, 0)),
	ColorSequenceKeypoint.new(0.5, Color3.fromRGB(0, 255, 0)),
	ColorSequenceKeypoint.new(0.75, Color3.fromRGB(0, 0, 255)),
	ColorSequenceKeypoint.new(1, Color3.fromRGB(0, 0, 0))
}

local colorTween = ShadLibrary.TweenSequence(beam, false, 1, "Linear", "InOut", 2, true, 0.5){Color = newSequence}
colorTween:Play()
--Will tween any beam's colors to look a bit rainbow-like, revert, and repeats this a couple of times.

----Example 2
--In this example, let's assume the particle emitter's default size NumberSequence also contains 3 KeyPoints
local particleEmitter = workspace.Part5.ParticleEmitter
local newSequence = NumberSequence.new{
	NumberSequenceKeypoint.new(0, 1),
	NumberSequenceKeypoint.new(0.349, 3.39, 1.58),
	NumberSequenceKeypoint.new(1, 1)
}

local myTweenInfo = TweenInfo.new(1, Enum.EasingStyle.Linear, Enum.EasingDirection.InOut, 2, true, 0.5)

local sizeTween = ShadLibrary.TweenSequence(particleEmitter, myTweenInfo){Size = newSequence}
sizeTween:Play()
--Will tween the particle emitter's size, sliding the middle KeyPoint's time to match the new time
```

___
</details>

<details><summary>TabClone</summary>

**Description:** Clones a table.

**Setup:** `ShadLibrary.TabClone(Table)`

**Returns:** The new cloned table.
| Variable | Type | Default | Description |
| --- | --- | --- | --- |
| Table | table | REQUIRED | The table you want to clone. |

### Usage Example

```lua
local Tab1 = {1, 2, 3, "a", "b", "c"}
local Tab2 = ShadLibrary.TabClone(Tab1)
print(Tab1==Tab2) --false
```

___
</details>

<details><summary>MassConnect</summary>

**Description:** A quick method for connecting multiple events and instances simultaneously.

**Setup:** `ShadLibrary.MassConnect(Instances, Events, Function)`

**Returns:** An array of every new connection made.
| Variable | Type | Default | Description |
| --- | --- | --- | --- |
| Instances | table | REQUIRED | An array of Instances you'd like to connect. |
| Events | table | REQUIRED | An array of events of the Instances you'd like to have connected. |
| Function | Function | REQUIRED | The function you'd like to be connected to the event(s). |

**Note:**
Function will always use 2 variables at the start: The Instance connected, and a string of the Event connected. After that, every variable that would be ordinarily returned via the event.
Format it as so:

```lua
function TestFunction(Instance, Event, ...)
--Instance being the instance.
--Event is a string of the event you chose to connect.
local VarA, VarB, VarC, etc = ...
--or
local Variables = {...}
end
```

### Usage Example

```lua
--Let's assume this is a normal script, and Parts is a folder in the workspace that contains a few blocks.
function Reader(Part, Event, ...)
    if Event=="Changed" then
    print(Part.Name, "was changed! Changed:", ...)

    elseif Event=="Touched" then
    print(Part.Name, "was touched! Part that hit it:", ...)
    end
end

ShadLibrary.MassConnect(Parts:GetChildren(), {"Touched", "Changed"}, Reader)
```

___
</details>

<details><summary>Match</summary>

**Description:** A simple replacement for using multiple "or" statements on the same variable.

**Setup:** `ShadLibrary.Match(Main, Variables...)`

**Returns:** A boolean value; true if the Main variable matches any of the subsequent variables or false if not.
| Variable | Type | Default | Description |
| --- | --- | --- | --- |
| Main | Any | REQUIRED | The variable you're reading off of. |
| Variables | Any | REQUIRED | The variables you'd like to compare it to. |

### Usage Example

```lua
--Let's assume Part is the variable set to a part under the workspace.

if ShadLibrary.Match(Part.Transparency, 0, 0.5, 1)  then
--This part of the code runs if the part's transparency is either 0, 0.5, or 1.
else
--Otherwise...
end
```

___
</details>

<details><summary>AllMatch</summary>

**Description:** A simple replacement for checking if all variables must equal the same thing.

**Setup:** `ShadLibrary.AllMatch(MatchMe, Variables...)`

**Returns:** A boolean value; true if the following variables all match the MatchMe variable or false if not.
| Variable | Type | Default | Description |
| --- | --- | --- | --- |
| MatchMe | Any | REQUIRED | The variable you want the rest to match. |
| Variables | Any | REQUIRED | The variables you'd like to compare it to. |

### Usage Example

```lua
local Result = ShadLibrary.AllMatch(1, 2, 3, 1)
--Result would be false since they all need to equal the first variable (1).
local Result = ShadLibrary.AllMatch(1, 1, 1, 1)
--Result would be true.
```

___
</details>

<details><summary>WaitOn</summary>

**Description:** Can wait on multiple occasions, but will resume as soon as 1 of them is met.

**Setup:** `ShadLibrary.WaitOn(methodTable)`

**Returns:** The name of the method that finishes first.
| Variable | Type | Default | Description |
| --- | --- | --- | --- |
| methodTable | {[string]: Number / Signal / Function} | REQUIRED | The method of waiting you'd like to input |

**methodTable Variables**

• string: The name of the **method** that will be returned should the accompanying **method** complete first.

• method > Number: The number of seconds you'd like to wait. Same as **wait(Number)**.

• method > Signal: An event such as **Instance.Changed**.

• method > Function: Your own method of yielding via an annonymous function.

### Usage Example

```lua
--In this example this will yield for both 10 and 20 seconds at the same time.
ShadLibrary.WaitOn({waitTime1 = 10, waitTime2 = 20})
	--Will only return "waitTime1" since 10 seconds is faster than 20.

--Let's assume Part is the variable set to a part under workspace, and we want to wait till it gets changed at all, but we don't want to wait more than 10 seconds for that to happen.
ShadLibrary.WaitOn({waitTime = 10, didChange = Part.Changed})
	--Will return either "waitTime" should 10 seconds pass with nothing happening, or "didChange" should the Part be changed.

--However, if we want to wait on a specific property (Transparency in this case)...
ShadLibrary.WaitOn({waitTime = 10, transparencyChanged = Part:GetPropertyChangedSignal("Transparency")})
	--Same deal as the last example.

--We can create our own means of waiting with a function
ShadLibrary.WaitOn({waitTime = 10, myfunction = function() task.wait(5) end})
	--Anything may go into the function.
```

___
</details>

<details><summary>Make</summary>

**Description:** Quickly make multiple of the same thing.

**Setup:** `ShadLibrary.Make(Data, Amount, SameTable)`

**Returns:** The data you've copied.
| Variable | Type | Default | Description |
| --- | --- | --- | --- |
| Data | Any | REQUIRED | The number, string, vector3, etc that you're copying. |
| Amount | number | REQUIRED | How many times it will copy. |
| SameTable | boolean | false | Only applicable if the Data is a table. Determines if the returned tables are all the same connected one or just copies. |

### Usage Example

```lua
----Example 1 (any data):
local A, B, C = ShadLibrary.Make("Test", 3)
print(A, B, C) --Test, Test, Test

----Example 2a (connected tables):
local Tab = {"A", "b"}
local T1, T2 = ShadLibrary.Make(Tab, 2, true)
print(T1==T2) --true

----Example 2b (unconnected tables):
local Tab = {"A", "b"}
local T1, T2 = ShadLibrary.Make(Tab, 2)
print(T1==T2) --false
```

___
</details>

<details><summary>TimeTable</summary>

**Description:** Takes any number of seconds and returns a dictionary of all possible conversions from the formula **TimeConvert**.

**Setup:** `ShadLibrary.TimeTable(Seconds)`

**Returns:** A dictionary of time conversions.
| Variable | Type | Default | Description |
| --- | --- | --- | --- |
| Seconds | number | REQUIRED | How many seconds you want to convert. |

### Usage Example

```lua
--Converts 65 seconds into a table that returns 1 minute and 5 seconds.
local Times = ShadLibrary.TimeTable(65)
print(Times)
--[[Output: {
    ["Days"] = 0,
    ["Hours"] = 0,
    ["Milliseconds"] = 0,
    ["Minutes"] = 1,
    ["Seconds"] = 5,
    ["Weeks"] = 0
    }]]
```

___
</details>

<details><summary>TimeFormat</summary>

**Description:** Takes any number of seconds and converts that to a time format of your choosing.

**Setup:** `ShadLibrary.TimeFormat(Seconds, Format, Simple)`

**Returns:** A string containing the newly formatted time.
| Variable | Type | Default | Description |
| --- | --- | --- | --- |
| Seconds | number | REQUIRED | How many seconds you want to convert and format. |
| Format | string | REQUIRED | How the string should be presented. See below category for more details. |
| Simple | boolean | true | If the conversion can be used without any words in the string. |

### Format Guides
The string for the **Format** variable looks for certain patterns to determine where to place the converted time units. This goes to Milliseconds, Seconds, Minutes, Hours (24 hour, not 12 hour time), Days, and Weeks. Months and onward are excluded for being too variable; not every month has a set amount of days or weeks. When using these patterns, keep in mind that these are case sensitive:

| Pattern | Time Unit | Number Range |
| --- | --- | --- |
| Mi | Milliseconds | 0 - 9999 |
| S | Seconds | 0 - 59 |
| M | Minutes | 0 - 59 |
| H | Hours | 0 - 23 |
| D | Days | 0 - 6 |
| W | Weeks | No Limit |

Units such as Seconds or Hours do not display their limits of 60 or 24 since at that number they will convert to the next unit of time. 60 seconds to +1 minute.

Note that when the **Simple** variable is set to *false*, you will instead need to use every pattern with an underscore before it. So **Mi** will now be **_Mi**. See Usage Examples to see why this can be useful.

### Usage Example

```lua
--Example 1: Simple timer conversion for 65 seconds.
local Time = ShadLibrary.TimeFormat(65, "MM:SS.MiMiMi", true)
print(Time)
    --Expected output: "01:05.000"

--Example 2: Takes Example 1 but removes the excess zero from the minutes, and removes the milliseconds.
local Time = ShadLibrary.TimeFormat(65, "M:SS")
print(Time)
    --Expected output: "1:05"
    --Note that the third variable is true by default, so it is unnecessary to include here.

--Example 3: Displays 3 Hours  1 Minute  20.5 Seconds
local Time = ShadLibrary.TimeFormat(60^2 * 3 + 80.5, "HH:MM:SS.MiMiMi")
print(Time)
    --Expected output: "03:01:20.500"
 
 --Example 4: How to utilize the third variable to be false to display more accurate information. This will display 6 days  1 Minute  20 Seconds
local Time = ShadLibrary.TimeFormat(60^2 * 24 * 6 + 80, "_D Days _HH:_MM:_SS._MiMiMi", false)
print(Time)
    --Expected output: "6 Days 00:01:20.000"
    --Had we not set the Simple variable to false, this function would have attempted to convert the word Days to a number for the number of Days as well since it starts with a D.
```

___
</details>

<details><summary>Plugin_Settings</summary>

**Description:** Fetches plugin settings. For Plugins only.

**Setup:** `ShadLibrary.Plugin_Settings(Plugin, Setting...)`

**Returns:** The setting(s) you wanted to fetch or their defaults if they didn't exist.
| Variable | Type | Default | Description |
| --- | --- | --- | --- |
| Plugin | plugin | REQUIRED | Simply pass the plugin variable through. |
| Setting | {Type, Default} | REQUIRED | The setting(s) that you're trying to fetch. |

**Note:**

The Setting table must be set up as:

• **Type:** The name you want to set/fetch the setting from in string form.

• **Default:** If the setting could not be found, this is what it will be defaulted to instead.

### Usage Example

```lua
local SettingA, SettingB = ShadLibrary.Plugin_Settings(plugin, {"Color", Color3.new(1, 1, 1)}, {"Word", "Hello!"})
-- If either SettingA or B did not exist, the setting for their 1st value in the table, it would be set to the 2nd value in the pair.
```

___
</details>

<details><summary>Plugin_Widget</summary>

**Description:** A simplified method of making a plugin widget. For Plugins only.

**Setup 1:** `ShadLibrary.Plugin_Widget(Plugin, Identifier, DisplayName, DockWidgetPluginGuiInfo)`

**Setup 2:** `ShadLibrary.Plugin_Widget(Plugin, Identifier, DisplayName, InitialDockState, InitialEnabled, RestoreOverride, SizeX, SizeY, SizeMinimumX, SizeMinimumY)`

**Returns:** The widget you've created.
| Variable | Type | Default | Description |
| --- | --- | --- | --- |
| Plugin | plugin | REQUIRED | Simply pass the plugin variable through. |
| Identifier | string | "UnknownWidget" | The plugin widget's ID. |
| DisplayName | string | "UnknownWidget" | The name shown on the widget window. |
| DockWidgetPluginGuiInfo¹ | DockWidgetPluginGuiInfo | Below Defaults | The DockWidgetPluginGuiInfo you wish to use. |
| InitialDockState² | Enum / string / number | Float | Initial dock state. |
| InitialEnabled | boolean | true | If the widget is visible upon being made. |
| RestoreOverride | boolean | false | If true, the value of **InitialEnabled** will override the previously saved enabled state. |
| SizeX | number | 200 | Initial width of the widget window. |
| SizeY | number | 300 | Initial height of the widget window. |
| SizeMinimumX | number | 150 | Minimum width of the widget window. |
| SizeMinimumY | number | 150 | Minimum heightof the widget window. |

### Usage Example

```lua
--Example 1
local MyInfo = DockWidgetPluginGuiInfo.new(Enum.InitialDockState.Float, true, false, 200, 300, 150, 150)
local Widget = ShadLibrary.Plugin_Widget(plugin, "TestWidget", "Test Widget", MyInfo)
-- Creating a basic test widget with basically the default values in place.

--Example 2
local Widget = ShadLibrary.Plugin_Widget(plugin, "TestWidget", "Test Widget", "Float", true, false, 200, 300, 150, 150)
-- Creating a basic test widget with basically the default values in place.
```

___
</details>

<details><summary>GetAttributes</summary>

**Aliases:** GetAtt, GetAtts

**Description:** Gets multiple specific attributes at once (in order).

**Setup 1:** `ShadLibrary.GetAttributes(Instance, AutoMake, Attribute...)`

**Setup 2:** `ShadLibrary.GetAttributes(Instance, Attribute...)`

**Returns:** The attribute's value or **nil** if none exists.
| Variable | Type | Default | Description |
| --- | --- | --- | --- |
| Instance | Instance | REQUIRED | The Instance that you'd like to find the attribute of. |
| AutoMake¹ | boolean | true | If Attribute is a table input and if the attribute is missing, automatically create one and return its value. |
| Attribute | string / table | nil | The attribute(s) you want to fetch. |

Attribute's table is meant to be set up as follows: `{Name, Preset}`

• Name: The name of the attribute.

• Preset: The default value of the attribute if it doesn't yet exist.

If you're using the table variant and if there is a found attribute that does not match the type/typeof the preset, then the preset will be used instead.

### Usage Example

```lua
----Example 1
local Part = workspace.Brick

--Lets say that Part has the following attributes:
--TestNumber (5), TestString ("String!"), TestBoolean (true)

local A, B, C, D = ShadLibrary.GetAttributes(Part, "TestNumber", "TestString", "TestBrick", "TestBoolean")
--A would be 5
--B would be "String!"
--C would be nil since there is no match
--D would be true

----Example 2
local Part = workspace.Brick
local A, B = ShadLibrary.GetAttributes(Part, {"TestNumber", 5}, "TestString")
--A would be 5 if there wasn't one already set
--B would be false assuming one wasn't there

----Example 3
local Part = workspace.Brick
local A, B = ShadLibrary.GetAttributes(Part, false, {"TestNumber", 5}, "TestString")
--A would be nil if it doesn't already exist since the AutoMake variable was set to false
--B would be nil assuming one wasn't there
```

___
</details>

<details><summary>ClearAttributes</summary>

**Aliases:** ClearAtt, ClearAtts, NoAtt

**Description:** Clears multiple attributes at once.

**Setup:** `ShadLibrary.ClearAttributes(Instance, Attribute...)`

**Returns:** Nothing.
| Variable | Type | Default | Description |
| --- | --- | --- | --- |
| Instance | Instance | REQUIRED | The Instance that you'd like to clean up. |
| Attribute | string / nil | nil |  |

### Usage Example

```lua
--Clearing specific ones:
local Part = workspace.Brick
ShadLibrary.ClearAttributes(Part, "TestString", "TestBoolean")

--Clearing all:
local Part = workspace.Brick
ShadLibrary.ClearAttributes(Part)
```
</details>

#

</details>

<details><summary><h3>Special Inputs List</h3></summary>

This list is specifically for the functions where changing the properties of an Instance is possible.

### On Changing Values

| Name | Type | Effects | Description | Example |
| --- | --- | --- | --- | --- |
| Sides | Property Name | Surfaces | Changes all of the sides for BaseParts at the same time. | {Sides = "Smooth"} |
| "nil" | Value | ObjectValues | Allows you to set a property to `nil` where applicable. | {Adornee = "nil"} |
| "+#" | Value | Numbers | Adds the number **#** to the original value being changed. | {Value = "+0.5"} |
| "-#" | Value | Numbers | Subtracts the number **#** to the original value being changed. | {Value = "-0.5"} |
| "*#" | Value | Numbers | Multiplies the number **#** with the original value being changed. | {Value = "*2"} |
| "/#" | Value | Numbers | Divides the number **#** from the original value being changed. | {Value = "/2"} |
| "^#" | Value | Numbers | Sets the original value to the power of number **#**. | {Value = "^2"} |
| "sqrt" | Value | Numbers | Returns the square root of the number being changed. | {Value = "sqrt"} |
| "negate" | Value | Numbers | Basically inverts the number from negative/positive to the other. | {Value = "negate"} |
| "not" | Value | Booleans | Returns the opposite boolean value. | {Value = "not"} |
| Function | Value | Any | Inputs a function to return a value of your choosing. The first/only value of the function is always the Instance being changed. | {Value = function(instance: Instance) end} |
| "~X, Y, Z" | Value | Vector3 | Changes the Vector3 relative to its current value. The **X**, **Y**, and **Z** variables are the X, Y, and Z of the Vector3 Value. No spaces should be present here. | {Position = "~0, 5, 0"} |
| **"~#, #, #"** | Value | CFrame | Translates as ToWorldSpace(CFrame). | {CFrame = "~0, 0, -5"} |
| **"@#, #, #"** | Value | CFrame | Translates as CFrame*CFrame.fromEulerAnglesXYZ(#, #, #). Automatically converted to math.rad(#). | {CFrame = "@90, 0, 45"} |
| **"<#, #, #, #, #, #"** | Value | CFrame | Basically acts as the **~** and then applies the **@** changes. First three #'s affect the movement and the other three affect the rotation. | {CFrame = "<0, 5, 0, 0, 45, 0"} |
| **">#, #, #, #, #, #"** | Value | CFrame | Basically acts as the **@** and then applies the **~** changes. First three #'s affect the movement and the other three affect the rotation. | {CFrame = ">0, 5, 0, 0, 45, 0"} |

### On Searching Values

| Name | Type | Effects | Description | Example |
| --- | --- | --- | --- | --- |
| ">#" | Value | Numbers | Detects anything greater than the **#** in its place. | {Transparency = ">0.5"} |
| ">=#" | Value | Numbers | Detects anything greater or equal to the **#** in its place. | {Transparency = ">=0.5"} |
| "<#" | Value | Numbers | Detects anything lower than the **#** in its place. | {Transparency = "<0.5"} |
| "<=#" | Value | Numbers | Detects anything lower or equal to the **#** in its place. | {Transparency = "<=0.5"} |
| ...String | Value | Strings | Checks if the desired String is at the end of the value. | {Name = "...Brick"} |
| String... | Value | Strings | Checks if the desired String is at the start of the value.  | {Name = "Brick..."}  |
| ...String... | Value | Strings | Checks if the desired String is anywhere inside of the value.  | {Name = "...Brick..."}  |
| "R" | Value | Color3 | Checks if the **R** value is greater than **B** and **G** . | {Color = "R"} |
| "G" | Value | Color3 | Checks if the **G** value is greater than **R** and **B** . | {Color = "G"} |
| "B" | Value | Color3 | Checks if the **B** value is greater than **R** and **G** . | {Color = "B"} |
| "RG" | Value | Color3 | Checks if the color is more **yellow** than **blue** . | {Color = "RG"} |
| "GB" | Value | Color3 | Checks if the color is more **cyan** than **red** . | {Color = "GB"} |
| "RB" | Value | Color3 | Checks if the color is more **purple** than **green** . | {Color = "RB"} |
| "...Name" | Value | BrickColor | Checks if the name of the **BrickColor** ends with your input. | {BrickColor = "...red"} |
| {Value, Value... etc} | Value | Any | You can now set each property to equal a table of values. Basically: If property equals this, or this, or this… etc. | {Transparency = {0, 0.5, 1}} |
</details>

# Velocity Executor Documentation

> Complete API reference for the Velocity Executor.

---

# Table of Contents

- Environment
- File System
- File Dialogs
- Console
- Debug Library
- Closure Library
- Instance Library
- Script Library
- Metatable Library
- Signal Library
- Actor Library
- Garbage Collection
- Cryptography
- Bit Library
- HTTP Library
- Input Library
- Drawing Library
- Cache Library

---

# Environment

The Environment library provides functions for interacting with the executor environment, thread identity, globals, Roblox FastFlags and teleport queue.

---

## identifyexecutor

Returns the executor name and version.

### Syntax

```lua
identifyexecutor() -> string, string
```

### Aliases

- `getexecutorname`

### Returns

| Type | Description |
|------|-------------|
| string | Executor name |
| string | Executor version |

### Example

```lua
local name, version = identifyexecutor()

print(name, version)

-- Velocity Velocity
```

---

## gethwid

Returns the machine HWID used for license binding.

### Syntax

```lua
gethwid() -> string
```

### Returns

| Type | Description |
|------|-------------|
| string | Hardware Identifier |

### Example

```lua
print(gethwid())
```

---

## getidentity

Returns the current thread identity.

### Syntax

```lua
getidentity() -> number
```

### Aliases

- `getthreadidentity`
- `getthreadcontext`
- `get_thread_identity`
- `get_thread_context`

### Returns

| Type | Description |
|------|-------------|
| number | Thread identity (1-8) |

### Example

```lua
print(getidentity())

-- 8
```

---

## setidentity

Sets the current thread identity.

### Syntax

```lua
setidentity(level: number)
```

### Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| level | number | Identity level |

### Aliases

- `setthreadidentity`
- `setthreadcontext`
- `set_thread_identity`
- `set_thread_context`

### Example

```lua
setidentity(2)

-- Testing...

setidentity(8)
```

---

## getfflag

Reads a Roblox FastFlag.

### Syntax

```lua
getfflag(name: string) -> any
```

### Parameters

| Parameter | Type |
|-----------|------|
| name | string |

### Returns

Any value stored in the FastFlag.

### Example

```lua
print(getfflag("DebugDisableTelemetry"))
```

---

## setfflag

Sets a Roblox FastFlag.

### Syntax

```lua
setfflag(name: string, value: any)
```

### Parameters

| Parameter | Type |
|-----------|------|
| name | string |
| value | any |

### Example

```lua
setfflag(
    "DebugGraphicsPreferD3D11",
    "True"
)
```

---

## checkcaller

Returns whether the current thread belongs to the executor.

### Syntax

```lua
checkcaller() -> boolean
```

### Returns

| Type |
|------|
| boolean |

### Example

```lua
if checkcaller() then
    print("Running inside Velocity")
end
```

---

## cloneref

Creates a new userdata pointing to the same Instance.

### Syntax

```lua
cloneref(instance: Instance) -> Instance
```

### Aliases

- `clonereference`

### Parameters

| Parameter | Type |
|-----------|------|
| instance | Instance |

### Returns

A cloned userdata referencing the same Instance.

### Example

```lua
local player = game.Players.LocalPlayer
local cloned = cloneref(player)

print(player == cloned)
-- false

print(compareinstances(player, cloned))
-- true
```

---

## getgenv

Returns the executor global environment.

### Syntax

```lua
getgenv() -> table
```

### Returns

Executor global table.

### Example

```lua
getgenv().SharedFlag = true
```

---

## getreg

Returns the Lua registry.

### Syntax

```lua
getreg() -> table
```

### Returns

Lua registry table.

### Example

```lua
for k,v in pairs(getreg()) do
    print(k, typeof(v))
end
```

---

## getrenv

Returns Roblox's global environment.

### Syntax

```lua
getrenv() -> table
```

### Returns

Roblox environment table.

### Example

```lua
local env = getrenv()

print(env.game == game)
```

---

## gettenv

Returns the environment of a thread.

### Syntax

```lua
gettenv(thread: thread) -> table
```

### Parameters

| Parameter | Type |
|-----------|------|
| thread | thread |

### Example

```lua
local co = coroutine.create(function() end)

local env = gettenv(co)

print(env.print)
```

---

## loadstring

Compiles Luau source code.

### Syntax

```lua
loadstring(source: string, chunkname: string?) -> function | nil, string?
```

### Aliases

- `executescript`

### Example

```lua
local f, err = loadstring("return 1 + 2")

print(f())
```

---

## getscriptfromthread

Returns the script that created a thread.

### Syntax

```lua
getscriptfromthread(thread: thread) -> Instance?
```

### Example

```lua
local script = getscriptfromthread(coroutine.running())

print(script)
```

---

## getobjects

Loads Roblox assets.

### Syntax

```lua
getobjects(assetId: string | number) -> {Instance}
```

### Example

```lua
local objects = getobjects("rbxassetid://1818")

for _,obj in ipairs(objects) do
    print(obj.Name)
end
```

---

## firetouchinterest

Simulates a touch event.

### Syntax

```lua
firetouchinterest(
    part: BasePart,
    humanoidPart: BasePart,
    action: number
)
```

### Parameters

| Name | Description |
|------|-------------|
| action | 0 = Touch |
| action | 1 = Untouch |

### Example

```lua
local hrp = game.Players.LocalPlayer.Character.HumanoidRootPart

firetouchinterest(hrp, workspace.KillBrick, 0)
firetouchinterest(hrp, workspace.KillBrick, 1)
```

---

## queue_on_teleport

Queues Lua code to execute after teleport.

### Syntax

```lua
queue_on_teleport(source: string)
```

### Aliases

- `queueonteleport`

### Example

```lua
queue_on_teleport([[
print("Teleported!")

loadstring(game:HttpGet("https://example.com"))()
]])
```

---

## clearqueueonteleport

Clears the teleport queue.

### Syntax

```lua
clearqueueonteleport()
```

### Aliases

- `clear_teleport_queue`
- `clearteleportqueue`

### Example

```lua
clearqueueonteleport()
```

---

## _G

Fresh executor global table.

### Example

```lua
_G.Counter = (_G.Counter or 0) + 1

print(_G.Counter)
```

---

## shared

Fresh shared global table.

### Example

```lua
shared.Value = true

print(shared.Value)
```

# File System

The File System library provides functions for reading, writing and managing files and folders on the host machine.

---

## readfile

Reads the contents of a file.

### Syntax

```lua
readfile(path: string) -> string
```

### Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| path | string | File path |

### Returns

| Type | Description |
|------|-------------|
| string | File contents |

### Example

```lua
local text = readfile("notes.txt")

print(text)
```

---

## writefile

Creates or overwrites a file.

### Syntax

```lua
writefile(path: string, contents: string)
```

### Parameters

| Parameter | Type |
|-----------|------|
| path | string |
| contents | string |

### Example

```lua
writefile(
    "save.json",
    game:GetService("HttpService"):JSONEncode({
        hp = 100
    })
)
```

---

## appendfile

Appends data to an existing file.

### Syntax

```lua
appendfile(path: string, contents: string)
```

### Example

```lua
appendfile(
    "log.txt",
    os.date() .. " - Event\n"
)
```

---

## isfile

Returns whether a file exists.

### Syntax

```lua
isfile(path: string) -> boolean
```

### Returns

| Type |
|------|
| boolean |

### Example

```lua
if isfile("save.json") then
    print("Save exists.")
end
```

---

## isfolder

Returns whether a directory exists.

### Syntax

```lua
isfolder(path: string) -> boolean
```

### Example

```lua
if not isfolder("dumps") then
    makefolder("dumps")
end
```

---

## delfile

Deletes a file.

### Syntax

```lua
delfile(path: string)
```

### Example

```lua
delfile("temp.txt")
```

---

## delfolder

Deletes a directory and all of its contents.

### Syntax

```lua
delfolder(path: string)
```

### Example

```lua
delfolder("dumps/old")
```

---

## makefolder

Creates a directory.

### Syntax

```lua
makefolder(path: string)
```

### Example

```lua
makefolder("dumps/2026-07/players")
```

---

## listfiles

Lists every file and folder inside a directory.

### Syntax

```lua
listfiles(path: string) -> {string}
```

### Returns

Array of file paths.

### Example

```lua
for _, file in ipairs(listfiles("dumps")) do
    print(file)
end
```

---

## loadfile

Compiles a Lua file.

### Syntax

```lua
loadfile(path: string, chunkname: string?) -> function | nil, string?
```

### Returns

Compiled function or error.

### Example

```lua
local f, err = loadfile("scripts/main.lua")

if f then
    f()
else
    warn(err)
end
```

---

## getcustomasset

Creates a Roblox asset URL from a local file.

### Syntax

```lua
getcustomasset(path: string) -> string
```

### Returns

rbxasset:// URL.

### Example

```lua
local img = Instance.new("ImageLabel")

img.Image = getcustomasset("images/logo.png")
```

---

## setclipboard

Copies text to the operating system clipboard.

### Syntax

```lua
setclipboard(text: string)
```

### Aliases

- setrbxclipboard
- toclipboard

### Example

```lua
setclipboard(
    tostring(game.Players.LocalPlayer.UserId)
)
```

---

# File Dialogs

Functions for opening native Windows file and folder dialogs.

---

## openfiledialog

Opens a file picker.

### Syntax

```lua
openfiledialog(
    filter: string?,
    title: string?
) -> string?
```

### Returns

Selected file path.

### Example

```lua
local path = openfiledialog(
    "Lua Files\0*.lua\0All Files\0*.*\0",
    "Select Script"
)

print(path)
```

---

## openfilesdialog

Opens a multi-select file picker.

### Syntax

```lua
openfilesdialog(
    filter: string?,
    title: string?
) -> {string}?
```

### Returns

Array of file paths.

### Example

```lua
local files = openfilesdialog(
    nil,
    "Select Logs"
)

for _, file in ipairs(files or {}) do
    print(file)
end
```

---

## savefiledialog

Opens a Save File dialog.

### Syntax

```lua
savefiledialog(
    filter: string?,
    defaultName: string?,
    title: string?
) -> string?
```

### Example

```lua
local file = savefiledialog(
    "Text Files\0*.txt\0",
    "dump.txt",
    "Save Dump"
)

if file then
    writefile(file, "Hello")
end
```

---

## openfolderdialog

Opens a folder picker.

### Syntax

```lua
openfolderdialog(
    title: string?
) -> string?
```

### Example

```lua
local folder = openfolderdialog(
    "Choose Output Folder"
)

print(folder)
```

---

# Console

The Console library provides access to Velocity's native console window.

---

## rconsolecreate

Creates and displays the console window.

### Syntax

```lua
rconsolecreate()
```

### Aliases

- consolecreate
- rconsoleshow
- consoleshow

### Example

```lua
rconsolecreate()
```

---

## rconsoledestroy

Destroys the console window.

### Syntax

```lua
rconsoledestroy()
```

### Aliases

- consoledestroy
- rconsolehide
- consolehide

### Example

```lua
rconsoledestroy()
```

---

## rconsolesettitle

Sets the console title.

### Syntax

```lua
rconsolesettitle(title: string)
```

### Aliases

- rconsolename
- consolesettitle
- consolename

### Example

```lua
rconsolesettitle(
    "Velocity - Debug"
)
```

---

## rconsoleclear

Clears the console buffer.

### Syntax

```lua
rconsoleclear()
```

### Aliases

- consoleclear

### Example

```lua
rconsoleclear()
```

---

## rconsoleprint

Prints text without a newline.

### Syntax

```lua
rconsoleprint(text: string)
```

### Aliases

- consoleprint

### Example

```lua
rconsoleprint("Progress: ")

for i = 1,5 do
    rconsoleprint(i .. " ")
    task.wait(0.1)
end

rconsoleprint("\n")
```

---

## rconsoleinput

Reads one line of input.

### Syntax

```lua
rconsoleinput() -> string
```

### Aliases

- consoleinput

### Returns

User input.

### Example

```lua
rconsoleprint("Name > ")

local name = rconsoleinput()

print(name)
```

---

## rconsoleinfo

Prints an informational message.

### Syntax

```lua
rconsoleinfo(text: string)
```

### Aliases

- consoleinfo

### Example

```lua
rconsoleinfo(
    "Loaded 42 modules."
)
```

---

## rconsolewarn

Prints a warning message.

### Syntax

```lua
rconsolewarn(text: string)
```

### Aliases

- consolewarn

### Example

```lua
rconsolewarn(
    "Slow server response."
)
```

---

## rconsoleerr

Prints an error message.

### Syntax

```lua
rconsoleerr(text: string)
```

### Aliases

- rconsoleerror
- consoleerr
- consoleerror

### Example

```lua
rconsoleerr(
    "Connection failed."
)
```

---

## messagebox

Displays a native Windows MessageBox.

### Syntax

```lua
messagebox(
    text: string,
    title: string,
    buttons: number
) -> number
```

### Button Values

| Value | Meaning |
|------:|---------|
| 0 | OK |
| 1 | OK / Cancel |
| 4 | Yes / No |

### Returns

| Value | Meaning |
|------:|---------|
| 6 | Yes |

### Example

```lua
local result = messagebox(
    "Continue?",
    "Confirmation",
    4
)

if result == 6 then
    print("User selected Yes.")
end
```

---

# Debug Library

The Debug Library provides advanced introspection and manipulation of Luau functions, constants, upvalues, stack frames, registry values and metatables.

---

## debug.getconstant

Returns the constant at the specified index from a function's constant table.

### Syntax

```lua
debug.getconstant(f: function | number, idx: number) -> any
```

### Aliases

- `getconstant`

### Parameters

| Parameter | Type | Description |
|----------|------|-------------|
| f | function \| number | Target function or stack level |
| idx | number | Constant index |

### Returns

The constant stored at the specified index.

### Example

```lua
local function f()
    return "hello"
end

print(debug.getconstant(f, 1))
```

---

## debug.getconstants

Returns every constant inside a function.

### Syntax

```lua
debug.getconstants(f: function | number) -> {any}
```

### Aliases

- `getconstants`

### Returns

Array containing every constant.

### Example

```lua
for i, constant in ipairs(debug.getconstants(f)) do
    print(i, constant)
end
```

---

## debug.setconstant

Replaces a constant inside a function.

> **Note**
>
> The replacement value must be the same type as the original constant.

### Syntax

```lua
debug.setconstant(f, idx: number, value: any)
```

### Aliases

- `setconstant`

### Example

```lua
debug.setconstant(f, 1, "patched")

print(f())
```

---

## debug.getproto

Returns a child prototype from a function.

### Syntax

```lua
debug.getproto(
    f,
    idx: number,
    active: boolean?
) -> function | {function}
```

### Aliases

- `getproto`

### Parameters

| Parameter | Description |
|----------|-------------|
| idx | Child prototype index |
| active | Returns every live closure created from the prototype |

### Example

```lua
local inner = debug.getproto(
    outer,
    1
)

inner()
```

---

## debug.getprotos

Returns every child prototype inside a function.

### Syntax

```lua
debug.getprotos(f) -> {function}
```

### Aliases

- `getprotos`

### Example

```lua
for i, proto in ipairs(debug.getprotos(f)) do
    print(
        i,
        debug.getinfo(proto).name
    )
end
```

---

## debug.getupvalue

Returns an upvalue by index.

### Syntax

```lua
debug.getupvalue(
    f,
    idx: number
) -> any
```

### Aliases

- `getupvalue`

### Example

```lua
local x = 10

local function get()
    return x
end

print(
    debug.getupvalue(get,1)
)
```

---

## debug.getupvalues

Returns every upvalue used by a function.

### Syntax

```lua
debug.getupvalues(f) -> {any}
```

### Aliases

- `getupvalues`

### Example

```lua
for i,v in ipairs(
    debug.getupvalues(get)
) do
    print(i,v)
end
```

---

## debug.setupvalue

Replaces an upvalue.

### Syntax

```lua
debug.setupvalue(
    f,
    idx: number,
    value: any
)
```

### Aliases

- `setupvalue`

### Example

```lua
debug.setupvalue(
    get,
    1,
    999
)

print(get())
```

---

## debug.getstack

Returns one value or the entire stack frame.

### Syntax

```lua
debug.getstack(
    level: number,
    idx: number?
) -> any | {any}
```

### Aliases

- `getstack`

### Example

```lua
local function test()

    local a,b = 1,2

    print(
        debug.getstack(
            1,
            1
        )
    )

end

test()
```

---

## debug.setstack

Replaces a stack value.

### Syntax

```lua
debug.setstack(
    level: number,
    idx: number,
    value: any
)
```

### Aliases

- `setstack`

### Example

```lua
local function test()

    local x = 1

    debug.setstack(
        1,
        1,
        42
    )

    print(x)

end

test()
```

---

## debug.getinfo

Returns debugging information about a function or stack frame.

### Syntax

```lua
debug.getinfo(
    f | level: number,
    what: string?
) -> table
```

### Aliases

- `getinfo`

### Returned Information

- source
- short_src
- currentline
- linedefined
- lastlinedefined
- what
- name
- nups
- numparams
- isvararg

### Example

```lua
local info =
    debug.getinfo(1)

print(
    info.source,
    info.currentline
)
```

---

## debug.getregistry

Returns Lua's registry table.

### Syntax

```lua
debug.getregistry() -> table
```

### Aliases

- `getregistry`

### Example

```lua
local registry =
    debug.getregistry()

print(
    registry["_LOADED"]
)
```

---

## debug.getmetatable

Returns a metatable while ignoring the `__metatable` field.

### Syntax

```lua
debug.getmetatable(
    value: any
) -> table?
```

### Example

```lua
local mt =
    debug.getmetatable(game)

print(
    rawget(
        mt,
        "__namecall"
    )
)
```

---

## debug.setmetatable

Replaces a metatable while ignoring `__metatable`.

### Syntax

```lua
debug.setmetatable(
    value: any,
    mt: table
) -> any
```

### Example

```lua
debug.setmetatable(
    myUserdata,
    {
        __index = function()
            return 0
        end
    }
)
```

---

# Closure Library

The Closure Library provides functions for creating, cloning, inspecting and hooking Lua and C closures.

---

## newcclosure

Wraps a Lua function into a native C closure.

Functions wrapped with `newcclosure()` return `true` when checked with `iscclosure()`.

Recommended for hooks that should appear native and reduce anti-tamper detection.

### Syntax

```lua
newcclosure(f: function) -> function
```

### Parameters

| Parameter | Type | Description |
|----------|------|-------------|
| f | function | Function to wrap |

### Returns

| Type | Description |
|------|-------------|
| function | Wrapped C Closure |

### Example

```lua
local wrapped = newcclosure(function(x)
    return x * 2
end)

print(iscclosure(wrapped))
-- true

print(wrapped(21))
-- 42
```

---

## newlclosure

Creates a Lua closure.

### Syntax

```lua
newlclosure(f: function) -> function
```

### Parameters

| Parameter | Type |
|----------|------|
| f | function |

### Returns

Lua Closure.

### Example

```lua
local closure = newlclosure(print)

print(
    islclosure(closure)
)
```

---

## wrapclosure

Automatically wraps a function using the appropriate closure type.

### Syntax

```lua
wrapclosure(f: function) -> function
```

### Returns

Wrapped function.

### Example

```lua
local wrapped = wrapclosure(function()

    return "wrapped"

end)

print(wrapped())
```

---

## clonefunction

Creates an identical copy of a function while giving it a new identity.

The cloned function behaves identically but compares differently using `==`.

### Syntax

```lua
clonefunction(f: function) -> function
```

### Returns

Function clone.

### Example

```lua
local cloned = clonefunction(print)

print(print == cloned)
-- false

print(comparefunctions(
    print,
    cloned
))
-- true
```

---

## hookfunction

Hooks a function and redirects every call through another function.

Returns the original function.

### Syntax

```lua
hookfunction(
    target: function,
    hook: function
) -> function
```

### Aliases

- hookfunc
- replacefunction
- replaceclosure

### Returns

Original function.

### Example

```lua
local original

original = hookfunction(
    print,
    function(...)

        original(
            "HOOK:",
            ...
        )

    end
)

print("Hello")
```

---

## hookmetamethod

Hooks a metamethod from an object's metatable.

Typically used with:

- __namecall
- __index
- __newindex

### Syntax

```lua
hookmetamethod(
    object: any,
    method: string,
    hook: function
) -> function
```

### Returns

Original metamethod.

### Example

```lua
local old

old = hookmetamethod(
    game,
    "__namecall",

    function(self,...)

        if getnamecallmethod() == "Kick" then
            return
        end

        return old(self,...)

    end
)
```

---

## iscclosure

Returns whether a function is a native C closure.

### Syntax

```lua
iscclosure(f) -> boolean
```

### Returns

Boolean.

### Example

```lua
print(
    iscclosure(print)
)
```

---

## islclosure

Returns whether a function is a Lua closure.

### Syntax

```lua
islclosure(f) -> boolean
```

### Returns

Boolean.

### Example

```lua
print(
    islclosure(function() end)
)
```

---

## isnewcclosure

Returns whether a function was created using `newcclosure()`.

### Syntax

```lua
isnewcclosure(f) -> boolean
```

### Returns

Boolean.

### Example

```lua
local c = newcclosure(function()

end)

print(
    isnewcclosure(c)
)
```

---

## isexecutorclosure

Returns whether a closure belongs to Velocity.

### Syntax

```lua
isexecutorclosure(f) -> boolean
```

### Aliases

- checkclosure
- isourclosure
- iscustomcclosure
- istempleclosure
- is_synapse_function
- issentinelclosure
- is_sirhurt_closure
- is_protosmasher_closure

### Returns

Boolean.

### Example

```lua
print(
    isexecutorclosure(getgenv)
)

print(
    isexecutorclosure(print)
)
```

---

## comparefunctions

Checks whether two functions dispatch to the same underlying implementation.

Automatically unwraps cloned functions.

### Syntax

```lua
comparefunctions(
    a: function,
    b: function
) -> boolean
```

### Returns

Boolean.

### Example

```lua
print(
    comparefunctions(
        print,
        clonefunction(print)
    )
)
```

---

## getfunctionhash

Returns a stable hash representing a function's bytecode.

### Syntax

```lua
getfunctionhash(f: function) -> string
```

### Returns

Hash string.

### Example

```lua
print(
    getfunctionhash(
        function()

            return 1

        end
    )
)
```

---

## isfunctionhooked

Returns whether a function currently has an active hook.

### Syntax

```lua
isfunctionhooked(f: function) -> boolean
```

### Returns

Boolean.

### Example

```lua
hookfunction(
    print,
    function()

    end
)

print(
    isfunctionhooked(print)
)
```

---

## restorefunction

Removes every hook installed on a function.

### Syntax

```lua
restorefunction(f: function)
```

### Aliases

- restorefunc

### Example

```lua
restorefunction(print)

print(
    isfunctionhooked(print)
)
```

---

## getcallbackvalue

Returns the Lua callback currently assigned to a callback member.

Useful for objects such as:

- RemoteFunction.OnClientInvoke
- BindableFunction.OnInvoke

### Syntax

```lua
getcallbackvalue(
    object: any,
    memberName: string
) -> function?
```

### Aliases

- getcallbackmember

### Returns

Callback function.

### Example

```lua
local remote =
    game.ReplicatedStorage.MyRemoteFunction

local callback =
    getcallbackvalue(
        remote,
        "OnClientInvoke"
    )

if callback then

    print(
        callback()
    )

end
```

---

## getcallstack

Returns the complete Lua call stack.

Each frame contains:

- source
- func
- currentline
- name
- what

### Syntax

```lua
getcallstack() -> {table}
```

### Returns

Array of stack frames.

### Example

```lua
for i, frame in ipairs(
    getcallstack()
) do

    print(

        i,

        frame.source,

        frame.currentline

    )

end
```

---

# Instance Library

The Instance Library provides functions for discovering, comparing and accessing hidden Roblox instances.

---

## getinstances

Returns every Instance currently loaded in the DataModel, including instances that are normally inaccessible.

### Syntax

```lua
getinstances() -> {Instance}
```

### Returns

| Type | Description |
|------|-------------|
| {Instance} | Every loaded Instance |

### Example

```lua
local instances = getinstances()

print(#instances)
```

---

## getnilinstances

Returns every Instance whose parent is `nil`.

### Syntax

```lua
getnilinstances() -> {Instance}
```

### Returns

Array containing orphaned instances.

### Example

```lua
for _, object in ipairs(getnilinstances()) do
    print(
        object.ClassName,
        object.Name
    )
end
```

---

## gethui

Returns Velocity's hidden GUI container.

Unlike CoreGui, this container cannot normally be discovered by game scripts, making it suitable for internal executor interfaces.

### Syntax

```lua
gethui() -> Instance
```

### Aliases

- get_hidden_gui

### Returns

Hidden GUI container.

### Example

```lua
local gui = Instance.new("ScreenGui")

gui.Name = "Velocity UI"

gui.Parent = gethui()
```

---

## compareinstances

Determines whether two userdata reference the same underlying Roblox Instance.

Useful when using `cloneref()`.

### Syntax

```lua
compareinstances(
    a: Instance,
    b: Instance
) -> boolean
```

### Returns

Boolean.

### Example

```lua
local player = game.Players.LocalPlayer
local cloned = cloneref(player)

print(
    compareinstances(
        player,
        cloned
    )
)
```

---

# Script Library

The Script Library provides utilities for accessing, inspecting, decompiling and manipulating Roblox scripts.

---

## getscriptbytecode

Returns the raw compiled Luau bytecode from a Script, LocalScript or ModuleScript.

### Syntax

```lua
getscriptbytecode(
    script: Instance
) -> string
```

### Aliases

- dumpstring

### Returns

Compiled Luau bytecode.

### Example

```lua
local bytecode = getscriptbytecode(
    game.Players.LocalPlayer.PlayerScripts.Main
)

writefile(
    "Main.luac",
    bytecode
)
```

---

## getscripthash

Returns the SHA hash of a script's bytecode.

Returns `nil` if the bytecode cannot be accessed.

### Syntax

```lua
getscripthash(
    script: Instance
) -> string?
```

### Returns

SHA hash.

### Example

```lua
print(
    getscripthash(
        workspace.Script
    )
)
```

---

## getscriptclosure

Compiles and returns the script's top-level closure without executing it automatically.

### Syntax

```lua
getscriptclosure(
    script: Instance
) -> function
```

### Aliases

- getscriptfunction

### Returns

Compiled Lua function.

### Example

```lua
local closure = getscriptclosure(
    game.ReplicatedStorage.Module
)

local exports = closure()
```

---

## getcallingscript

Returns the script responsible for the currently executing thread.

### Syntax

```lua
getcallingscript() -> Instance?
```

### Returns

Calling script or `nil`.

### Example

```lua
local script = getcallingscript()

if script then
    print(script:GetFullName())
end
```

---

## getscripts

Returns every Script, LocalScript and ModuleScript inside the DataModel.

### Syntax

```lua
getscripts() -> {Instance}
```

### Returns

Array of scripts.

### Example

```lua
for _, script in ipairs(getscripts()) do

    if script.Name:match("Anti") then

        print(
            script:GetFullName()
        )

    end

end
```

---

## getloadedmodules

Returns every ModuleScript that has already been required.

### Syntax

```lua
getloadedmodules() -> {Instance}
```

### Returns

Loaded modules.

### Example

```lua
print(
    #getloadedmodules(),
    "modules loaded."
)
```

---

## getrunningscripts

Returns every Script whose main thread is currently alive.

### Syntax

```lua
getrunningscripts() -> {Instance}
```

### Returns

Running scripts.

### Example

```lua
for _, script in ipairs(
    getrunningscripts()
) do

    print(script.Name)

end
```

---

## getsenv

Returns the environment table of a Script or LocalScript.

### Syntax

```lua
getsenv(
    script: Instance
) -> table
```

### Returns

Environment table.

### Example

```lua
local env = getsenv(
    game.Players.LocalPlayer.PlayerScripts.Main
)

print(
    env.myVariable
)
```

---

## getmenv

Returns the environment table of a required ModuleScript.

### Syntax

```lua
getmenv(
    module: ModuleScript
) -> table
```

### Returns

Module environment.

### Example

```lua
local env = getmenv(
    game.ReplicatedStorage.Shared
)

print(env._VERSION)
```

---

## decompile

Attempts to decompile a script into readable Luau source.

### Syntax

```lua
decompile(
    script: Instance
) -> string
```

### Returns

Decompiled source.

### Example

```lua
print(
    decompile(
        workspace.Script
    )
)
```

---

## setscriptable

Changes a property's Scriptable flag.

Returns the property's previous Scriptable state.

### Syntax

```lua
setscriptable(
    instance: Instance,
    property: string,
    scriptable: boolean
) -> boolean
```

### Returns

Previous Scriptable value.

### Example

```lua
setscriptable(
    workspace.Camera,
    "Focus",
    true
)

print(workspace.Camera.Focus)
```

---

## isscriptable

Returns whether a property is currently Scriptable.

### Syntax

```lua
isscriptable(
    instance: Instance,
    property: string
) -> boolean
```

### Returns

Boolean.

### Example

```lua
print(
    isscriptable(
        workspace.Camera,
        "Focus"
    )
)
```

---

## gethiddenproperty

Reads a property regardless of its Scriptable state.

### Syntax

```lua
gethiddenproperty(
    instance: Instance,
    property: string
) -> any, boolean
```

### Returns

| Return | Description |
|---------|-------------|
| any | Property value |
| boolean | Whether the property is hidden |

### Example

```lua
local value, hidden =
    gethiddenproperty(
        workspace.Terrain,
        "PhysicsGrid"
    )

print(hidden)
```

---

## sethiddenproperty

Writes a property's value regardless of Scriptable restrictions.

Supports complex Roblox datatypes such as:

- CFrame
- Vector3
- ColorSequence
- BinaryString
- Enum
- Content
- NumberSequence

### Syntax

```lua
sethiddenproperty(
    instance: Instance,
    property: string,
    value: any
) -> boolean
```

### Returns

Boolean indicating success.

### Example

```lua
sethiddenproperty(
    workspace.Part,
    "Size",
    Vector3.new(
        10,
        10,
        10
    )
)
```

---

# Metatable Library

The Metatable Library provides functions for reading, modifying and interacting with Luau metatables, including protected metatables and namecall manipulation.

---

## getrawmetatable

Returns the raw metatable of a value, ignoring the `__metatable` protection.

### Syntax

```lua
getrawmetatable(value: any) -> table?
```

### Parameters

| Parameter | Type | Description |
|----------|------|-------------|
| value | any | Value to retrieve the metatable from |

### Returns

| Type | Description |
|------|-------------|
| table? | Raw metatable |

### Example

```lua
local mt = getrawmetatable(game)

print(mt.__namecall)
```

---

## setrawmetatable

Replaces a metatable while bypassing `__metatable` protection.

### Syntax

```lua
setrawmetatable(value: any, mt: table) -> any
```

### Parameters

| Parameter | Type |
|----------|------|
| value | any |
| mt | table |

### Returns

The modified value.

### Example

```lua
local t = setmetatable({}, {
    __metatable = "Locked"
})

setrawmetatable(t,{
    __index = function()

        return 0

    end
})
```

---

## setreadonly

Sets the readonly state of a Luau table.

### Syntax

```lua
setreadonly(
    table: table,
    readonly: boolean
)
```

### Parameters

| Parameter | Type |
|----------|------|
| table | table |
| readonly | boolean |

### Example

```lua
local mt = getrawmetatable(game)

setreadonly(mt,false)

mt.__namecall = function()

end

setreadonly(mt,true)
```

---

## isreadonly

Returns whether a table is readonly.

### Syntax

```lua
isreadonly(table: table) -> boolean
```

### Returns

Boolean.

### Example

```lua
print(
    isreadonly(
        getrawmetatable(game)
    )
)
```

---

## makewriteable

Shorthand for:

```lua
setreadonly(table,false)
```

### Syntax

```lua
makewriteable(table)
```

### Example

```lua
makewriteable(
    getrawmetatable(game)
)
```

---

## makereadonly

Shorthand for:

```lua
setreadonly(table,true)
```

### Syntax

```lua
makereadonly(table)
```

### Example

```lua
makereadonly(myTable)
```

---

## getnamecallmethod

Returns the current method being executed inside a `__namecall` hook.

### Syntax

```lua
getnamecallmethod() -> string?
```

### Returns

Method name.

### Example

```lua
hookmetamethod(
    game,
    "__namecall",

    function(self,...)

        print(
            getnamecallmethod()
        )

    end
)
```

---

## setnamecallmethod

Overrides the current method inside a `__namecall` hook.

### Syntax

```lua
setnamecallmethod(name: string)
```

### Parameters

| Parameter | Type |
|----------|------|
| name | string |

### Example

```lua
hookmetamethod(game,"__namecall",

function(self,...)

    if getnamecallmethod() == "Kick" then

        setnamecallmethod("Chat")

    end

end)
```

---

# Signal Library

Velocity exposes both Roblox's native signal API and a custom userland Signal implementation.

---

# Roblox Signals

Functions that operate directly on `RBXScriptSignal` objects.

---

## firesignal

Invokes every connection attached to a signal.

Supports cross-VM connections and preserves Instance identity.

### Syntax

```lua
firesignal(
    signal: RBXScriptSignal,
    ...args
)
```

### Example

```lua
firesignal(
    workspace.Part.Touched,
    workspace.OtherPart
)
```

---

## defersignal

Equivalent to `firesignal()` but dispatches through `task.defer()`.

### Syntax

```lua
defersignal(
    signal,
    ...args
)
```

### Example

```lua
defersignal(
    game.Players.PlayerAdded,
    game.Players.LocalPlayer
)
```

---

## replicatesignal

Invokes a signal locally and replicates it to the server.

Only works for allowlisted events.

### Syntax

```lua
replicatesignal(
    signal,
    ...args
)
```

### Example

```lua
replicatesignal(

    Remote.OnServerEvent,

    game.Players.LocalPlayer,

    "Hello"

)
```

---

## cansignalreplicate

Returns whether a signal supports replication.

### Syntax

```lua
cansignalreplicate(
    signal
) -> boolean
```

### Returns

Boolean.

### Example

```lua
if cansignalreplicate(
    Remote.OnServerEvent
) then

    replicatesignal(...)

end
```

---

## getconnections

Returns every connection attached to a signal.

Each connection exposes:

- Function
- Thread
- Enabled
- ForeignState
- Fire()
- Defer()
- Enable()
- Disable()
- Disconnect()

### Syntax

```lua
getconnections(
    signal
) -> {Connection}
```

### Example

```lua
for _,connection in ipairs(

    getconnections(
        workspace.ChildAdded
    )

) do

    connection:Disable()

end
```

---

## fireclickdetector

Simulates a ClickDetector interaction.

### Syntax

```lua
fireclickdetector(
    detector: ClickDetector,
    distance: number?
)
```

### Example

```lua
fireclickdetector(

    workspace.Button.ClickDetector,

    5

)
```

---

## fireproximityprompt

Triggers a ProximityPrompt instantly.

### Syntax

```lua
fireproximityprompt(
    prompt: ProximityPrompt
)
```

### Example

```lua
fireproximityprompt(

    workspace.Door.ProximityPrompt

)
```

---

## getsignalarguments

Returns the argument types accepted by a signal.

### Syntax

```lua
getsignalarguments(
    signal
) -> {string}
```

### Example

```lua
print(

table.concat(

    getsignalarguments(

        workspace.Part.Touched

    ),

", "

)

)
```

---

## getsignalargumentsinfo

Returns detailed information about every signal parameter.

### Syntax

```lua
getsignalargumentsinfo(
    signal
) -> {{Name,Type}}
```

### Example

```lua
for _,arg in ipairs(

    getsignalargumentsinfo(

        workspace.Part.Touched

    )

) do

    print(

        arg.Name,

        arg.Type

    )

end
```

---

## getsignalwhitelist

Returns every event that supports `replicatesignal()`.

### Syntax

```lua
getsignalwhitelist() -> table
```

### Example

```lua
for _,entry in ipairs(

    getsignalwhitelist()

) do

    print(

        entry.Parent.."."..entry.Event

    )

end
```

---

# Userland Signals

Velocity also provides its own signal implementation.

---

## Signal.new

Creates a custom Signal object.

### Syntax

```lua
Signal.new() -> Signal
```

### Example

```lua
local signal = Signal.new()
```

---

## Signal:Connect

Registers a callback.

### Syntax

```lua
Signal:Connect(
    callback
) -> Connection
```

### Example

```lua
local connection = signal:Connect(

function(value)

    print(value)

end)
```

---

## Signal:Once

Registers a callback that automatically disconnects after firing once.

### Syntax

```lua
Signal:Once(callback)
```

### Example

```lua
signal:Once(function()

    print("Executed.")

end)
```

---

## Signal:Wait

Yields until the next signal dispatch.

### Syntax

```lua
Signal:Wait() -> ...
```

### Example

```lua
local value = signal:Wait()

print(value)
```

---

## Signal:Fire

Dispatches the signal.

### Syntax

```lua
Signal:Fire(...args)
```

### Example

```lua
signal:Fire(

    "Hello",

    42

)
```

---

## Connection:Disconnect

Disconnects a signal callback.

### Syntax

```lua
connection:Disconnect()
```

### Example

```lua
connection:Disconnect()
```

---

# Actor Library

The Actor Library provides APIs for interacting with Roblox's parallel Luau Actors. These functions allow scripts to enumerate active Actor VMs, execute code across Actor boundaries, communicate between VMs and inspect parallel execution state.

---

## getactors

Returns every Actor that currently owns a live Lua VM.

### Syntax

```lua
getactors() -> {Instance}
```

### Aliases

- get_actors

### Returns

| Type | Description |
|------|-------------|
| {Instance} | Array containing every active Actor |

### Example

```lua
for _, actor in ipairs(getactors()) do
    print(actor:GetFullName())
end
```

---

## getactorthreads

Returns the main thread of every Actor VM.

The returned thread objects belong to foreign Lua states and should **never** be resumed or moved manually. They are intended for use with `run_on_thread()`.

### Syntax

```lua
getactorthreads() -> {thread}
```

### Aliases

- get_actor_threads

### Returns

Array of Actor threads.

### Example

```lua
for _, thread in ipairs(getactorthreads()) do

    run_on_thread(thread,[[
        print("Hello from Actor!")
    ]])

end
```

---

## isparallel

Returns whether the current execution context is running inside a desynchronized (parallel) thread.

### Syntax

```lua
isparallel() -> boolean
```

### Aliases

- checkparallel
- inparallel

### Returns

Boolean.

### Example

```lua
if isparallel() then

    task.synchronize()

end
```

---

## run_on_actor

Executes Lua source inside another Actor's VM.

Arguments are automatically marshaled between VMs, supporting:

- Primitive values
- Roblox Instances
- Tables
- Nested tables
- Cyclic references

### Syntax

```lua
run_on_actor(
    actor: Instance,
    source: string,
    ...args
)
```

### Parameters

| Parameter | Type |
|----------|------|
| actor | Instance |
| source | string |
| args | any |

### Example

```lua
local actor = getactors()[1]

run_on_actor(actor,[[

local greeting,target = ...

print(
    greeting,
    target.Name
)

]],

"Hello",

game.Players.LocalPlayer
)
```

---

## run_on_thread

Executes Lua source inside a foreign Actor thread.

Unlike `run_on_actor()`, this function skips Actor lookup and executes directly on the supplied thread.

### Syntax

```lua
run_on_thread(
    thread: thread,
    source: string,
    ...args
)
```

### Aliases

- runonthread

### Example

```lua
local threads = getactorthreads()

run_on_thread(

    threads[1],

[[

print(select(1,...))

]],

"Velocity"

)
```

---

## create_comm_channel

Creates a communication channel shared between multiple Actor VMs.

Returns both:

- Channel ID
- Local BindableEvent

### Syntax

```lua
create_comm_channel()
    -> number,
       BindableEvent
```

### Returns

| Type | Description |
|------|-------------|
| number | Channel ID |
| BindableEvent | Local event |

### Example

```lua
local id,event =
    create_comm_channel()

event.Event:Connect(function(message)

    print(message)

end)
```

---

## get_comm_channel

Returns the BindableEvent associated with a communication channel.

When called from another VM, Velocity transparently returns another userdata pointing to the same underlying Instance.

### Syntax

```lua
get_comm_channel(
    id: number
) -> BindableEvent?
```

### Example

```lua
local event =
    get_comm_channel(channelId)

event:Fire("Hello!")
```

---

# Garbage Collector Library

The Garbage Collector Library provides functions for enumerating and searching Lua objects currently managed by the garbage collector.

---

## getgc

Returns every collectable object currently allocated.

Passing `true` includes tables.

### Syntax

```lua
getgc(
    includeTables: boolean?
) -> {any}
```

### Returns

Array of GC objects.

### Example

```lua
for _, object in ipairs(

    getgc(true)

) do

    if typeof(object) == "table" then

        print(object)

    end

end
```

---

## filtergc

Searches the garbage collector using predicates.

Supports two search modes:

- Functions
- Tables

### Function Filters

| Property | Description |
|----------|-------------|
| Name | Function name |
| Constants | Constant list |
| Upvalues | Upvalues |
| Hash | Bytecode hash |
| IgnoreExecutor | Ignore executor-created closures |

### Table Filters

| Property | Description |
|----------|-------------|
| Keys | Required keys |
| Values | Required values |
| KeyValuePairs | Exact pairs |
| Metatable | Required metatable |

### Syntax

```lua
filtergc(
    kind: "function" | "table",
    filter: table,
    findFirst: boolean?
)
    -> any | {any}
```

### Example

```lua
local result = filtergc(

"table",

{

    Keys = {

        "password",

        "token"

    }

},

true

)

print(result)
```

---

# HTTP Library

The HTTP Library provides functions for sending HTTP requests.

---

## httpget

Performs an HTTP GET request.

### Syntax

```lua
httpget(
    url: string,
    followRedirects: boolean?
) -> string
```

### Returns

Response body.

### Example

```lua
local body = httpget(
    "https://api.github.com"
)

print(body)
```

---

## httppost

Performs an HTTP POST request.

### Syntax

```lua
httppost(

    url: string,

    body: string,

    contentType: string?

) -> string
```

### Parameters

| Parameter | Description |
|----------|-------------|
| url | Destination URL |
| body | Request body |
| contentType | MIME type |

### Example

```lua
local response = httppost(

    "https://httpbin.org/post",

    '{"hello":"world"}',

    "application/json"

)

print(response)
```

---

## request

Performs a fully configurable HTTP request.

### Syntax

```lua
request(options: table) -> table
```

### Supported Request Fields

| Field | Description |
|-------|-------------|
| Url | Target URL |
| Method | HTTP method |
| Headers | Request headers |
| Body | Request body |
| Cookies | Cookies |

### Response Fields

| Field | Description |
|-------|-------------|
| Success | Whether request succeeded |
| StatusCode | HTTP status code |
| StatusMessage | HTTP status |
| Headers | Response headers |
| Body | Response body |

### Aliases

- http_request
- http.request

### Example

```lua
local response = request({

    Url = "https://httpbin.org/headers",

    Method = "GET",

    Headers = {

        ["User-Agent"] = "Velocity"

    }

})

print(

    response.StatusCode,

    response.Body

)
```

---

# Input Library

The Input Library provides functions for simulating keyboard and mouse input. These functions allow scripts to emulate user interactions such as mouse clicks, cursor movement, scrolling, and keyboard events.

---

## Mouse Functions

Functions for simulating mouse input.

---

## mouse1click

Simulates a complete left mouse click.

### Syntax

```lua
mouse1click()
```

### Example

```lua
mouse1click()
```

---

## mouse1press

Presses and holds the left mouse button.

### Syntax

```lua
mouse1press()
```

### Example

```lua
mouse1press()

task.wait(0.5)

mouse1release()
```

---

## mouse1release

Releases the left mouse button.

### Syntax

```lua
mouse1release()
```

### Example

```lua
mouse1press()

task.wait(1)

mouse1release()
```

---

## mouse2click

Simulates a complete right mouse click.

### Syntax

```lua
mouse2click()
```

### Example

```lua
mouse2click()
```

---

## mouse2press

Presses and holds the right mouse button.

### Syntax

```lua
mouse2press()
```

### Example

```lua
mouse2press()

task.wait(0.25)

mouse2release()
```

---

## mouse2release

Releases the right mouse button.

### Syntax

```lua
mouse2release()
```

### Example

```lua
mouse2press()

task.wait(1)

mouse2release()
```

---

## mousemoveabs

Moves the cursor to an absolute screen position.

Coordinates are expressed in screen pixels.

### Syntax

```lua
mousemoveabs(
    x: number,
    y: number
)
```

### Parameters

| Parameter | Type | Description |
|----------|------|-------------|
| x | number | Screen X coordinate |
| y | number | Screen Y coordinate |

### Example

```lua
mousemoveabs(
    960,
    540
)
```

---

## mousemoverel

Moves the cursor relative to its current position.

### Syntax

```lua
mousemoverel(
    dx: number,
    dy: number
)
```

### Parameters

| Parameter | Type | Description |
|----------|------|-------------|
| dx | number | Horizontal offset |
| dy | number | Vertical offset |

### Example

```lua
mousemoverel(
    10,
    0
)
```

---

## mousescroll

Simulates mouse wheel scrolling.

Positive values scroll upward.

Negative values scroll downward.

### Syntax

```lua
mousescroll(
    delta: number
)
```

### Parameters

| Parameter | Type |
|----------|------|
| delta | number |

### Example

```lua
mousescroll(120)

mousescroll(-120)
```

---

# Keyboard Functions

Functions for simulating keyboard input.

---

## keypress

Presses and holds a keyboard key.

### Syntax

```lua
keypress(
    keyCode: number
)
```

### Parameters

| Parameter | Type | Description |
|----------|------|-------------|
| keyCode | number | Windows Virtual-Key Code |

### Example

```lua
keypress(0x57)

task.wait(1)

keyrelease(0x57)
```

The above example holds the **W** key for one second.

---

## keyrelease

Releases a previously pressed keyboard key.

### Syntax

```lua
keyrelease(
    keyCode: number
)
```

### Parameters

| Parameter | Type |
|----------|------|
| keyCode | number |

### Example

```lua
keyrelease(0x57)
```

---

## keyclick

Performs a complete key press and release.

Equivalent to:

```lua
keypress(key)

keyrelease(key)
```

### Syntax

```lua
keyclick(
    keyCode: number
)
```

### Aliases

- keytap

### Parameters

| Parameter | Type |
|----------|------|
| keyCode | number |

### Example

```lua
keyclick(0x1B)
```

The above example simulates pressing the **Escape** key.

---

# Common Virtual-Key Codes

| Key | Hex |
|------|-----|
| Left Mouse | 0x01 |
| Right Mouse | 0x02 |
| Backspace | 0x08 |
| Tab | 0x09 |
| Enter | 0x0D |
| Shift | 0x10 |
| Ctrl | 0x11 |
| Alt | 0x12 |
| Escape | 0x1B |
| Space | 0x20 |
| Left Arrow | 0x25 |
| Up Arrow | 0x26 |
| Right Arrow | 0x27 |
| Down Arrow | 0x28 |
| 0 | 0x30 |
| 1 | 0x31 |
| 2 | 0x32 |
| 3 | 0x33 |
| 4 | 0x34 |
| 5 | 0x35 |
| 6 | 0x36 |
| 7 | 0x37 |
| 8 | 0x38 |
| 9 | 0x39 |
| A | 0x41 |
| D | 0x44 |
| S | 0x53 |
| W | 0x57 |
| F1 | 0x70 |
| F2 | 0x71 |
| F3 | 0x72 |
| F4 | 0x73 |
| F5 | 0x74 |
| F12 | 0x7B |

---

## Notes

- Mouse coordinates use the operating system's screen coordinate system.
- Keyboard functions expect Windows Virtual-Key codes.
- `keyclick()` is functionally equivalent to calling `keypress()` followed by `keyrelease()`.
- Mouse button functions operate independently from keyboard input and may be combined for automation workflows.

---

# Drawing Library

The Drawing Library provides a lightweight rendering API for creating custom 2D overlays. Drawing objects are rendered independently from Roblox's UI system, making them suitable for ESPs, menus, debugging overlays and custom visualizations.

---

## Drawing.new

Creates a new drawing object.

### Syntax

```lua
Drawing.new(type: string) -> DrawObject
```

### Supported Types

| Type | Description |
|------|-------------|
| Line | Draws a line |
| Text | Draws text |
| Image | Displays an image |
| Circle | Draws a circle |
| Square | Draws a rectangle or square |
| Triangle | Draws a triangle |
| Quad | Draws a quadrilateral |

### Common Properties

Every Drawing object supports the following properties.

| Property | Type |
|----------|------|
| Visible | boolean |
| Transparency | number |
| Color | Color3 |
| ZIndex | number |

Each object also exposes additional properties depending on its type.

### Example

```lua
local text = Drawing.new("Text")

text.Text = "Hello Velocity"

text.Position = Vector2.new(
    100,
    100
)

text.Size = 20

text.Color = Color3.new(
    1,
    1,
    1
)

text.Visible = true
```

---

## cleardrawcache

Destroys every Drawing object created by the executor.

Useful when reloading scripts or cleaning up overlays.

### Syntax

```lua
cleardrawcache()
```

### Example

```lua
cleardrawcache()
```

---

## isrenderobj

Returns whether a value is a Drawing object.

### Syntax

```lua
isrenderobj(value: any) -> boolean
```

### Parameters

| Parameter | Type |
|----------|------|
| value | any |

### Returns

| Type |
|------|
| boolean |

### Example

```lua
local square = Drawing.new("Square")

print(
    isrenderobj(square)
)

print(
    isrenderobj({})
)
```

---

## getrenderproperty

Returns the value of a Drawing property.

Equivalent to:

```lua
object[property]
```

### Syntax

```lua
getrenderproperty(
    object: DrawObject,
    property: string
) -> any
```

### Parameters

| Parameter | Type |
|----------|------|
| object | DrawObject |
| property | string |

### Returns

Current property value.

### Example

```lua
local square = Drawing.new("Square")

print(

    getrenderproperty(

        square,

        "Visible"

    )

)
```

---

## setrenderproperty

Updates a Drawing property.

Equivalent to:

```lua
object[property] = value
```

### Syntax

```lua
setrenderproperty(
    object: DrawObject,
    property: string,
    value: any
)
```

### Parameters

| Parameter | Type |
|----------|------|
| object | DrawObject |
| property | string |
| value | any |

### Example

```lua
local square = Drawing.new("Square")

setrenderproperty(

    square,

    "Color",

    Color3.new(
        1,
        0,
        0
    )

)
```

---

## getfpscap

Returns the current FPS cap.

### Syntax

```lua
getfpscap() -> number
```

### Returns

| Type | Description |
|------|-------------|
| number | Current FPS limit (0 = Unlimited) |

### Example

```lua
print(
    getfpscap()
)
```

---

## setfpscap

Sets the client's FPS cap.

Passing **0** removes the frame limit.

### Syntax

```lua
setfpscap(fps: number)
```

### Parameters

| Parameter | Type |
|----------|------|
| fps | number |

### Example

```lua
setfpscap(240)
```

Unlimited FPS:

```lua
setfpscap(0)
```

---

# Drawing Example

Below is a complete example demonstrating how to create a filled square.

```lua
local square = Drawing.new("Square")

square.Size = Vector2.new(
    100,
    60
)

square.Position = Vector2.new(
    400,
    300
)

square.Color = Color3.fromRGB(
    255,
    84,
    69
)

square.Filled = true

square.Transparency = 0.8

square.Visible = true
```

---

# Object-Specific Properties

Depending on the object type, additional properties become available.

## Line

| Property | Type |
|----------|------|
| From | Vector2 |
| To | Vector2 |
| Thickness | number |

---

## Text

| Property | Type |
|----------|------|
| Text | string |
| Size | number |
| Font | number |
| Center | boolean |
| Outline | boolean |
| Position | Vector2 |

---

## Circle

| Property | Type |
|----------|------|
| Radius | number |
| Filled | boolean |
| NumSides | number |
| Position | Vector2 |

---

## Square

| Property | Type |
|----------|------|
| Size | Vector2 |
| Position | Vector2 |
| Filled | boolean |
| Thickness | number |

---

## Triangle

| Property | Type |
|----------|------|
| PointA | Vector2 |
| PointB | Vector2 |
| PointC | Vector2 |
| Filled | boolean |

---

## Quad

| Property | Type |
|----------|------|
| PointA | Vector2 |
| PointB | Vector2 |
| PointC | Vector2 |
| PointD | Vector2 |
| Filled | boolean |

---

## Image

| Property | Type |
|----------|------|
| Data | string |
| Position | Vector2 |
| Size | Vector2 |
| Rounding | number |

---

## Notes

- Drawing objects are rendered independently from Roblox's GUI system.
- Every object should be destroyed using `:Remove()` when no longer needed.
- `cleardrawcache()` destroys every Drawing object created by the current executor session.
- `getrenderproperty()` and `setrenderproperty()` are helper functions equivalent to directly indexing the object.

---

# Cache Library

The Cache Library provides functions for interacting with Roblox's internal userdata cache. These functions allow scripts to inspect, invalidate or redirect cached `Instance` references.

> **Note**
>
> These APIs operate on the executor's userdata cache and should only be used when necessary. Incorrect usage may lead to unexpected behavior if multiple references depend on the same cached object.

---

## cache.invalidate

Removes an `Instance` from Roblox's userdata cache.

The next time the instance is accessed from Lua, a fresh userdata will be created.

### Syntax

```lua
cache.invalidate(
    instance: Instance
)
```

### Parameters

| Parameter | Type | Description |
|----------|------|-------------|
| instance | Instance | Instance to invalidate |

### Returns

None.

### Example

```lua
local oldReference = workspace.Part

cache.invalidate(
    workspace.Part
)

print(
    workspace.Part == oldReference
)
```

Output:

```lua
false
```

---

## cache.iscached

Returns whether an `Instance` currently exists inside the userdata cache.

### Syntax

```lua
cache.iscached(
    instance: Instance
) -> boolean
```

### Parameters

| Parameter | Type |
|----------|------|
| instance | Instance |

### Returns

| Type | Description |
|------|-------------|
| boolean | Whether the instance is currently cached |

### Example

```lua
print(

    cache.iscached(

        workspace.Part

    )

)
```

---

## cache.replace

Redirects one cached userdata reference to another `Instance`.

After replacement, Lua references pointing to the original cached object will resolve to the new instance.

### Syntax

```lua
cache.replace(

    oldInstance: Instance,

    newInstance: Instance

)
```

### Parameters

| Parameter | Type |
|----------|------|
| oldInstance | Instance |
| newInstance | Instance |

### Returns

None.

### Example

```lua
cache.replace(

    workspace.RealPart,

    workspace.FakePart

)

print(

    workspace.RealPart ==

    workspace.FakePart

)
```

Output:

```lua
true
```

---

# Cache Workflow Example

The example below demonstrates the complete cache workflow.

```lua
local original = workspace.Part

print(

    cache.iscached(original)

)

cache.invalidate(original)

local newReference = workspace.Part

print(

    original == newReference

)

cache.replace(

    newReference,

    workspace.OtherPart

)
```

---

# Notes

- `cache.invalidate()` only removes the cached userdata reference. The underlying Roblox `Instance` is **not** destroyed.
- `cache.replace()` redirects userdata references without modifying the actual DataModel hierarchy.
- `cache.iscached()` can be used to determine whether an `Instance` already has an active userdata representation.
- Cache operations affect userdata identity and should be used with caution when multiple scripts share references.

---

# See Also

- `cloneref()`
- `compareinstances()`
- `getinstances()`
- `getnilinstances()`

---

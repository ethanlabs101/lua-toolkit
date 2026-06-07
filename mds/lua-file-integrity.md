# Lua File Integrity

Here is a small file integrity check for GG lua scripting.

I developed this in the Game Guardian niche as a simple way to protect
the name of your script. 
Most Game Guardian scripts released are not open source,
they are layered with obfuscation, encryption, and encoding.

Although its relatively simple, this function proved useful in its enviornment.

Check It Out!

````lua

detector = gg.getFile():match('[^/]+$')  
name = "file_name.lua"  

if detector == name then  
    -- The script name matches, continue execution  
else  
    gg.alert("CHANGE NAME BACK TO ORIGINAL YOU NAUGHTY SKID") -- Stops execution with an error message  
    print("Idiot")
    os.exit()
 
end

````

In GG, there are many GG-specific API functions such as gg.alert, gg.sleep etc.
This function utilizes a GG-specifc API called gg.getFile().

Unlike standard Lua which does not have a direct equivalent to gg.getFile(),
this specific API function allows you to return the path to the current script
file being executed.

Standard Lua intentionally excludes file I/O libraries that allow reading the script's own path or contents from within the script itself for security and sandboxing reasons.

---

## How Does it Work?

Let's break it down:

The key component of this utility is:

```lua
gg.getFile():match('[^/]+$')
```

This means `gg.getFile()` reads the full path of the currently executing script.

For example:

```text
/storage/emulated/0/Download/file_name.lua
```

The Lua pattern:

```lua
[^/]+$
```

is responsible for extracting only the filename from that path.

### Understanding the Pattern

```lua
[^/]
```

Matches any character that is **not** a forward slash (`/`).

The caret (`^`) inside square brackets means "not."

Examples:

```text
f
i
l
e
_
n
a
m
e
.
l
u
a
```

All match because none of them are `/`.

---

```lua
+
```

Means "one or more" of the previous match.

So:

```lua
[^/]+
```

means:

> Match one or more characters that are not a slash.

---

```lua
$
```

Represents the end of the string.

Combining everything:

```lua
[^/]+$
```

tells Lua:

> Starting from the end of the path, capture every character until a slash is encountered.

Given:

```text
/storage/emulated/0/Download/file_name.lua
```

The result becomes:

```text
file_name.lua
```

which is then stored in:

```lua
detector
```

---

## The Filename Check

Once the filename has been extracted, it is compared against the expected name:

```lua
name = "file_name.lua"

if detector == name then
    -- continue execution
else
    os.exit()
end
```

If the script has its original name:

```text
file_name.lua
```

the check succeeds and execution continues normally.

If someone renames the file:

```text
my_free_premium_script.lua
```

then:

```lua
detector = "my_free_premium_script.lua"
```

which no longer matches:

```lua
name = "file_name.lua"
```

causing the script to terminate.

---

## Limitations

This is not a true integrity check.

A real integrity check typically verifies file contents using techniques such as checksums or cryptographic hashes.

This utility only verifies the filename.

However, within the Game Guardian scripting community, it can still be useful for discouraging casual renaming, repackaging, or redistribution of scripts by inexperienced users.

Its strength comes from simplicity rather than security.


# ***Lua GG File Name Checker***

Here is a small file name check for GG lua scripting.

I developed this in the Game Guardian niche as a simple way to protect
the name of your script. 

Most Game Guardian scripts released are not open source,
they are layered with obfuscation, encryption, and encoding.

This makes a simple check like this useful in context, when
layered with other protections.

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

Unlike Game Guardian, standard Lua does not provide a built-in function for retrieving the path of the currently executing script.

Standard Lua intentionally excludes file I/O libraries that allow reading the script's own path or contents from within the script itself for security and sandboxing reasons.

---

## ***How Does it Work?***

Let's break it down:

The key component of this utility is:

```lua
gg.getFile():match('[^/]+$')
```

The Game Guardian API function `gg.getFile()` returns the full path of the currently executing script.

For example:

```text
/storage/emulated/0/Download/file_name.lua
```

The Lua pattern:

```lua
[^/]+$
```

is then used to extract only the filename from that path.

### ***Understanding the Pattern***

The pattern is made up of three parts:

```lua
[^/]
```

Matches any character that is **not** a forward slash (`/`).

The caret (`^`) inside square brackets means "not," so this pattern will match characters such as:

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

because none of them are `/`.

---

```lua
+
```

Means "one or more" of the previous match.

This turns:

```lua
[^/]
```

into:

```lua
[^/]+
```

which means:

> Match one or more consecutive characters that are not a slash.

---

```lua
$
```

Represents the end of the string.

This is the part that makes the pattern useful.

A common misconception would be the pattern should return:

```text
storageemulated0Downloadfile_name.lua
```

because every character except `/` technically matches `storageemulated0Downloadfile_name.lua`.

However, the `$` forces Lua to find a match that reaches the **end of the string**.

Given:

```text
/storage/emulated/0/Download/file_name.lua
```

Lua sees multiple valid non-slash sequences:

```text
storage
emulated
0
Download
file_name.lua
```

But only one of them appears at the very end of the path:

```text
file_name.lua
```

Because of the `$`, that is the value returned by `match()`.

The result is stored in:

```lua
detector
```

which now contains:

```text
file_name.lua
```

### ***The Filename Check***

Once the filename has been extracted, it is compared against the expected script name:

```lua
name = "file_name.lua"

if detector == name then
    -- continue execution
else
    os.exit()
end
```

If the filename matches, execution continues normally.

If someone renames the script:

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

### ***Limitations***

This is not a true file integrity check.

A real integrity check verifies file contents using techniques such as hashes or checksums to detect modifications.

This utility only verifies the filename.

However, within the Game Guardian scripting community, it can still be useful for discouraging casual renaming, repackaging, or redistribution of scripts by inexperienced users.

Its strength comes from simplicity rather than security.

---

### ***Sources / Links***

| Link | Description |
|------|-------------|
| [Game Guardian](https://gameguardian.net/forum/) | Official Game Guardian homepage
| [gg.getFile()](https://gameguardian.net/help/classgg.html#a835c5691f8e6512057a87e86169a0164) | Official GG gg.getFile() documentation

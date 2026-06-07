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

## How Does it work?

Lets break it down:

The key component of this utility is gg.getFile():match('[^/]+$')

This means gg.getFile() reads

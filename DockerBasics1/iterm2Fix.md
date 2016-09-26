<!-- iterm2 has an error.  Does not "hook" terminal to Docker Machine properly (no whale icon when run Docker QuickTerm.  To fix replace code in file path below with the following code ....)

/Applications/Docker/Docker Quickstart Terminal.app/Contents/Resources/Scripts/iterm.scpt

---- replace all code in iterm.scpt with the following ---- -->


set itermRunning to (application "iTerm" is running)
set scriptPath to quoted form of POSIX path of ((path to me as text) & "::" & "start.sh")
set user_shell to do shell script "dscl /Search -read /Users/$USER UserShell | awk '{print $2}'"

tell application "iTerm"
    activate
    if not (exists window 1) or (itermRunning = false) then
        reopen
    end if

    try
        tell current window
            set newTab to (create tab with default profile)
            tell current session of newTab
                write text "bash --login " & scriptPath
            end tell
        end tell
    on error
        tell current session of (create window with default profile)
            write text "bash --login " & scriptPath
        end tell
    end try
end tell

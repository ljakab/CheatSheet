Check power settings from CLI

    pmset -g


Eject CD/DVD from CLI

    drutil eject


Remote Virtual Interface

Remote Virtual Interface (rvi) is very useful for analysing. Install latest
Xcode and run it (needed once to install some programs/scripts). Connect
iphone/ipad via USB. Lookup UDID in iTunes (click on serial number in main
page and right click on UDID). On macbook run "rvictl -s <udid>". You now have
a rvi0 device on your macbook on which you can snoop and see all network
traffic of iphone/ipad (wireless/cellular). End with rvictl -x <udid>. Have
fun :-)</udid></udid>


Start Eclipse with Java 7

    sudo mkdir /System/Library/Java/JavaVirtualMachines
    sudo ln -s /Library/Java/JavaVirtualMachines/1.7.0.jdk /System/Library/Java/JavaVirtualMachines/1.6.0.jdk


Remap <Home> and <End> keys for native apps

Create the `~/Library/KeyBindings/DefaultKeyBinding.dict` file with the
following content:

    {
        /* Remap Home / End keys to be correct */
        "\UF729"   = "moveToBeginningOfLine:"; /* Home */
        "\UF72B"   = "moveToEndOfLine:"; /* End */
        "$\UF729"  = "moveToBeginningOfLineAndModifySelection:"; /* Shift + Home */
        "$\UF72B"  = "moveToEndOfLineAndModifySelection:"; /* Shift + End */
        "^\UF729"  = "moveToBeginningOfDocument:"; /* Ctrl + Home */
        "^\UF72B"  = "moveToEndOfDocument:"; /* Ctrl + End */
        "$^\UF729" = "moveToBeginningOfDocumentAndModifySelection:"; /* Shift + Ctrl + Home */
        "$^\UF72B" = "moveToEndOfDocumentAndModifySelection:"; /* Shift + Ctrl + End */
    }

You need to reopen applications for changes to take effect.  Xcode, Terminal,
and many cross-platform applications ingnore `Xcode, Terminal, and many
cross-platform applications ignore `DefaultKeyBinding.dict`.


Remap <Home and <End> keys in Eclipse

Preferences->General->Keys, filter for "line start" and "line end" and change
binding.


Remap <Home and <End> keys in Firefox/Thunderbird

Use the Keyfixer addon


List IPv4 open ports

    lsof -nP -i4 | grep LISTEN

DNS query using the macOS resolver

    dns-sd -G v4v6 www.example.com

Avoid DavMail using the dGPU all the time:

    defaults write /Applications/DavMail.app/Contents/Info.plist NSSupportsAutomaticGraphicsSwitching -bool YES

Same for WebEx

    defaults write /Applications/Webex/Cisco\ Webex\ Meetings.app/Contents/Info.plist NSSupportsAutomaticGraphicsSwitching -bool YES

Disable an installed JDK to be selected as default

    mv $PATH_TO_JDK/Content/Info.plist $PATH_TO_JDK/Content/Info.plist.disabled

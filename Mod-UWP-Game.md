# How to extract and mod a UWP game

1. Dump the files:
   1. Download UWPDumper: <https://github.com/Wunkolo/UWPDumper>
   2. Extract UWPDumper to a temporary folder
   3. Start your game
   4. Run UWPDumper's "UWPInjector.exe"
   5. Pick your game's PID
   6. Wait for UWPDumper to extract the files
      * This will probably take a while
      * When it finishes, it should open an Explorer to a directory like `%LOCALAPPDATA%\Packages\YourGame\TempState\DUMP`
2. Copy the files where you want them:
   1. Open a console (Explorer can't do this copy)
      * You might want PowerShell so you can reuse it later
   2. `cd %LOCALAPPDATA%\Packages\YourGame\TempState\DUMP`
   3. `robocopy /E . D:\Games\YourGame`
      * This will probably take a while
      * `/E` means "copy subdirectories including empty ones"
      * `/DCOPY:D` would copy files without their attributes, which might help in some cases
   4. Double-check the output from `robocopy`: did anything fail?
3. Decrypt the destination files (might or might not be necessary):
   1. Right-click destination directory
   2. Pick "Properties", then "Advanced"
   3. Uncheck "Encrypt contents to secure data"
   4. Pick "OK"
   5. Pick "OK" again
   6. Pick "Apply changes to this folder, subfolders, and files"
   7. Pick "OK" one last time
4. Consider making a backup:
   * It took a while to get here. Maybe archive the destination files in case modding goes wrong?
5. Clean up:
   * Delete `%LOCALAPPDATA%\Packages\YourGame\TempState\DUMP`
   * Uninstall the Microsoft Store version of the game
     * **This is required** or the `Add-AppxPackage` command will fail
6. Register the new app location:
   1. Enable "Developer Mode" in the Windows Settings, under "For Developers"
      * This will allow you to run the app from loose files instead of a sealed APPX package
   2. Open (or reuse) a PowerShell console
   3. `cd D:\Games\YourGame`
   4. `Add-AppxPackage -Register AppxManifest.xml`

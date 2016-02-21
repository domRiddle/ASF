# Import

This page covers topics about importing data from other applications like Steam Desktop Authenticator.

## Import Auth-Files from Steam Desktop Authenticator (SDA)

ASF and SDA are using the same Files for 2FA-Authentication and this said you can import the .maFile-Files from SDA as .auth-Files for ASF. All we need is a little batch-script to rename the SDA-Files so they fit ASF. Normally you would use "username.xml" in ASF. Create a folder on your desktop and Backup(!!) your .maFiles from SDA. Then copy all your .maFile-Files into this folder. Then create a textfile, give it a name and change the file-extension to ".bat". Then you can copy&paste the following script and use it to rename the files from (SteamID64)(.maFile) to (SteamLoginName)(.auth).
**This will only work if you use config-files with (SteamLoginName)(.xml), but you can modify this script easily.**

```batch
@echo off
setlocal EnableDelayedExpansion

echo This script will search for files in _this_ directory with (.maFile)-Extension and replace them with "username(.auth)"
set /p=Hit ENTER to continue...

for %%F in (*.maFile) do (

	for /f "tokens=1" %%i in (%%F) do (
		for /f "tokens=14 delims=:," %%a in ("%%i") do (
			set user=%%~na.auth
			ren "%%F" "!user!"
			echo Filename %%F replaced with !user!
		)
	)

)

pause
```

Maybe you discover that some of your xml-configs saved with lower- and uppercase usernames. The .maFile-Files from SDA only using lowercase login-names and if you have uppercase characters in your ASF config-Files they wouldn´t match. Same as before, create a folder, backup your xml-Configs, copy all xml-Configs into this folder, create a textfile, name it, replace file-Extension with ".bat" and copy&paste the following script to rename all files to lowercase.

```batch
@echo off
setlocal EnableDelayedExpansion

echo This script will rename all files in this directory to lowercase.
set /p=Hit ENTER to continue...

for /f "Tokens=*" %%f in ('dir /l/b/a-d') do (ren "%%f " "%%f ")
echo All files were renamed to lowercase. You can close this window now.

pause
```
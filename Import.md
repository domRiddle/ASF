# Import

This page covers topics about importing data from other applications like Steam Desktop Authenticator.

## Import Auth-Files from Steam Dekstop Authenticator (SDA)

ASF and SDA are using the same Files for 2FA-Authentication and this said you can import the .maFile-Files from SDA as .auth-Files for ASF. All we need is a little batch-script to rename the SDA-Files so they fit ASF. Normally you would use "username.xml" in ASF. Create a folder on your desktop and Backup(!!) your .maFiles from SDA. Then copy all your .maFile-Files into this folder. Then create a textfile, give it a name and change the file-extension to ".bat". Then you can use this batch-script to rename the files from (SteamID64).(maFile) to (SteamLoginName)(.auth).

``@echo off
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
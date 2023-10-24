# BatchGitUpdater
Keep any Script up to date.
If you start your script it checks if there are newer Versions in a Github Repo and if so it downloads the Update. You can implement this very easy in your script. Windows OS is requiered

# How to Use
If you dont have a Batch File in your Script/Program That executes at the Begining go [here](##1)
If you already have a Batch File in your Script/Program go [here](##2)

## 1 
The start.bat File has to be in the Same Folder as your Script/Program
Paste the start.bat File in your Script Folder
Then change the 3 Texts in the 6,7,8 Line and The 3 Last lines. Then its ready.


## 2 
The start.bat File has to be in the Same Folder as your Script/Program
Implement this in your Script. You only need to Modify the gitver,owner and repo Variable and the content at the Bottom. Then it works.

```
set "gitver=--The Version of your Script--"
cd %~dp0
color 2

:inscheck1
for /f "delims=" %%a in ('where git') do (
    set "where_output=%%a"
)
if defined where_output (
    set /a installed+=1
    goto inscheck2
) else (
    goto jinscheck2
)

:inscheck2
for /f "delims=" %%a in ('where scoop') do (
    set "where_output=%%a"
)
if defined where_output (
    set /a installed+=1
    goto inscheck3
) else (
    goto inscheck3
)

:inscheck3
for /f "delims=" %%a in ('where jq') do (
    set "where_output=%%a"
)
if defined where_output (
    set /a installed+=1
    goto inscheck4
) else (
    goto inscheck4
)

:inscheck4
for /f "delims=" %%a in ('where jid') do (
    set "where_output=%%a"
)
if defined where_output (
    set /a installed+=1
) else (
    goto instcheckdone
)

:instcheckdone
if %installed% == 4 (
    goto updtsearch
) else (
    goto installgit
)


:installgit
echo.
@ping -n 1 localhost> nul
echo When you want the script to auto upgrade itself when you start it you can install Git, JQ , Jid and Scoop automaticly too.
echo This will be fully automaticly.
echo When you choose to Install Git too it needs Administrator privileges too.
echo.
@ping -n 1 localhost> nul
echo [1] Install those Too
@ping -n 1 localhost> nul
echo.
@ping -n 1 localhost> nul
echo [2] View Information about Git
@ping -n 1 localhost> nul
echo.
@ping -n 1 localhost> nul
echo [3] View Information about Jid
@ping -n 1 localhost> nul
echo.
@ping -n 1 localhost> nul
echo [4] View Information about JQ
@ping -n 1 localhost> nul
echo.
@ping -n 1 localhost> nul
echo [5] View Information about Scoop
@ping -n 1 localhost> nul
echo.
@ping -n 1 localhost> nul
echo [6] Dont install those Components.
echo.
@ping -n 1 localhost> nul
echo.
set /p menu3=Choose an Option from Above:
If %menu3% == 1 goto gitinstall
If %menu3% == 2 start "" "https://git-scm.com" | goto installgit
If %menu3% == 3 start "" "https://github.com/simeji/jid" | goto installgit
If %menu3% == 4 start "" "https://jqlang.github.io/jq/" | goto installgit
If %menu3% == 5 start "" "https://scoop.sh/#/" | goto installgit
If %menu3% == 6 goto openscript




:gitinstall
for /f "delims=" %%a in ('where git') do (
    set "where_output=%%a"
)
if defined where_output (
    echo Git is already installed!
) else (
    winget install --id Git.Git -e --source winget
)


for /f "delims=" %%a in ('where scoop') do (
    set "where_output=%%a"
)
if defined where_output (
    echo Scoop is already installed!
) else (
    PowerShell -Command "Set-ExecutionPolicy RemoteSigned -Scope CurrentUser"
    PowerShell -Command "iex (irm https://get.scoop.sh)"
)


for /f "delims=" %%a in ('where jq') do (
    set "where_output=%%a"
)
if defined where_output (
    echo jq is already installed!
) else (
    scoop install jq
)


for /f "delims=" %%a in ('where jid') do (
    set "where_output=%%a"
)
if defined where_output (
    echo jid is already installed!
    goto installgitdone
) else (
    scoop install jid
    goto installgitdone
)

:installgitdone
echo Git,Scoop,JQ and JID are now Successfull installed.
echo The Script will now check for Updates...
timeout 5

goto updtsearch


:updtsearch
set "owner=--The Name of Your GitHub Account"
set "repo=Your GitHub Repo Name"
set "api_url=https://api.github.com/repos/%owner%/%repo%/releases/latest"
echo Fetching Git Url....
@ping -n 1 localhost> nul
for /f "usebackq tokens=*" %%i in (`curl -s %api_url% ^| jq -r ".tag_name"`) do (set "latest_version=%%i")
if "%latest_version%"=="%gitver%" (goto UpToDate) else (goto gitverout)

:UpToDate
scoop update
scoop update jq
scoop update jid
winget upgrade --id Git.Git -e --source winget
@ping -n 1 localhost> nul
echo The Version you are currently Using is the newest one (%latest_version%)
goto openscript


:gitverout
echo.
@ping -n 1 localhost> nul
echo Version Outdated!
@ping -n 1 localhost> nul
echo.
@ping -n 1 localhost> nul
echo Please consider downloading the new Version. 
@ping -n 1 localhost> nul
echo The Version you are currently using is %gitver12%
@ping -n 1 localhost> nul 
echo The newest Version avaiable is %latest_version%
@ping -n 1 localhost> nul
echo.
echo [1] Update
@ping -n 1 localhost> nul
echo.
@ping -n 1 localhost> nul
echo [2] Continue Anyways
@ping -n 1 localhost> nul
echo.
@ping -n 1 localhost> nul
set /p menu4=Choose an Option from Above:
If %menu4% == 1 goto gitupt
If %menu4% == 2 goto openscript
goto gitverout

:gitupt
cd ..
mkdir %latest_version%
cd %latest_version%
git clone https://github.com/%owner%/%repo% %cd%
echo Downloaded
Paste the Name Of the File you Want to open after the script update has been downloaded+the ending here 
```

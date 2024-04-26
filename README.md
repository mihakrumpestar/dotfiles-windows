# Windows dotfiles

## ISO install

1. Standard Windows installer:

   > When you get to the "Let's add your Microsoft account" phase in Windows 11 Setup, enter `no@thankyou.com` in the Sign in field and then select the Next button.

   [Disable Defender](https://github.com/swagkarna/Defeat-Defender-V1.2.0):
   Alternatives: [alt1](https://github.com/qtkite/defender-control), [alt2](https://github.com/teeotsa/windows-11-debloat)

2. [Ghost Spectre](https://tech-latest.com/ghost-spectre-windows-11/)

[Activate Windows and Office](https://github.com/massgravel/Microsoft-Activation-Scripts):

```powershell
irm https://massgrave.dev/get | iex
```

[Chris Titus winutil](https://github.com/ChrisTitusTech/winutil):

```powershell
iwr -useb https://christitus.com/win | iex
```

## Miscellaneous

Create user:

```powershell
# Define the username and password
$userName = "<NewAdminUser>"
$password = ConvertTo-SecureString "<YourStrongPasswordHere>" -AsPlainText -Force

# Create the new local user
New-LocalUser -Name $userName -Password $password -Description "Administrator account" -AccountNeverExpires

Set-LocalUser -Name $userName -PasswordNeverExpires $true
Add-LocalGroupMember -Group "Administrators" -Member $userName
```

## Package managers

Stores:

- [Chocolatey](https://community.chocolatey.org/packages):

  ```powershell
  Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))

  # Upgrade with
  # choco upgrade [program]
  ```

- Winget:

  ```powershell
  winget upgrade winget
  ```

## Programs

- Basic:

  ```powershell
  $packages = @(
    "SomePythonThings.WingetUIStore", # UI for package managers
    "LibreWolf.LibreWolf Brave.Brave",  # Browsers
    "ONLYOFFICE.DesktopEditors", # Office suite
    "Daum.PotPlayer GIMP.GIMP Inkscape.Inkscape DuongDieuPhap.ImageGlass Upscayl.Upscayl OBSProject.OBSStudio", # Media, alternatives: nomacs, VideoLAN.VLC
    "ActivityWatch.ActivityWatch 7zip.7zip KDE.Kate Microsoft.VisualStudioCode geeksoftwareGmbH.PDF24Creator Ventoy.Ventoy MatteoRossi.iCopy Nextcloud.NextcloudDesktop NGWIN.PicPick KDE.Okular VirtualHere.USBClient Xournal++.Xournal++ BleachBit.BleachBit voidtools.Everything stnkl.EverythingToolbar", # Tools
    "Audacity.Audacity", # Audio
    "KeePassXCTeam.KeePassXC Bitwarden.Bitwarden", # Password manager
    "Discord.Discord Zoom.Zoom", # Communication
    "AntibodySoftware.WizTree MiniTool.PartitionWizard.Free Klocman.BulkCrapUninstaller GlennDelahoy.SnappyDriverInstallerOrigin", # System utilities
    "CPUID.CPU-Z CPUID.HWMonitor CrystalDewWorld.CrystalDiskInfo", # System info
    "Python.Python.3.12" # Language support
  )

  $command = "winget install --accept-source-agreements --silent --disable-interactivity --accept-package-agreements " + ($packages -join ' ')
  Invoke-Expression $command

  $packages = @(
    "inventor autocad", # CAD
    "superslicer", # 3D printing
    "raidrive" # Mount storage
    "handbrake", # Video
  )

  $command = "choco install -y " + ($packages -join ' ')
  Invoke-Expression $command

  # Manually install LaserCut
  ```

- Advanced:

  ```powershell
  choco install -y github-desktop msiafterburner libreoffice-fresh
  # For MS Office
  choco install -y office-tool

  # Manually install https://www.bikechecker.com/demo.php # LinkageX3
  ```

- Benchmarks:

  ```powershell
  choco install -y cinebench performancetest crystaldiskmark
  ```

- Laptop:

  ```powershell
  $packages = @(
    "9WZDNCRFJ4MV" # Lenovo Vantage - drivers
    "9MVLWT5DMSKR" # Lenovo Pen Settings
  )

  $command = "winget install --accept-source-agreements --silent --disable-interactivity --accept-package-agreements " + ($packages -join ' ')
  Invoke-Expression $command
  ```

## Manually

- Snappy Driver Installer Origin: Install drivers
- Set default apps
- Set dark theme
- Enable printer service and add printer
- Enable fingerprint
- Startup:

  ```powershell
  C:\ProgramData\Microsoft\Windows\Start Menu\Programs\Startup
  ```

  Add programs (shortcuts) to be run on startup:

  - VirtualHere Client

- Firefox addons:
  - [ublock-origin](https://addons.mozilla.org/en-US/firefox/addon/ublock-origin)
  - [instant-translate](https://addons.mozilla.org/en-US/firefox/addon/instant-translate)
  - [tabliss](https://addons.mozilla.org/en-US/firefox/addon/tabliss/)
  - [bitwarden](https://addons.mozilla.org/en-US/firefox/addon/bitwarden-password-manager/)

## Fixes

- [Fix corrupt windows install](https://christitus.com/fix-corrupt-windows-install/)

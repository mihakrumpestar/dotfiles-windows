<!-- markdownlint-disable MD033 MD041 -->

<!--- Title --->

<h1 align="center" style="font-size: 42px; font-weight: bold; margin-top: 200px; margin-bottom: 60px;">
Windows dotfiles
</h1>

# 1. ISO install

## 1.1. [Ghost Spectre](https://tech-latest.com/ghost-spectre-windows-11/)

Others:

- Standard Windows installer:

  > When you get to the "Let's add your Microsoft account" phase in Windows 11 Setup, enter `no@thankyou.com` in the Sign in field and then select the Next button.

  [Disable Defender](https://github.com/swagkarna/Defeat-Defender-V1.2.0): [alt1](https://github.com/qtkite/defender-control), [alt2](https://github.com/teeotsa/windows-11-debloat)

- Windows 10 Enterprise LTSC (you can use NTLite to modify ISO)
- [ReviOS](https://revi.cc/revios/download/?method=iso): custom Windows
- [Gandalf’s Windows 11 PE](http://windowsmatters.com/): live Windows debugging

## 1.2. [Activate Windows and Office](https://github.com/massgravel/Microsoft-Activation-Scripts):

```powershell
irm https://get.activated.win | iex
```

## 1.3. [Chris Titus winutil](https://github.com/ChrisTitusTech/winutil) (can also fix installation):

```powershell
iwr -useb https://christitus.com/win | iex
```

# 2. Miscellaneous

Create user:

```powershell
# Define the username and password
$userName = <userName>
$password = ConvertTo-SecureString <password> -AsPlainText -Force

# Create the new local user
New-LocalUser -Name $userName -Password $password -Description "Administrator account" -AccountNeverExpires

Set-LocalUser -Name $userName -PasswordNeverExpires $true
Add-LocalGroupMember -Group "Administrators" -Member $userName
```

Enable Printer Service:

```powershell
$printerService = "Spooler"
if ((Get-Service -Name $printerService).Status -ne 'Running') {
    Set-Service -Name $printerService -StartupType Automatic
    Start-Service -Name $printerService
    Write-Output "$printerService service is now enabled and started."
} else {
    Write-Output "$printerService service is already running."
}
```

Enable Fingerprint Service:

```powershell
$fingerprintService = "WbioSrvc"
if ((Get-Service -Name $fingerprintService).Status -ne 'Running') {
    Set-Service -Name $fingerprintService -StartupType Automatic
    Start-Service -Name $fingerprintService
    Write-Output "$fingerprintService service is now enabled and started."
} else {
    Write-Output "$fingerprintService service is already running."
}
```

# 3. Store

  ```powershell
  winget install --exact --id MartiCliment.UniGetUI --source winget
  ```

# 4. Programs

- Basic:

  ```powershell
  $packages = @(
    "RClone-Manager.rclone-manager", # Rclone desktop UI
    "LibreWolf.LibreWolf", "Brave.Brave",  # Browsers
    "ONLYOFFICE.DesktopEditors", # Office suite
    "Daum.PotPlayer", "GIMP.GIMP", "Inkscape.Inkscape", "DuongDieuPhap.ImageGlass", "Upscayl.Upscayl", "OBSProject.OBSStudio", # Media
    "ActivityWatch.ActivityWatch", "7zip.7zip", "KDE.Kate", "Microsoft.VisualStudioCode", "geeksoftwareGmbH.PDF24Creator", "Ventoy.Ventoy", "Nextcloud.NextcloudDesktop", "NGWIN.PicPick", "KDE.Okular", "VirtualHere.USBClient", "Xournal++.Xournal++", "BleachBit.BleachBit", "voidtools.Everything", "stnkl.EverythingToolbar", # Tools
    "Audacity.Audacity", # Audio
    "KeePassXCTeam.KeePassXC", "Bitwarden.Bitwarden", # Password manager
    "Discord.Discord", # Communication
    "AntibodySoftware.WizTree", "AOMEI.PartitionAssistant", "Klocman.BulkCrapUninstaller", "GlennDelahoy.SnappyDriverInstallerOrigin", # System utilities
    "CPUID.CPU-Z", "CPUID.HWMonitor", "CrystalDewWorld.CrystalDiskInfo", "FinalWire.AIDA64.Extreme", "Maxon.CinebenchR23" # System info
  )

  $command = "winget install --accept-source-agreements --silent --disable-interactivity --accept-package-agreements " + ($packages -join ' ')
  Invoke-Expression $command

  $packages = @(
    "office-tool", # MS Office installer
    "inventor", "autocad", # CAD
    "superslicer", # 3D printing
    "handbrake", # Video
    "kdeconnect-kde", # Connect devices
  )

  $command = "choco install -y " + ($packages -join ' ')
  Invoke-Expression $command
  ```

  Manually install:

  - LaserCut

  To be installed by users:

  - Solidworks

- Advanced:

  ```powershell
  choco install -y github-desktop msiafterburner libreoffice-fresh
  ```

  Manually install [LinkageX3](https://www.bikechecker.com/demo.php)

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

# 5. Manually

- Snappy Driver Installer Origin: Install drivers
- Install MS Office if required
- Set RClone-Manager shared storage
- Set default apps
- Set dark theme
- Add printer
- Startup:

  ```powershell
  C:\ProgramData\Microsoft\Windows\Start Menu\Programs\Startup
  ```

  Add programs (shortcuts) to be run on startup:

  - VirtualHere Client

- Firefox addons:
  - [Cookie AutoDelete](https://addons.mozilla.org/en-US/firefox/addon/cookie-autodelete/)
  - [Dark Reader](https://addons.mozilla.org/en-US/firefox/addon/darkreader/)
  - [KeePassXC-Browser](https://addons.mozilla.org/en-US/firefox/addon/keepassxc-browser/)
  - [Mate Translate – translator, dictionary](https://addons.mozilla.org/en-US/firefox/addon/instant-translate/)
  - [Panorama Tab Groups](https://addons.mozilla.org/en-US/firefox/addon/panorama-tab-groups/)
  - [Simple Translate](https://addons.mozilla.org/en-US/firefox/addon/simple-translate/)
  - [Tabliss - New Tab](https://addons.mozilla.org/en-US/firefox/addon/tabliss/)
  - [TWP - Translate Web Pages](https://addons.mozilla.org/en-US/firefox/addon/traduzir-paginas-web/)
  - [uBlock Origin](https://addons.mozilla.org/en-US/firefox/addon/ublock-origin)
  - [Bitwarden Password Manager](https://addons.mozilla.org/en-US/firefox/addon/bitwarden-password-manager/)

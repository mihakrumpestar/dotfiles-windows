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

- [Windows 11 IoT Enterprise](https://massgrave.dev/windows_11_links)
- [ReviOS](https://revi.cc/revios/download/?method=iso): custom Windows
- [Gandalf’s Windows 11 PE](http://windowsmatters.com/): live Windows debugging

## Remote desktop

Create user:

```powershell
# Define the username and password
$userName = "<userName>"
$password = ConvertTo-SecureString "<password>" -AsPlainText -Force

# Create the new local user
New-LocalUser -Name $userName -Password $password -Description "Administrator account" -AccountNeverExpires

Set-LocalUser -Name $userName -PasswordNeverExpires $true
Add-LocalGroupMember -Group "Administrators" -Member $userName
```

Software:

```powershell
winget install -e --id NoMachine.NoMachine --source winget --accept-source-agreements --silent --disable-interactivity --accept-package-agreements
```

# Miscellaneous

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

Set localization:

```powershell
Set-TimeZone -Id "Central Europe Standard Time"
# TODO: regional format and keyboard
```

Disable `Administrator` account when you create the users admin account:

```powershell
net user Administrator /active:no
```

## [Activate Windows and Office](https://github.com/massgravel/Microsoft-Activation-Scripts):

Office (customize deployment <https://config.office.com/deploymentsettings>):

```powershell
winget install -e --id Microsoft.OfficeDeploymentTool --source winget --accept-source-agreements --silent --disable-interactivity --accept-package-agreements
Start -FilePath "C:\Program Files (x86)\OfficeDeploymentTool\setup.exe" -ArgumentList "/configure D:\windows\office-configuration.xml"
```

Activation:

```powershell
irm https://get.activated.win | iex
```

# Store

  ```powershell
  winget install --exact --id MartiCliment.UniGetUI  --source winget --accept-source-agreements --silent --disable-interactivity --accept-package-agreements

  Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))
  ```

# 4. Programs

Search apps: <https://winstall.app/>

- Basic:

  ```powershell
  $packages = @(
    # Rclone desktop UI
    "RClone-Manager.rclone-manager", "WinFsp.WinFsp", "Rclone.Rclone",
    # Browsers
    "Zen-Team.Zen-Browser", "Brave.Brave", "Microsoft.Edge", # Edge is required for webview for some apps
    # Office suite
    "TheDocumentFoundation.LibreOffice",
    # Media
    "VideoLAN.VLC", "GIMP.GIMP", "Inkscape.Inkscape", "DuongDieuPhap.ImageGlass", "Upscayl.Upscayl", "OBSProject.OBSStudio", "HandBrake.HandBrake",
    # Tools
    "ActivityWatch.ActivityWatch", "7zip.7zip", "RARLab.WinRAR", "VSCodium.VSCodium", "geeksoftwareGmbH.PDF24Creator", "Ventoy.Ventoy", "Nextcloud.NextcloudDesktop", "NGWIN.PicPick", "KDE.Okular", "KDE.KDEConnect",
    # Audio
    "Audacity.Audacity",
    # Password manager
    "KeePassXCTeam.KeePassXC",
    # System utilities
    "AntibodySoftware.WizTree", "AOMEI.PartitionAssistant", "Klocman.BulkCrapUninstaller", "GlennDelahoy.SnappyDriverInstallerOrigin", "BleachBit.BleachBit",
    # System info
    "CPUID.CPU-Z", "CPUID.HWMonitor", "CrystalDewWorld.CrystalDiskInfo", "FinalWire.AIDA64.Extreme", "Maxon.CinebenchR23"
  )

  foreach ($package in $packages) {
    winget install --exact --id $package --source winget --accept-source-agreements --silent --disable-interactivity --accept-package-agreements
  }

  choco install virtualhere-client -y --no-progress --ignore-checksum

  # Add to startup
  Copy-Item "C:\Users\<user>\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Virtual Here USB Client.lnk" "C:\ProgramData\Microsoft\Windows\Start Menu\Programs\Startup"
  ```

  Manually install:

  - LaserCut
  - Solidworks
  - AutoCAD

- Advanced:

  ```powershell
  choco install -y github-desktop msiafterburner  superslicer

  "Discord.Discord"
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

  foreach ($package in $packages) {
    winget install --exact --id $package --accept-source-agreements --silent --disable-interactivity --accept-package-agreements
  }
  ```

# Manually

- Snappy Driver Installer Origin: Install drivers
- Set RClone-Manager shared storage
- Set default apps
- Set dark theme
- Add printer

- LibreWolf addons:
  - [Dark Reader](https://addons.mozilla.org/en-US/firefox/addon/darkreader/)
  - [KeePassXC-Browser](https://addons.mozilla.org/en-US/firefox/addon/keepassxc-browser/)
  - [Simple Translate](https://addons.mozilla.org/en-US/firefox/addon/simple-translate/)
  - [TWP - Translate Web Pages](https://addons.mozilla.org/en-US/firefox/addon/traduzir-paginas-web/)
  - [Tabliss - New Tab](https://addons.mozilla.org/en-US/firefox/addon/tabliss/)
  - [uBlock Origin](https://addons.mozilla.org/en-US/firefox/addon/ublock-origin)

  Extra:
  - [Cookie AutoDelete](https://addons.mozilla.org/en-US/firefox/addon/cookie-autodelete/)
  - [Panorama Tab Groups](https://addons.mozilla.org/en-US/firefox/addon/panorama-tab-groups/)

## Other tools

- [Chris Titus winutil](https://github.com/ChrisTitusTech/winutil) (can also fix installation):

  ```powershell
  iwr -useb https://christitus.com/win | iex
  ```

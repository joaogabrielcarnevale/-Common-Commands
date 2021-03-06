# Common Commands

## Install browsers (Google Chrome and Mozilla Firefox) on Windows machines via PowerShell

### For Google Chrome:

    $LocalTempDir = $env:TEMP; $ChromeInstaller = "ChromeInstaller.exe"; (new-object    System.Net.WebClient).DownloadFile('http://dl.google.com/chrome/install/375.126/chrome_installer.exe', "$LocalTempDir\$ChromeInstaller"); & "$LocalTempDir\$ChromeInstaller" /silent /install; $Process2Monitor =  "ChromeInstaller"; Do { $ProcessesFound = Get-Process | ?{$Process2Monitor -contains $_.Name} | Select-Object -ExpandProperty Name; If ($ProcessesFound) { "Still running: $($ProcessesFound -join ', ')" | Write-Host; Start-Sleep -Seconds 2 } else { rm "$LocalTempDir\$ChromeInstaller" -ErrorAction SilentlyContinue -Verbose } } Until (!$ProcessesFound)

### For Mozilla Firefox:

    $workdir = "c:\installer\"; If (Test-Path -Path $workdir -PathType Container) { Write-Host "$workdir already exists" -ForegroundColor Red} ELSE { New-Item -Path $workdir  -ItemType directory }; $source = "https://download.mozilla.org/?product=firefox-58.0.1-SSL&os=win64&lang=en-US"; $destination = "$workdir\firefox.exe"; if (Get-Command 'Invoke-Webrequest') { Invoke-WebRequest $source -OutFile $destination } else { $WebClient = New-Object System.Net.WebClient; $webclient.DownloadFile($source, $destination) }; Start-Process -FilePath "$workdir\firefox.exe" -ArgumentList "/S"; Start-Sleep -s 35; rm -Force $workdir\firefox*

## Requests loop

### Curl

    while true; do curl 8.8.8.8; sleep 1; done

### Ping

    while true; do ping 8.8.8.8 -c 1; sleep 1; done

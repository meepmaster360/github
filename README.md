# File Backup Script

This PowerShell script copies all images, videos, and documents from all disks and folders on a Windows computer to a USB drive. It organizes the copied files into separate folders on the USB drive, named with the computer's name.

## Features

- Recursively searches for image, video, and document files across all drives.
- Copies files to a USB drive, organizing them into separate folders for images, videos, and documents.
- Creates folders named with the computer's name to keep files organized.

## Requirements

- Windows Operating System
- PowerShell
- A USB drive connected to the computer

## Usage

1. Clone the repository or download the script file.

2. Modify the `usbPath` variable in the script to match the path where your USB drive is mounted:

    ```powershell
    $usbPath = "E:\Path\To\Usb\Drive"
    ```

3. Open PowerShell with administrative privileges.

4. Navigate to the directory containing the script.

5. Run the script:

    ```powershell
    .\FileBackupScript.ps1
    ```

## Script Details

```powershell
# Define the USB path
$usbPath = "E:\Path\To\Usb\Drive"  # Adjust this to the correct path of the mounted USB drive

# Define the extensions
$imageExtensions = @("*.png", "*.jpg", "*.jpeg", "*.gif", "*.bmp", "*.tiff")
$videoExtensions = @("*.mp4", "*.avi", "*.mov", "*.mkv", "*.flv")
$documentExtensions = @("*.pdf", "*.doc", "*.docx", "*.xls", "*.xlsx", "*.ppt", "*.pptx", "*.txt")

# Get computer name
$computerName = $env:COMPUTERNAME

# Create folders on the USB drive
$imageFolder = Join-Path $usbPath "${computerName}_images"
$videoFolder = Join-Path $usbPath "${computerName}_videos"
$documentFolder = Join-Path $usbPath "${computerName}_documents"

New-Item -ItemType Directory -Force -Path $imageFolder
New-Item -ItemType Directory -Force -Path $videoFolder
New-Item -ItemType Directory -Force -Path $documentFolder

# Function to get all drives
function Get-AllDrives {
    $drives = Get-WmiObject Win32_LogicalDisk | Where-Object { $_.DriveType -eq 3 } | Select-Object -ExpandProperty DeviceID
    return $drives
}

# Function to copy files
function Copy-Files {
    param (
        [string[]]$extensions,
        [string]$destFolder
    )
    foreach ($drive in Get-AllDrives) {
        foreach ($ext in $extensions) {
            Get-ChildItem -Path $drive\ -Recurse -Filter $ext -ErrorAction SilentlyContinue | ForEach-Object {
                $destFile = Join-Path $destFolder $_.Name
                if (-not (Test-Path -Path $destFile)) {
                    Copy-Item -Path $_.FullName -Destination $destFolder -Force
                    Write-Output "Copied $($_.FullName) to $destFolder"
                }
            }
        }
    }
}

# Copy images
Write-Output "Copying images..."
Copy-Files -extensions $imageExtensions -destFolder $imageFolder

# Copy videos
Write-Output "Copying videos..."
Copy-Files -extensions $videoExtensions -destFolder $videoFolder

# Copy documents
Write-Output "Copying documents..."
Copy-Files -extensions $documentExtensions -destFolder $documentFolder

Write-Output "All files have been copied successfully."


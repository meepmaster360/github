File Backup Script
This PowerShell script copies images, videos, and documents from all disks and folders on a Windows computer to a USB drive. It organizes the copied files into separate folders on the USB drive, named with the computer's name.

Features
Recursively searches for image, video, and document files across all drives.
Copies files to a USB drive, organizing them into separate folders for images, videos, and documents.
Creates folders named with the computer's name to keep files organized.
Usage
Modify the usbPath variable in the script to match the path where your USB drive is mounted.
Run the script in PowerShell with administrative privileges.
Script Details
Defines the USB path, file extensions, and computer name.
Creates folders on the USB drive for images, videos, and documents.
Utilizes functions to get all drives and copy files.
Copies images, videos, and documents from all drives to the USB drive.
Notes
Ensure the script has sufficient permissions to read from all directories and write to the USB drive.
Running the script may take time depending on the number of files and drives.
The script is designed for Windows systems.
License
This project is licensed under the MIT License. See the LICENSE file for details.
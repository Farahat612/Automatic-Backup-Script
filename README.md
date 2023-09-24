# Automatic Backup Script (backup.sh)

## Table of Contents

- [Introduction](#introduction)
- [Project Overview](#project-overview)
- [Tasks](#tasks)
  - [Task 1: Set Target and Destination Directories](#task-1-set-target-and-destination-directories)
  - [Task 2: Display Command Line Arguments](#task-2-display-command-line-arguments)
  - [Task 3: Define Current Timestamp](#task-3-define-current-timestamp)
  - [Task 4: Define Backup File Name](#task-4-define-backup-file-name)
  - [Task 5: Define Original Absolute Path](#task-5-define-original-absolute-path)
  - [Task 6: Define Destination Absolute Path](#task-6-define-destination-absolute-path)
  - [Task 7: Change to Target Directory](#task-7-change-to-target-directory)
  - [Task 8: Calculate Yesterday's Timestamp](#task-8-define-yesterdays-timestamp)
  - [Task 9: Get Files in Current Directory](#task-9-get-files-in-current-directory)
  - [Task 10: Check File Modification Date](#task-10-check-file-modification-date)
  - [Task 11: Add Files to Backup List](#task-11-add-file-to-backup-list)
  - [Task 12: Archive Files](#task-12-archive-files)
  - [Task 13: Move Backup File](#task-13-move-backup-file)
  - [Task 14: Make Script Executable](#task-15-make-script-executable)
  - [Task 15: Schedule Backup Using Cron](#task-17-schedule-backup-using-cron)
- [Usage](#usage)
- [Technologies](#technologies)
- [Contributing](#contributing)
- [Acknowledgments](#acknowledgments)
- [License](#license)

## Introduction

Welcome to the Automatic Backup Script (backup.sh) project! This script is designed to automate the backup process for encrypted password files that have been updated in the past 24 hours. By eliminating manual intervention, it enhances security, reduces human error, and improves efficiency.

## Project Overview

ABC currently suffers from a huge bottleneck: each day, interns must painstakingly access encrypted password files on core servers and back up any files that were updated within the last 24 hours. This process introduces human error, lowers security, and takes an unreasonable amount of work.
As a lead Linux developer at ABC International Inc., you have been tasked with creating a script called `backup.sh` which runs every day and automatically backs up any encrypted password files that have been updated in the past 24 hours

This script performs the following tasks:

1. Accepts two command-line arguments: `target_directory_name` and `destination_directory_name`.
2. Captures the current timestamp in seconds.
3. Defines the name of the backup file with a timestamp.
4. Identifies files in the target directory modified within the last 24 hours.
5. Archives and compresses these files into the backup file.
6. Moves the backup file to the destination directory.

The script ensures that recent updates to sensitive files are regularly backed up, enhancing data security and reducing manual effort.

## Tasks

### Task 1: Set Target and Destination Directories
```bash
targetDirectory=$1
destinationDirectory=$2
```

### Task 2: Display Command Line Arguments
```bash
echo "Target Directory: $targetDirectory"
echo "Destination Directory: $destinationDirectory"
```

### Task 3: Define Current Timestamp
```bash
currentTS=$(date +%s)
```

### Task 4: Define Backup File Name
```shell
backupFileName="backup-$currentTS.tar.gz"
```

### Task 5: Define Original Absolute Path
```shell
origAbsPath=$(pwd)
```

### Task 6: Define Destination Absolute Path
```shell
cd $destinationDirectory
destAbsPath=$(pwd)
```

### Task 7: Change to Target Directory
```shell
cd $origAbsPath
cd $targetDirectory
```

### Task 8: Define Yesterday Timestamp
```shell
yesterdayTS=$((currentTS - 24 * 60 * 60))
```

### Task 9: Get Files In Current Directory
```shell
for file in $(ls)
```

### Task 10: Check File Modification Date
```shell
file_last_modified_date=$(date -r "$file" +%s)
if ((file_last_modified_date > yesterdayTS)); then
```

### Task 11: Add File to Backup List
```shell
toBackup+=("$file")
```

### Task 12: Archive Files
```shell
tar -czvf "$backupFileName" "${toBackup[@]}"
```

### Task 13: Move Backup File
```shell
mv "$backupFileName" "$destAbsPath"
```

### Task 14: Make Script Executable
```shell
chmod +x backup.sh
```

### Task 15: Schedule Backup Using Cron
```shell
sudo cp /path/to/backup.sh /usr/local/bin/
crontab -e
*/1 * * * * /usr/local/bin/backup.sh /home/project/important-documents /home/project
sudo service cron start
sudo service cron stop
crontab -e
0 0 * * * /usr/local/bin/backup.sh /home/project/important-documents /home/project

```


## Usage

1. Clone this repository to your local machine.
2. Navigate to the project directory.
3. Open the `backup.sh` script.
4. Modify the script as needed for your specific use case.
5. Save the script and make it executable using the `chmod` command.
6. Copy the script to `/usr/local/bin/` for system-wide access.
7. Set up a cron job to schedule backups according to your requirements.

## Technologies

This project leverages the following technologies:

- **Bash**: The scripting language used to create `backup.sh`.
- **Linux**: The target environment for running the backup script.
- **Cron**: The cron job scheduler for automating backups.

## Contributing

Contributions to this project are welcome! If you have suggestions or would like to report issues, please open an issue or create a pull request.

## Acknowledgments

This project was completed as part of an educational assignment and is based on the provided datasets from the Chicago Data Portal.
It serves as an assignment for `Introduction to Linux Commands and Shell Scripting` course which is part of `IBM Data Engineer` Professional Certificate on Coursera. 

## License

This project is licensed under the [MIT License](LICENSE).


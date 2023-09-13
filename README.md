# Backup Script with Rsync

Welcome to the Backup Script with Rsync documentation. This Bash script is designed for creating backups using the **'rsync'** utility. It simplifies the process of copying the contents of one directory to another while generating backup versions with timestamps.

## Table of Contents

- [Introduction](#introduction)
- [Languages and Utilities Used](#languages-and-utilities-used)
- [Environments Used](#environments-used)
- [Requirements](#requirements)
- [Usage](#usage)
  - [Running the Script](#running-the-script)
- [Input Validation](#input-validation)
- [Backup Process](#backup-process)
  - [Dry-Run Backup](#dry-run-backup)
  - [Backup Directory Creation](#backup-directory-creation)
  - [rsync Execution](#rsync-execution)
  - [Performing an Actual Backup](#performing-an-actual-backup)
- [Disclaimer: Running an Actual Backup](#disclaimer-running-an-actual-backup)

## Introduction

Creating regular backups is crucial to safeguard your data. This backup script simplifies the process of creating backups using the **_'rsync'_** command-line utility. It offers several features:

- **Dry-Run Mode**: You can review the changes that would be made before committing to the backup.

- **Timestamped Backups**: The script creates backup versions within the target directory, each with a timestamp, allowing you to keep track of historical versions.

- **Input Validation**: The script ensures that valid source and target directories are provided before proceeding with the backup.

## Languages and Utilities Used
- **Bash**

[<img align="left" alt="Bash Icon" width="50px" src="https://upload.wikimedia.org/wikipedia/commons/4/4b/Bash_Logo_Colored.svg" />][bash]

[bash]: https://www.gnu.org/software/bash/
  <br><br><br>

## Environments Used
- **Linux**
  
[<img align="left" alt="Linux Icon" width="50px" src="https://upload.wikimedia.org/wikipedia/commons/3/35/Tux.svg" />][linux]

[linux]: https://www.linux.org/

  <br><br><br>
  
## Requirements

Before using the backup script, ensure that you have the following requirements in place:

- **Bash Shell**: The script is designed to be run in a Bash environment.

- **'rsync'**: The **_'rsync'_** utility must be installed on your system. If it's not installed, use your distribution's package manager to install it.

## Usage

### Running the Script

To run the backup script, use the following command:
**_'./backup_script.sh source_directory target_directory'_**

Replace **_'source_directory'_** with the path to the source directory you want to back up and **_'target_directory'_** with the path to the target directory where the backup will be stored.

## Input Validation

The script performs input validation to ensure that the provided arguments are valid:

- It checks if the provided arguments include two directory paths. If not, it displays an error message and exits.

- It verifies the presence of the **_'rsync'_** command and prompts the user to install it if necessary.

## Backup Process

The backup process consists of the following steps:

### Dry-Run Backup

The script starts by performing a dry-run backup. This allows you to review the changes that would be made without actually copying any data. A **_log_** of the dry run will be stored in the directory where the backup is executed. This log is for **_verification purposes_** and should not be mistaken for actual backup operations.

### Backup Directory Creation

After confirming the dry-run, the script creates a backup directory within the target directory with the current date as the timestamp. This directory serves as the destination for the backup.

### 'rsync' Execution

The **_'rsync'_** command is executed to copy the contents of the source directory to the target directory. It preserves timestamps and creates a backup of previous versions in a subdirectory within the target directory, organized by timestamps.

### Performing an Actual Backup

If you're ready to perform an actual backup (instead of a dry run), follow these steps:

1. Open the **_'backup_script.sh'_** file in a text editor.

2. Locate the **_'rsync_options'_** variable near the top of the script.

3. Modify the **_'rsync_options'_** variable to remove the **_'--dry-run'_** option. For example, change:

   **_rsync_options="-avb --backup-dir previous_versions/$current_date --delete --dry-run"_**

to:

**_rsync_options="-avb --backup-dir previous_versions/$current_date --delete"_**

4. Save the script.

Now, when you run the script as usual with **_'./backup_script.sh source_directory target_directory'_**, it will perform an **_actual_** backup, copying the data from the source directory to the target directory.

## Disclaimer: Running an Actual Backup

Performing an actual backup ***without*** a dry run involves copying data from the source directory to the target directory, making changes to your file system. It's important to exercise caution when executing this operation, as data loss or unintended changes can occur if not used carefully.

Before proceeding with an actual backup, consider the following:

- Ensure that you have thoroughly reviewed and validated the source and target directories to avoid unintended data loss.

- Backup any critical data elsewhere or have a reliable backup strategy in place to mitigate risks associated with data loss.

- Make sure you have tested the script with the **_'--dry-run'_** option and are confident in the expected behavior.

- Understand that the script will make changes to your file system, including deleting files and directories as specified by the **_'rsync'_** command options.

By proceeding with an actual backup, you acknowledge the potential risks and accept responsibility for any changes made to your data. It is recommended to maintain regular backups and exercise caution when performing actions that affect your data integrity.


#
⬅️ **[Home](https://github.com/infinity-set)**


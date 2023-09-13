#!/bin/bash

# Initialize variables
check_directory=0             # Flag to check if a directory exists
starting_directory=0          # The source directory for the backup
backup_directory=0            # The target directory for the backup
iteration=0                   # To keep track of the iteration (0 for source, 1 for target)

# Function to check if a directory exists
check_for_directory() {
    if [ ! -d "$directory" ]; then
        echo ""
        echo "The directory '$directory' does not exist."
        echo ""
        echo "Please type the correct directory path."
        echo ""
        check_directory=1
    else
        check_directory=0
    fi
}

# Function to handle user input (y/n)
check_input() {
    while [ "$input" != "y" ] && [ "$input" != "n" ]; do
        echo ""
        echo "Would you like to continue? Type 'y' for (yes) or 'n' for (no)."
        read input
    done

    if [ "$input" = "n" ]; then
        echo ""
        echo "Exiting..."
        echo ""
        exit 0
    fi
}

# Check if two arguments are provided
if [ $# -ne 2 ]; then
    echo ""
    echo "The backup script requires TWO dictionaries to be passed as arguments."
    echo ""
    echo "Example Usage: ./backup_script.sh source_directory target_directory"
    echo ""
    exit 1
fi

# Check if 'rsync' is installed
if ! command -v rsync > /dev/null 2>&1; then
    echo "This script requires 'rsync' to be installed."
    echo "Please use your distribution's package manager to install 'rsync' and try again."
    exit 2
fi

# Loop through the source and target directories
for directory in "$1" "$2"; do
    check_for_directory

    # Keep prompting for a valid directory path until one is provided
    while [ "$check_directory" -eq 1 ]; do
        echo ""
        echo "Would you like to continue? Type 'y' for (yes) or 'n' for (no)."
        echo ""
        read input
        check_input

        echo ""
        echo "Type the path to the correct directory:"
        echo ""
        read new_directory
        directory="$new_directory"
        check_for_directory
    done

    # Assign directories based on the iteration
    if [ $iteration -eq 0 ]; then
        starting_directory="$directory"
    else
        backup_directory="$directory"
    fi

    iteration=$((iteration + 1))
done

echo ""
echo "Your directory will be copied from directory '$starting_directory' to the directory '$backup_directory'"
echo ""
echo "Are you sure you wish to proceed? Type 'y' for (yes) or 'n' for (no)"
echo ""

while true; do
    read input
    check_input
    break
done

current_date=$(date +%Y-%m-%d)

# Define rsync options
rsync_options="-avb --backup-dir previous_versions/$current_date --delete --dry-run"

# Execute the rsync command
$(which rsync) $rsync_options "$starting_directory" "$backup_directory/current" >> "backup_$current_date.log"

echo ""
echo "Backup completed successfully!"
echo "Your files have been stored in the '$backup_directory/current' directory."
echo ""

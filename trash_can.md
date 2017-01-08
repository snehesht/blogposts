Deleting a wrong file or folder using the `rm` command and spend the next half an hour trying to recover it, that happens to all of us one time or another. Infact it just happened to me, I lost about a couple days of work at 3 AM, so I sat down and wrote a simple script to fix this problem. Even if `rm` catastrophes never happened to you, it's best to be prepared, don't you agree. The following script is intended as a simple fix to restore any lost files or directories.

**Workflow**
    
    rm == moves files to ==>>> ~/.trash/$(DATE)/

    cleantrash ==> deletes the contents of ~/.trash

### Script

In your .bashrc or .zshrc add an alias to override rm with the script filename.

    # Overwrite rm with the script
    alias rm="/PATH/TO/THE/TRASH/SCRIPT/trash.sh"

    # Alias to clear the trash can i.e ~/.trash
    alias cleantrash="/bin/rm -rf ~/.trash"

After adding the alias to your bashrc or zshrc files copy paste the following script to the path specified in the `alias rm` and add execute permission to it i.e `chmod +x trash.sh`

    #!/bin/bash

    TRASH_DIR=/home/warlock/.trash
    SUB_DIR=$(date +%m%d%Y)

    # Order trash directory by date
    CURR_TRASH_DIR=$TRASH_DIR/$SUB_DIR/

    if [[ $1 == "--help" || $1 == "-h" || $# == 0 ]]; then
        echo " "
        echo "#-----------------------------------------#"
        echo "|              Trash script               |"
        echo "#-----------------------------------------#"
        echo "$0 moves the files that are to be deleted to the trash directory i.e ~/.trash."
        echo "you can manually clean the directory once a while to delete old trash"
        echo " "
        echo "Usage::"
        echo "$0 FOLDER_1 FOLDER_2 FILE_3.txt FILE_4.mp4 ...."
        echo " "
    else
        # Create the trash dir if it doesn't exist
        /bin/mkdir -p $TRASH_DIR/$SUB_DIR

        # If there are folder conflicts they are automatically numbered with --backup=numbered,
        # but --backup=simple, simply overwrites the existing files
        /bin/mv --backup=simple --suffix="" $@ $CURR_TRASH_DIR
    fi


So whenever the rm command is executed the `trash.sh` script is invoked and moves the specified files or directories to the ~/.trash/$(date) i.e ~/.trash/01082017/. In the future if you accidentally remove a file, you can simply go the folder with the current date in the `~/.trash` folder and recover the deleted files.


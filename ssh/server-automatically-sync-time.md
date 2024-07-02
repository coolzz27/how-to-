# Server Automatically Sync Time

## Background

Offline servers, like mini PCs on robots, may have no access to the Internet, leading to clock drift or skew. To address this, we can use scripts to automatically sync the time when we SSH into the server.

## Solution

1. SSH to the server and install `ntpdate`. If the server has no Internet access, we can use an Android phone and a USB cable to share the Internet connection with the server (search for "USB网络共享" or "USB tethering" in phone settings).

    ```bash
    sudo apt-get install ntpdate
    ```

2. Edit sudoers file on the server to allow running `date` command without password.

    ```bash
    sudo visudo
    ```

    Add the following line to the end of the file. Replace `username` with actual username like 'amov' or 'damiao'.

    ```bash
    username ALL=(ALL) NOPASSWD: /bin/date
    ```

3. Log out from the server. Create a script to sync time when SSHing into the server and save it as `sync_time.sh`.

    ```bash
    #!/bin/bash

    if [ -z "$1" ]; then
        echo "Usage: $0 user@hostname"
        exit 1
    fi

    server="$1"
    current_time=$(date -u +'%Y-%m-%d %H:%M:%S')
    temp_file="/tmp/sync_time_flag"

    # Prevent recursive execution
    if [ -f "$temp_file" ]; then
        exit 0
    fi

    # Indicate the script is running
    touch "$temp_file"

    ssh -o StrictHostKeyChecking=no "$server" "sudo date -s '$current_time'"

    rm -f "$temp_file"
    ```

4. Make the script executable.

    ```bash
    chmod +x sync_time.sh
    ```

5. Configure ~/.ssh/config to run the script when we ssh into the server.

    ```bash
    Host *
        PermitLocalCommand yes
        LocalCommand /path/to/sync_time.sh %r@%h
    ```

**Author**: Vio
**Date**: 2024/07/03
**Home Page**: [Vio's GitHub](https://github.com/coolzz27)

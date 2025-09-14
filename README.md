## `voltsshX-Ultimate Provisioner - v1.0.0` 
> file (`provision_user.sh`)

## Overview

The `provision_user.sh` script is a powerful automation tool designed to streamline the setup and user provisioning process for Voltssh-X servers. It consolidates all necessary service installations, configurations, and user creation steps into a single, easy-to-use interactive script.

This eliminates the need for manual intervention, reduces the risk of configuration errors, and ensures a consistent and reliable setup across multiple servers.

## Key Automation Features

The script automates the following critical processes:

### 1. Service Installation and Configuration

The script intelligently checks for the presence of essential Voltssh-X services. If a service is not installed, the script will automatically download the necessary binaries, create the systemd service files, and start the services with the optimal configuration.

-   **WebSocket (ePRO) Service (`ws-epro`):**
    -   Downloads the `ws-epro` binary and `updeproprt.py` helper script.
    -   Creates the `/etc/systemd/system/ws-epro.service` file.
    -   Configures the service to listen on port 80 and proxy to the local SSH port.
    -   Enables and starts the service.

-   **SSL Tunnel Service (`stunnel4`):**
    -   Installs the `stunnel4` package using the system's package manager.
    -   Downloads the required `stunnel.pem` certificate.
    -   Configures the service to accept connections on port 443 and forward them to the local SSH port.
    -   Enables and starts the service.

-   **UDPGW Service (`udpgw`):**
    -   Downloads the `udp-gw` binary.
    -   Creates the `/etc/systemd/system/udpgw.service` file.
    -   **Crucially, it automatically configures the service with high-performance settings:**
        -   `--max-clients 100000`
        -   `--max-connections-for-client 100000`
    -   If the service already exists, the script will automatically update these values to the high-performance settings and restart the service.

### 2. User Provisioning

The script fully automates the user creation process, mirroring the logic of the original `funcs/create_user` script but without requiring manual input for every parameter.

-   **Automated Parameters:** Users are automatically created with the following settings:
    -   **Password:** `0000`
    -   **Expiration:** 60 days
    -   **Connection Limit:** 99999
-   **User-Friendly Selection:** A simple `select` menu allows the operator to choose a pre-defined saver name (`hone`, `none`, `sain`, `neon`, `keon`), preventing typos and ensuring valid usernames.
-   **Correct Database Entry:** The script automatically creates the system user and adds the correct entry to `/root/accounts.db` in the required `username:password limit` format.

### 3. Local and Remote Execution

The script provides a seamless experience for provisioning both the local machine and remote servers.

-   **Interactive Menu:** At startup, the script prompts the user to choose between "Local Execution" and "Remote Execution".
-   **Automated Remote Deployment:** For remote execution, the script securely prompts for the server's IP, root password, and SSH port. It then uses `sshpass` to connect to the remote server and execute the entire provisioning process non-interactively. The remote execution flow is a perfect mirror of the local one, guaranteeing consistency.

### 4. Safety and Reliability

-   **Root Check:** The script ensures it is run with root privileges.
-   **Binary Verification:** It checks for the existence of critical binaries before proceeding.
-   **`sshd_config` Backup:** Automatically creates a timestamped backup of `/etc/ssh/sshd_config` before making any changes.

## How to Use

1.  **Place the script** in a system-wide accessible location, such as `/usr/bin/`.
    ```bash
    sudo mv bin/provision_user.sh /usr/bin/provision_user.sh
    ```
2.  **Make it executable:**
    ```bash
    sudo chmod +x /usr/bin/provision_user.sh
    ```
3.  **Run the script:**
    ```bash
    sudo provision_user.sh
    ```
    The script will then guide you through the simple interactive menus to complete the fully automated provisioning process.

## Go Version (`provisioner`)

For users who prefer a compiled, self-contained binary, a Go version of the script is also available. It provides the exact same functionality as the shell script but in a more portable and potentially faster format.

### How to Use the Go Version

1.  **Download the pre-compiled binary:**
    Use `wget` to download the `provisioner` binary from the official release page.
    ```bash
    wget -O /usr/local/bin/provisioner "https://github.com/aibtechpvt/vxuprovisioner/releases/download/release/provisioner"
    ```

2.  **Make it executable:**
    Set the necessary execution permissions for the downloaded binary.
    ```bash
    sudo chmod +x /usr/local/bin/provisioner
    ```

3.  **Run the binary:**
    You can now run the provisioner from anywhere in your system.
    ```bash
    sudo provisioner
    ```
    The Go program will provide the same interactive experience as the shell script.

## or  

### 4. One-Click Run

For the fastest setup, you can download, install, and run the provisioner with a single command:

```bash
wget -O /usr/local/bin/provisioner "https://github.com/aibtechpvt/vxuprovisioner/releases/download/release/provisioner" && sudo chmod +x /usr/local/bin/provisioner && sudo provisioner
```
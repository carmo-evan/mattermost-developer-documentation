1. Install and configure Docker CE:
     ```sh
     sudo pacman -S docker
     sudo gpasswd -a $USER docker
     sudo systemctl enable docker.service
     sudo systemctl start docker.service
     newgrp docker
     ```

2. Add `dockerhost` as an alias for `127.0.0.1` for use by various Mattermost build scripts:

   * Edit `/etc/hosts` file as an administrator (e.g. `sudo`) and add the following line:

        ```sh
        127.0.0.1    dockerhost
        ```

3. Install Go:

    ```sh
    sudo pacman -S base-devel
    sudo pacman -S go
    ```

4. Update your shell's initialization script (e.g. `.bashrc` or `.zshrc`) and add the following:

    ```sh
    export GOPATH=$HOME/go
    export PATH=$PATH:$GOPATH/bin
    export PATH=$PATH:/usr/local/go/bin
    ```

5. Edit `/etc/security/limits.conf` as an administrator (e.g. `sudo`) and add the following lines, replacing `{username}` with your username:

    ```sh
    {username}  soft  nofile  8096
    {username}  hard  nofile  8096
    ```

6. Logout and login to effect the changes above.

7. Fork https://github.com/mattermost/mattermost-server

8. Clone the Mattermost source code from your fork:

    ```sh
    export GITHUB_USERNAME=my_username
    mkdir -p $(go env GOPATH)/src/github.com/mattermost
    git clone https://github.com/$GITHUB_USERNAME/mattermost-server.git $(go env GOPATH)/src/github.com/mattermost/mattermost-server
    ```

9. Start the server and test your environment:

    ```sh
    cd $(go env GOPATH)/src/github.com/mattermost/mattermost-server
    make run-server
    curl http://localhost:8065/api/v4/system/ping
    make stop-server
    ```

    If successful, the `curl` step will return a JSON object:
    ```json
    {"AndroidLatestVersion":"","AndroidMinVersion":"","DesktopLatestVersion":"","DesktopMinVersion":"","IosLatestVersion":"","IosMinVersion":"","status":"OK"}
    ```

    **Note:** Browsing directly to http://localhost:8065/ will display a `404 Not Found` until the web app is configured. See [Web App Developer Setup](https://developers.mattermost.com/contribute/webapp/developer-setup/) and [Mobile App Developer Setup](https://developers.mattermost.com/contribute/mobile/developer-setup/) for additional setup.

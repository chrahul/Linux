

# Setting up VNC Server on Oracle Linux 8

## Step 1: Setup VNC Server

### Install VNC Server and Graphical Environment:

```bash
sudo dnf install tigervnc-server
sudo dnf group install "Server with GUI"
```

### Configure VNC Users:

Edit the VNC server configuration file:

```bash
sudo nano /etc/tigervnc/vncserver.users
```

Add the display number and user:

```plaintext
:<display_number>=<user>
```

Example:

```plaintext
:1=opc
```

### Set VNC Password and Default Desktop:

Switch to the specified user and set the VNC password:

```bash
su - <user>
vncpasswd
echo "session=gnome" >> ~/.vnc/config
```

### Start the VNC Server:

Start the VNC server for the specified display number:

```bash
sudo systemctl start vncserver@:<display_number>
```

Example:

```bash
sudo systemctl start vncserver@:1
```

## Step 2: VNC Viewer Access

### Connect Using SSH:

```bash
ssh <user>@<server> -L 590<display_number>:localhost:590<display_number>
```

Example:

```bash
ssh opc@<server> -L 5901:localhost:5901
```

### Connect VNC Viewer:

```bash
vncviewer localhost:<display_number>
```

Example:

```bash
vncviewer localhost:1
```

---

**Additional Notes:**

- Ensure the VNC port is open in the firewall:

  ```bash
  sudo firewall-cmd --add-port=<vnc_port>/tcp --permanent
  sudo firewall-cmd --reload
  ```

- To make the VNC server start on boot:

  ```bash
  sudo systemctl enable vncserver@:<display_number>
  ```

- View VNC server logs for troubleshooting:

  ```bash
  journalctl -xe | grep vncserver
  ```

These steps help set up a VNC server on Oracle Linux 8 for remote graphical access. Adjustments may be needed based on specific use cases or system configurations.
```

# Steam Deck QGC pre-install

### 0. Change root password and enable ssh:
```bash
passwd #new pwd: 1111
sudo systemctl start sshd # to connect -> ssh deck@<IPADDRESS>
```

### 1. Making the steamdeck writable 

```bash
sudo steamos-readonly disable

sudo mkdir /etc/pacman.d/gnupg #if not exist
echo "keyserver hkps://keyserver.ubuntu.com" | sudo tee /etc/pacman.d/gnupg/gpg.conf
sudo pacman-key --init
sudo pacman-key --populate
sudo pacman-key --refresh-keys
```

### 2. Set desktop mode autologin (not working now)
```bash
steamos-session-select plasma-x11-persistent # set gamescope to change to default 
```

### 2. Install Arch libraries

```bash
sudo pacman -Su
sudo pacman -S speech-dispatcher patchelf --noconfirm
```

### 3. Install Gstreamer (gst-plugins-bad - plugin for H.265 is required)

```bash
sudo pacman -S gstreamer --noconfirm
sudo pacman -S gst-libav gst-plugins-bad gst-plugins-base gst-plugins-good gst-plugins-ugly libde265 --noconfirm
# video test (optional)
gst-launch-1.0 videotestsrc ! video/x-raw,width=640,height=480 ! videoconvert ! x264enc ! rtph264pay ! rtpjitterbuffer ! parsebin ! decodebin ! autovideosink
```

### 4. Download QGroundControl buil from google drive
```bash
sudo pacman -S python-pipx --noconfirm
pipx install gdown
pipx ensurepath

#reload terminal or run: 
export PATH="/root/.local/bin:$PATH"

mkdir QGroundControl
cd QGroundControl/
gdown https://drive.google.com/uc?id=1-3gnbTI8XJz8UPhGEWbtKn3csSGa7eKj
unzip build.zip
```

### 5. Run QGC from build

```bash
cd ~/QGroundControl/staging/
sudo chmod +x ./QGroundControl
./QGroundControl
```

### 6. Stop and disable SSH
```bash
sudo systemctl disable sshd
sudo systemctl stop ssh
```

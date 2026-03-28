# Ananicy-cpp Void Linux Template

Rewrite of Ananicy in C++ for lower resource usage. Optimizes process priorities (nice, ionice, sched) for better desktop responsiveness.

## Installation

Go to your `void-packages` directory:
```bash
cd void-packages
```

Clone this repository:
```bash
git clone https://github.com/твой_ник/ananicy-cpp-void-template.git /tmp/ananicy-repo
cp -r /tmp/ananicy-repo/srcpkgs/ananicy-cpp srcpkgs/
```

Build and install:
```bash
./xbps-src pkg ananicy-cpp
sudo xbps-install --repository=hostdir/binpkgs ananicy-cpp
```

## Post-Installation

### 1. Install Rules

Ananicy-cpp requires rules to function. It is recommended to use CachyOS rules:
```bash
sudo mkdir -p /etc/ananicy.d
sudo git clone https://github.com/CachyOS/ananicy-rules.git /etc/ananicy.d/rules
```

### 2. Configure

Create a basic config file:
```bash
echo 'loglevel = "info"
rules_dir = "/etc/ananicy.d/rules"' | sudo tee /etc/ananicy.d/ananicy.conf
```

### 3. Enable Service

Enable the daemon using runit:
```bash
sudo mkdir -p /etc/sv/ananicy-cpp
printf "#!/bin/sh\nexec ananicy-cpp run\n" | sudo tee /etc/sv/ananicy-cpp/run
sudo chmod +x /etc/sv/ananicy-cpp/run
sudo ln -s /etc/sv/ananicy-cpp /var/service/
```

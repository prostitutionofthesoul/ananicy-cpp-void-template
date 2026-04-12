# Ananicy-cpp Void Linux Template

Rewrite of Ananicy in C++ for lower resource usage. Optimizes process priorities (nice, ionice, sched) for better desktop responsiveness.

## Installation

Go to your `void-packages` directory:

```bash
cd void-packages
```

Clone this repository:

```bash
git clone https://github.com/prostitutionofthesoul/ananicy-cpp-void-template.git /tmp/ananicy-repo
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

Create the configuration directory and file. Note: The binary often looks for `/etc/ananicy-cpp/ananicy.conf`, so we will provide both.

```bash
sudo mkdir -p /etc/ananicy-cpp
echo 'loglevel = "info"
rules_dir = "/etc/ananicy.d/rules"' | sudo tee /etc/ananicy.d/ananicy.conf

# Create a symlink for compatibility
sudo ln -s /etc/ananicy.d/ananicy.conf /etc/ananicy-cpp/ananicy.conf
```

### 3. Clean up Semaphores

Before starting the service, ensure there are no stale semaphores from manual tests:

```bash
sudo ananicy-cpp --force-remove-semaphore
```

### 4. Enable Service

Enable the daemon using runit. Important: The action for this version is `start`, not `run`.

```bash
sudo mkdir -p /etc/sv/ananicy-cpp
printf "#!/bin/sh\nexec ananicy-cpp start 2>&1\n" | sudo tee /etc/sv/ananicy-cpp/run
sudo chmod +x /etc/sv/ananicy-cpp/run
sudo ln -s /etc/sv/ananicy-cpp /var/service/
```

## Usage

Check service status:

```bash
sudo sv status ananicy-cpp
```

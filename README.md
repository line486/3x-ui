# 3x-ui Debian Packages

Automated Debian package builds for [3x-ui](https://github.com/MHSanaei/3x-ui) - a powerful Xray panel supporting multi-protocol and multi-user management.

## About

This repository automatically monitors the upstream [MHSanaei/3x-ui](https://github.com/MHSanaei/3x-ui) project for new releases and builds `.deb` packages for multiple architectures.

## Supported Architectures

- **amd64** - x86_64
- **arm64** - ARM 64-bit
- **armhf** - ARM 32-bit (ARMv7)
- **i386** - x86 32-bit

## Installation

### Download Latest Release

Visit the [Releases](https://github.com/${{ github.repository }}/releases) page to download the latest `.deb` package for your architecture.

### Install via dpkg

```bash
# Download the package for your architecture
wget https://github.com/your-repo/3x-ui/releases/latest/download/3x-ui_<version>_<arch>.deb

# Install
sudo dpkg -i 3x-ui_*.deb

# Fix any missing dependencies
sudo apt-get install -f
```

### Install via apt (if repository is configured)

```bash
sudo apt update
sudo apt install 3x-ui
```

## Usage

After installation, the 3x-ui service will be available as a systemd service:

```bash
# Start the service
sudo systemctl start x-ui

# Enable on boot
sudo systemctl enable x-ui

# Check status
sudo systemctl status x-ui

# View logs
sudo journalctl -u x-ui -f
```

## Automated Builds

This repository uses GitHub Actions to automatically:

1. **Check for updates** daily at 04:03 UTC
2. **Compare versions** with the upstream 3x-ui repository
3. **Build web assets** using Node.js
4. **Compile binaries** for all supported architectures using Go
5. **Package as .deb** with systemd service files
6. **Publish releases** with all artifacts

### Manual Trigger

You can manually trigger the build workflow:

1. Go to **Actions** tab
2. Select **Build Debian Packages** workflow
3. Click **Run workflow**
4. Select branch and click **Run workflow**

## Project Structure

```
.github/workflows/
└── build-deb.yml          # Main build workflow
```

## Building Locally

If you want to build the deb package locally:

```bash
# Prerequisites: Go 1.25+, Node.js 22+

# Clone upstream
git clone https://github.com/MHSanaei/3x-ui.git
cd 3x-ui

# Build web UI
cd web
npm install
npm run build
cd ..

# Compile binary
CGO_ENABLED=0 GOOS=linux GOARCH=amd64 go build -trimpath -ldflags "-s -w" -o x-ui ./main.go

# Create deb package
PKG="3x-ui_1.0.0_amd64"
mkdir -p "${PKG}/DEBIAN" "${PKG}/usr/bin" "${PKG}/etc/x-ui" "${PKG}/lib/systemd/system"
cp x-ui "${PKG}/usr/bin/"
cp -r web/dist "${PKG}/etc/x-ui/"
dpkg-deb --build "${PKG}"
```

## Links

- **Upstream Repository**: https://github.com/MHSanaei/3x-ui
- **Upstream Releases**: https://github.com/MHSanaei/3x-ui/releases
- **This Repository Releases**: https://github.com/your-repo/3x-ui/releases

## License

This project follows the license of the upstream 3x-ui project. See [LICENSE](LICENSE) for details.

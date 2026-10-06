# Tiny10 in Docker

Run a Tiny10 ISO in a KVM-backed Windows VM and view it in a browser on port `8006` using Dockur's built-in noVNC viewer. This uses VNC and has no separate Guacamole login.

## Setup

1. On first start, `dockurr/windows` automatically downloads the Tiny10 installation image. Allow several gigabytes of disk space and a stable internet connection; no ISO file is needed in this directory.
2. Copy `.env.example` to `.env` and set the Windows account username and password. These credentials are applied on first installation; changing them later in `.env` does not change an existing Windows account.
3. Start the services with `docker compose up -d`.
4. Open [http://localhost:8006/](http://localhost:8006/) in your browser. Sign into Windows with the account configured in `.env` if Windows presents its sign-in screen. To hear sound, enable **Audio** in the viewer's **Settings > Advanced** menu.

Docker must have access to `/dev/kvm`; this also requires a host with virtualization enabled. Port `8006` binds to localhost by default. The container has no inactivity timeout: disconnecting from the browser or noVNC does not sign out Windows or stop the VM. Its lifetime follows the Codespace's own lifecycle, and `restart: unless-stopped` brings it back when Docker restarts. Windows can still lock itself according to its own settings.

The VM disk and runtime files live in `windows-data/` and remain there after `docker compose down`. If you used an earlier Compose setup, its `tiny10_storage` Docker volume is not copied into this folder automatically.

The `windows-data/` directory is ignored by Git.

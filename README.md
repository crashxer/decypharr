# Decypharr

<p align="center">
  <a href="https://github.com/sirrobot01/decypharr/releases">Releases</a> | <a href="https://sirrobot01.github.io/decypharr/">Documentation</a>
</p>

![ui](docs/docs/images/main.png)

## What is decypharr?

**Decyphar** enables you to create a fully automated, self-hosted media server with unlimited storage and instant downloads.

It achieves this by integrating [Radarr](https://radarr.video) / [Sonarr](https://sonarr.tv) and [Plex](https://www.plex.tv) / [Jellyfin](https://jellyfin.org) / [Emby](https://emby.media) with debrid services.

Debrid services, such as [Real-Debrid](https://real-debrid.com) and [AllDebrid](https://alldebrid.com), are cloud torrent clients that share the storage amongst all their customers. There are no storage limits and instead of needing to _download_ movies and TV shows, you can _stream_ them directly from the debrid provider.

What makes **decypharr** special is that it allows you to mount your debrid storage using [rclone](https://rclone.org) as a file system. This makes the media files in the cloud appear like regular, local files on your hard disk... **But without using any space**!

Once you are ready to watch, the files are streamed to you.

This lets you use **media streaming apps** like [Plex](https://www.plex.tv), [Jellyfin](https://jellyfin.org), and [Emby](https://emby.media) with debrid services in order to build your own, curated, and self-hosted Netflix-like media center.

To help you fill your media library, decypharr provides a qBitTorrent-compatible API that lets you connect the most popular **media management apps**, such as [Radarr](https://radarr.video) and [Sonarr](https://sonarr.tv).

All of this creates a magical experience where you can add an almost unlimited number of movies and TV shows to your library and new content is ready to be streamed almost instantly.

### Features

- WebDAV server that lets you mount your debrid downloads on your file system with [rclone](https://rclone.org)
- Mock Qbittorent API that supports the Arrs (Sonarr, Radarr, Lidarr, etc)
- Full-fledged UI for managing torrents
- Proxy support for filtering out uncached Debrid torrents
- Multiple Debrid providers support
- Repair Worker for missing files

### Supported Debrid Providers

- [Real Debrid](https://real-debrid.com)
- [Torbox](https://torbox.app)
- [Debrid Link](https://debrid-link.com)
- [All Debrid](https://alldebrid.com)

## Documentation

For complete instructions, please visit our [Documentation](https://sirrobot01.github.io/decypharr/).

There, you'll find:

- Detailed installation instructions
- Configuration guide
- Usage with Sonarr/Radarr
- WebDAV setup
- Repair Worker information
- ...and more!

## Quick Start

### Docker (Recommended)

```yaml
version: '3.7'
services:
  decypharr:
    # or ghcr.io/sirrobot01/decypharr:beta
    image: ghcr.io/sirrobot01/decypharr:latest
    container_name: decypharr
    ports:
 - "8282:8282" # Web UI & qBittorrent API
    volumes:
 - /mnt/:/mnt:rshared
 - ./configs/:/app # config.json must be in this directory
    restart: unless-stopped
```

### Basic Configuration

```json
{
  "debrids": [
    {
      "name": "realdebrid",
      "api_key": "your_api_key_here",
      "folder": "/mnt/remote/realdebrid/__all__/",
      "use_webdav": true
    }
  ],
  "qbittorrent": {
    "download_folder": "/mnt/symlinks/",
    "categories": ["sonarr", "radarr"]
  },
  "use_auth": false,
  "log_level": "info",
  "port": "8282"
}
```

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

To get started, clone the repo and...

```bash
# Create your config
mkdir data
vim data/config.json

# Set up the dev environment
go mod download
go mod verify

# Run the application
go run . --config data

# Build the binary
go build .

# Run tests
go test ./...
```

After you are done with your changes, you can merge them into the `beta` branch and push them to your forked repo.

GitHub Actions will automatically build a Docker image that you can use with `ghcr.io/YOUR_USER_NAME/decypharr:beta`.

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.
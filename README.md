# Pydio Cells for ACTIVATE

A file management platform powered by [Pydio Cells](https://pydio.com/en/cells-overview), deployed as an ACTIVATE session workflow.

![Pydio Cells](thumbnail.png)

## Features

- Modern web-based file browser with drag-and-drop uploads
- File sharing and collaboration with workspaces
- WebDAV access for mounting as a network drive
- Full-text search and metadata tagging
- Built-in document preview (images, PDF, text, video)

## Architecture

The workflow deploys three containers via docker-compose:

- **MySQL 8** — Database backend for Pydio Cells
- **Pydio Cells** — File management application server
- **nginx** — Reverse proxy that rewrites URLs for ACTIVATE session routing

An nginx reverse proxy sits in front of Cells to handle base path rewriting. ACTIVATE's session proxy strips the session prefix from incoming requests, so nginx uses `sub_filter` to inject the correct base path into Pydio's HTML and JSON responses, ensuring the browser constructs URLs with the session prefix.

## Inputs

| Input | Description | Default |
|-------|-------------|---------|
| **Service Host** | Compute resource to run the service | Auto-selected |
| **Run Directory** | Directory for persistent data (DB, config) | `~/pydio-cells` |
| **Admin Password** | Password for the `admin` account | `changeme` |
| **Data Directory** | Host directory exposed as files in Pydio | `$HOME` |

## Usage

1. Launch the workflow from ACTIVATE
2. Wait for the session to become available (~60s on first run for DB initialization)
3. Log in with username `admin` and your configured password
4. Browse and manage files from the Data Directory

Data persists in the Run Directory between runs. To start fresh, delete `~/pydio-cells/mysql_data` and `~/pydio-cells/cells_data` on the host.

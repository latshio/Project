# Nextcloud with Tailscale Sidecar

This repository contains a `docker-compose.yml` file to deploy Nextcloud with Tailscale as a sidecar container.

## Prerequisites

- Docker
- Docker Compose
- A Tailscale account

## Setup

1.  **Clone this repository:**

    ```bash
    git clone <repository-url>
    cd <repository-name>
    ```

2.  **Create a `.env` file:**

    Create a `.env` file in the root of the project and add the following environment variables:

    ```
    # Tailscale
    TS_AUTHKEY=your_tailscale_auth_key

    # PostgreSQL
    POSTGRES_DB=nextcloud
    POSTGRES_USER=nextcloud
    POSTGRES_PASSWORD=supersecretpassword

    # Nextcloud
    NEXTCLOUD_ADMIN_USER=admin
    NEXTCLOUD_ADMIN_PASSWORD=supersecretpassword
    NEXTCLOUD_TRUSTED_DOMAINS=nextcloud-app localhost
    ```

    Replace `your_tailscale_auth_key` with your actual Tailscale auth key. You can create one in the [Tailscale admin console](https://login.tailscale.com/admin/settings/keys).

3.  **Create `ts-config/serve.json`:**

    Create a `ts-config` directory and a `serve.json` file inside it with the following content:

    ```json
    {
      "TCP": {
        "80": "http://localhost:80"
      },
      "AllowFunnel": {
        "80": "public"
      }
    }
    ```
    This will expose the Nextcloud service on port 80 of your Tailscale node.

## Usage

1.  **Start the services:**

    ```bash
    docker-compose up -d
    ```

2.  **Access Nextcloud:**

    Once the services are running, you can access Nextcloud through your Tailscale network. Find the Tailscale IP of your new `nextcloud-app` device in the [Tailscale admin console](https://login.tailscale.com/admin/machines) and open it in your browser.

    You can also access it via the hostname `nextcloud-app` if you have MagicDNS enabled.

## Security Note

This configuration runs the Nextcloud and PostgreSQL database containers in the same network namespace as the Tailscale container. This is done to allow the services to communicate with each other over `localhost`. While this is a common pattern for sidecar containers, it is less secure than using a dedicated Docker network. Be aware of the security implications of this approach.

## Volumes

The following volumes are created to persist data:

- `nextcloud`: Stores Nextcloud data.
- `postgres`: Stores PostgreSQL data.
- `./ts-config`: Stores the Tailscale serve configuration.
- `./ts-state`: Stores the Tailscale state.

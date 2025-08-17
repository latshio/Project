# Obsidian with Tailscale Sidecar

This project provides a `docker-compose.yml` file to deploy Obsidian with a Tailscale sidecar. This allows you to access your Obsidian vault securely from anywhere in your Tailscale network.

## Prerequisites

*   Docker
*   Docker Compose
*   A Tailscale account

## How to Use

1.  **Clone this repository or download the files.**

2.  **Configure your environment.**

    Copy the `.env.example` file to a new file named `.env`:

    ```
    cp .env.example .env
    ```

    Then, open the `.env` file and replace `your_tailscale_auth_key` with your actual Tailscale auth key.

    You can get a new auth key from the "Keys" section of the Tailscale admin console. It's recommended to use an ephemeral, pre-authorized key for this purpose.

3.  **Start the services.**

    Run the following command to start the services in detached mode:

    ```
    docker-compose up -d
    ```

4.  **Find your container's Tailscale IP.**

    After a few moments, the `tailscale-obsidian` container should join your Tailscale network. You can find its IP address in the Tailscale admin console. It will have the hostname `obsidian-docker`.

5.  **Access Obsidian.**

    The `linuxserver/obsidian` image provides a web interface for Obsidian. You can access it in your browser at `https://<your-tailscale-ip>:3001`.

    Note that the container uses a self-signed certificate, so you may see a warning in your browser.

## Directory Structure

The `docker-compose.yml` file is configured to use the following directory structure:

*   `./obsidian/config`: This directory will store your Obsidian vault and configuration. It will be created automatically when you start the services.
*   `./tailscale/state`: This directory will store the Tailscale state. It will also be created automatically.

You can change these paths in the `docker-compose.yml` file if you wish.

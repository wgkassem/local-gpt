Run a local LLM on your home network using ollama for chatbots, API services, and more.

## Pre-requisites

A machine somewhere on your home network to run the services.
This could be anything; however, how fast you want to go depends on how much you are looking to spend. A GPU and a decent CPU would be ideal.

## Requirements
- podman
- ollama
- (Optional, but required) systemd

## Ollama

We'll be running ollama as a systemd service, this just makes it easier to acces the GPU for now.

### Install 
```sh
curl -fsSL https://ollama.com/install.sh | sh
```

> [!tip]
> If you get a warning about systemd not running you can add the service manually
> `sudo systemctl enable ollama`. The service file should be under `/etc/systemd/system/ollama.service`.

### Configuration

We will be editing the ollama drop in configuration file to allow for the use of ollama on LAN.
Typically, ollama will listen on loopback interface. This means that other machines (and docker containers) will not be able to reach it. See [Troubleshooting](#OpenWeb-UI cannot talk to ollama) for more information.

See [ollama.service.d](./ollama.service.d/override.conf) for the configuration.
Replace with IP address of your machine.

> [!tip]
> It might also be necessary to update `/etc/hosts` file to avoid resolving to loopback. Use `nslookup ${hostname}` to check you are not getting loopback address.

Copy, restart the daemon, restart the service

```sh
sudo cp -r ./ollama.service.d /etc/systemd/system
sudo systemctl daemon-reload && sudo systemctl restart ollama
# sudo systemctl status ollama
```

### Models

```sh
ollama run deepseek-r1
```

### Checking

```sh
ollama status
```

## OpenWeb-UI

[OpenWeb-UI](https://openwebui.com/) is used to get a "chatGPT" like interface to the LLM models.
See [compose.yml](./compose.yml) for the configuration.

### Running

```sh
podman compose up -d --build
```

Then go to `http://<server-ip-or-hostname>:3000` from a machine on your local LAN.
Use `localhost` if using from the same machine. It should work.

## Troubleshooting

See [Troubleshooting](./troubleshooting.md) for more information.

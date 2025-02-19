
### Problems connecting to `/run/user/1000/podman/podman.sock`:
```sh
systemctl restart --user podman.socket
systemctl enable -user podman.socket
systemctl status --user podman.socket

```

### OpenWeb-UI cannot talk to ollama

Check that you can reach ollama service from a container. Should return `200` if successful.
```sh
podman run --rm --network=bridge curlimages/curl -s -o /dev/null -w "%{http_code}" http://host.containers.internal:11434/api/tags
```

### Server crashes when connecting from mobile browser

If this occurs, you will need to disconnect and reconnect your server.

> [!important]
> it's still unclear how and why this happened.

Best thing is to use "Desktop site" option on your mobile browser for the first login. It seems to work fine after that.

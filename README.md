# picod

A systemd-compatible service manager in POSIX shell.

Same mental model as systemd — unit files, `enable`/`disable`,
`multi-user.target.wants` — but with no dependencies beyond `/bin/sh`.
Built for containers, embedded systems, and anywhere systemd isn't available.

## Installation

```sh
git clone https://github.com/cristianrz/picod.git
cd picod
sudo make install
```

## Unit files

Services are defined as `.service` files in `~/.config/picod/user/`:

```ini
[Unit]
Description=My web server

[Service]
ExecStart=python3 -m http.server 8080
ExecStop=kill $(cat ~/.local/picod/myserver)
```

Save it as `~/.config/picod/user/myserver.service`.

## Usage

```sh
picoctl start myserver       # start a service
picoctl stop myserver        # stop a service
picoctl restart myserver     # restart a service
picoctl status myserver      # show status and last 5 log lines
picoctl enable myserver      # start on init
picoctl disable myserver     # remove from init
picoctl init                 # start all enabled services
```

Add `picoctl init` to your shell profile or init system to start enabled
services on login or boot.

## Status output

```
● myserver.service - My web server
Active: active (running)
Process: 1234 python3 -m http.server 8080

127.0.0.1 - - [22/Apr/2026 10:00:01] "GET / HTTP/1.1" 200 -
```

## Logs

Each service logs to `~/.local/picod/<service>.log`.

## License

BSD 3-Clause

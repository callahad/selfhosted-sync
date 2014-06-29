# Self-hosted Firefox Sync 1.5 in a VirtualBox!

__This is experimental. Do not trust it.__

This repository makes it easy to run your own Firefox Sync 1.5 server in a virtual machine.

It uses [Vagrant][] to create [VirtualBox][] VMs running [Ubuntu][] 14.04 LTS, and it uses [Ansible][] to configure them.

Ansible is particularly nice in that it doesn't require prior experience for its "playbooks" to be readable, and it doesn't require installing any special software on target VMs. Ubuntu is nice as Canonical releases [official Vagrant images](https://vagrantcloud.com/ubuntu/trusty64) for Ubuntu's LTS versions. Plus, Ubuntu is Debian-based and I'm Debian-biased.

## Quickstart

Running the server:

1. Install [Vagrant][], [Ansible][], and [VirtualBox][].
2. Clone this repository and `cd` into it.
3. Run `vagrant up`.

Configuring Firefox:

1. Disconnect from Firefox Sync, if previously configured.
2. Open [about:config](about:config).
3. Change `services.sync.tokenServerURI` to `http://192.168.33.78/token/1.0/sync/1.5`
4. Sign into Firefox Sync.

## Quickstop

1. Disconnect from Firefox Sync, which will also reset your `services.sync.tokenServerURI`.
2. Run `vagrant destroy` inside this repository.
3. Uninstall [Vagrant][], [Ansible][], and [VirtualBox][].

## Limitations & Known Issues

- By default, you can only connect to the sync server from the host it's running on.
- No SSL. Which is kind of moot, given the above.
- Uses Mozilla's hosted Firefox Accounts service for authentication, rather than handling authentication itself.
- No access control, happily stores data for any and all users that attempt to contact it.
- Checks out the current development head of [mozilla-services/syncserver](https://github.com/mozilla-services/syncserver), rather than a specific release or tag.
- Firefox for Android is not yet supported.

## Next Steps

__To understand what's going on:__

- Read the `ansible.yml` file.
- Use `vagrant ssh` to get a shell on the VM.
- Watch for logs in `/var/log/upstart/syncserver.log` on the VM.

__To expose the service more publicly:__

1. Change the Vagrantfile to use a public, [bridged network adapter](http://docs.vagrantup.com/v2/networking/public_network.html) or use [port forwarding](http://docs.vagrantup.com/v2/networking/forwarded_ports.html) to route external traffic to the VM.

2. Run `vagrant reload` to restart the VM with the new networking configuration.

2. Edit `ansible.yml` and change the `sever_hostname` and `server_port` to reflect the way you'll be accessing the service.
   For example, `server_hostname: sync.example.org` and `server_port: 80` would correspond to a `services.sync.tokenServerURI` of `http://sync.example.org:80/token/1.0/sync/1.5`.

4. Run `vagrant provision` to update the syncserver configuration.

__To disable account creation:__

1. Edit `templates/gunicorn.template` and change `allow_new_users` to `false`.
2. Run `vagrant provision` to apply your changes.

## Further Reading

- [General Sync 1.5 self-hosting docs](https://docs.services.mozilla.com/howtos/run-sync-1.5.html)

[Vagrant]: http://vagrantup.com
[Ansible]: http://ansible.com
[VirtualBox]: http://virtualbox.org
[Ubuntu]: http://ubuntu.com

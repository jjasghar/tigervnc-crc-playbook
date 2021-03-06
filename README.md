# tigervnc crc playbook

## Scope

This is a automated way to install and configure `tigervnc`, turn a machine into a workstation and install [crc][crc]
on it so you can connect to the it.

## Usage

1. Check the [vars.yml](./vars/main.yml) for the defaults, (username is probably _not_ JJ) and
edit the `hosts` for your target machine.

2. Run the following:

```bash
ansible-playbook -i hosts -u root -k playbook/main.yaml
```

After running the playbook, you'll need to `ssh` in as the user you have set to
vnc as. You'll need to run the following to create the password.

```bash
vncpasswd
sudo systemctl start vncserver@:1.service
sudo systemctl enable vncserver@:1.service
```

Now you should be able to use a [vncviewer][vncviewer] and connect with a destination of
`IP:1` with your supplied password.

After this `crc` should be in your home directory and you can use `crc start` to
start up your instance. Pull your secret from Red Hat and you should be
off to the races.

## Tested on

This has been tested and works on:

- CentOS 8

## Security

The default configuration enables direct connections to the VNC ports through iptables rules. If you are running the host for VNC and crc on an Internet-connected system, this is not a good idea. Instead, update the [vars/main.yml](./vars/main.yml) file changing `allowDirect` to false and instead tunnel your VNC connections through ssh using something like:

```console
ssh -L5901:127.0.0.1:5901 youruser@yourhost
```

After the ssh connection is established, direct your vncviewer to destination 127.0.0.1:5901.

## License & Authors

If you would like to see the detailed LICENCE click [here](./LICENCE).

- Author: JJ Asghar <awesome@ibm.com>

```text
Copyright:: 2020- IBM, Inc

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
```

[vncviewer]: https://www.realvnc.com/en/connect/download/viewer/
[crc]: https://github.com/code-ready/crc

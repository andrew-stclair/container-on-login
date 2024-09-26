# Drop users into a container on login

This repo is loosely based on [this blog post](https://cyberhacktics.com/passing-ssh-users-to-unique-docker-containers/)

I cant guarantee this is 100% secure and will work for 100% of use cases. But i have tested it where a user can ssh into the host and be immediately and transparently dropped into a container with home directory persistence.

I have been unable to get VSCode to work connecting to the host via ssh and being dropped into a container. And running commands directly from an ssh client requires specifying the `-t` argument for a Psudo-TTY

## installing

### Copy the contents of the host folder to the root of your host server, set executable, and add new shell to shells file

```bash
sudo cp host/* /
sudo chmod +x /usr/bin/dsh
echo "/usr/bin/dsh" | sudo tee -a /etc/shells
```

### install docker

```bash
sudo apt update; sudo apt install docker docker.io -y
```

### build the container image

```bash
cd container
sudo docker build -t debian-container .
```

### create a group for your quarantined users

```bash
sudo addgroup quarantine
```

### setup users

What matters is the user has the shell `/usr/bin/dsh` and the group `quarantine`

#### setup new users with the following

```bash
sudo adduser --shell /usr/bin/dsh --groups quarantine ${USERNAME}
sudo usermod --append --groups quarantine ${USERNAME}
```

#### setup existing users with the following

```bash
sudo usermod --shell /usr/bin/dsh ${USERNAME}
sudo usermod --append --groups quarantine ${USERNAME}
```

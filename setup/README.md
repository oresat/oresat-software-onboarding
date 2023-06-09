# Setup the Development Environment

## Getting started
1. Clone the `oresat-onboarding` repository into your workspace. A workspace is a structured environment that help
you organize and manage their work. Generally, each project should have it's own workspace. 
2. Copy the `setup/` folder into your workspace. Because we are cloning additional repositories into our workspace,
we don't want our workspace to be within a git repository. 
3. Run the `clone_repos.sh` script with either `ssh` or `https`. 
4. Run `vagrant up`. 

## Setup Contents
### `clone_repos.sh`
This script is designed to clone a set of repositories for OreSat interns using either the SSH or HTTPS protocol. 
It simplifies the process of setting up the required repositories on your local machine, ensuring that you have the 
necessary codebase to start working on the project.

**Script Overview:**

The script takes a single command-line argument that specifies the protocol to use for cloning the repositories. 
The accepted values are "ssh" or "https".

It defines two arrays, REPOS_HTTPS and REPOS_SSH, containing the HTTPS and SSH URLs of the repositories, respectively.
Based on the provided protocol argument, it selects the appropriate array of repository URLs.
The script creates a new directory called "repos" if it doesn't already exist and changes the working directory to "repos".
It iterates through the selected array of repository URLs and clones each repository into the "repos" directory.

For SSH:
```
./clone_repos.sh ssh
```
For HTTPS:
```
./clone_repos.sh https
```

### Vagrantfile

1. **Vagrant configuration version**: The line `Vagrant.configure("2")` sets the configuration version.
   You should not change this value unless you know what you're doing, as it ensures compatibility with Vagrant.
2. **Box configuration**: The line `config.vm.box = "ubuntu/focal64"` sets the base box for the virtual machine.
   In this case, it is using Ubuntu 20.04 (Focal Fossa) 64-bit. You can find more boxes at https://vagrantcloud.com/search.
3. **Synced folders**: The following lines configure shared folders between the host and guest machines
```
config.vm.synced_folder "repos/", "/vagrant"
```
These lines map the "repos" folders on the host machine to the "/vagrant" folder on the guest machine.

4. **Provisioning**: The Vagrantfile includes a shell provisioner that runs a series of commands on the guest machine
   after it starts. In this case, the script updates the package lists, installs Python 3 Pip, and installs the extra
   Linux modules for the current kernel version
```
config.vm.provision "shell", inline: <<-SHELL
  sudo apt update
  sudo apt install python3-pip
  sudo apt-get install -y linux-modules-extra-$(uname -r)
SHELL
```

## Vagrant Boxes

To begin working with Vagrant boxes in our tech stack, please follow the steps below:

1. **Install Vagrant**: Visit the [Vagrant website](https://www.vagrantup.com/downloads) and download the appropriate version
   for your operating system. Follow the installation instructions provided on the website.

2. **Install VirtualBox**: Vagrant requires a virtualization provider like VirtualBox to manage VMs. Download and
   install the latest version of [VirtualBox](https://www.virtualbox.org/wiki/Downloads) for your operating system.

3. **Initialize and start the Vagrant box**: Run the command `vagrant up` to initialize and start the VM.
   Vagrant will download the specified box, set up the VM, and provision it according to the configuration
   defined in the Vagrantfile.

4. **Connect to the VM**: Once the VM is up and running, use the command `vagrant ssh` to connect to it.
   You are now inside the VM and can start working on the project within the isolated development environment.

5. **Stop and manage the VM**: Use `vagrant halt` to stop the VM when you are done working.
   Other useful commands include `vagrant help`, `vagrant suspend`, `vagrant resume`, and `vagrant destroy`.
   Refer to the [official Vagrant documentation](https://www.vagrantup.com/docs) for more information on managing your VM.

## Using Github Docker Registry

To authenticate with GitHub Container Registry (ghcr.io), you need to create a personal access token on GitHub and use
it to log in to the registry.

Please follow these steps:

1. Visit GitHub's [Personal access tokens](https://github.com/settings/tokens) page.

2. Click "Generate new token" in the top right corner.

3. Enter a descriptive name for the token in the "Note" field, e.g., "Docker Login".

4. In the "Select scopes" section, check the box next to "read:packages" (and "write:packages" if you plan 
to push images to the registry).

6. Click "Generate token" at the bottom of the page.

Now that you have a personal access token, you can use it to authenticate with the GitHub Container Registry:

Open your terminal or command prompt.

Run the following command, replacing <your-github-username> with your GitHub username and <your-personal-access-token> 
with the token you just created:

```bash
docker login ghcr.io -u <your-github-username> -p <your-personal-access-token>
```

You should see the message "Login Succeeded" if the authentication was successful. Now you should be able to pull, 
push, and interact with images hosted on ghcr.io that you have access to.
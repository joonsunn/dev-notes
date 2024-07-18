## SSH stuff from client side

1. Generate ssh keys to be added to Github: <https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent?platform=linux>

   ```bash
   ssh-keygen -t ed25519 -C "your_email@example.com"
   exec ssh-agent bash
   ssh-add ~/.ssh/id_ed25519
   cat ~/.ssh/id_ed25519.pub
   ```

   Copy output and paste into Github ssh keys page.

   Test connection: <https://docs.github.com/en/authentication/connecting-to-github-with-ssh/testing-your-ssh-connection>

   ```bash
   ssh -T git@github.com
   ```

   Should see something like the following:

   ```bash
   Hi newuser! You've successfully authenticated, but GitHub does not provide shell access.
   ```

## Set up Git global config

Info: <https://git-scm.com/book/en/v2/Getting-Started-First-Time-Git-Setup>

```bash
git config --global user.name "John Doe"
git config --global user.email johndoe@example.com
git config --global core.editor "code --wait"
```

## Setup Git in Windows machine

### Item 6: Install Git

More info: <https://git-scm.com/download/win>

Use winget CLI tool.

Open terminal/CMD and run the following:

```bash
winget install --id Git.Git -e --source winget
```

Need to add following to PATH variables to get git to work on terminal:

```bash
C:\Program Files\Git\bin\
C:\Program Files\Git\cmd\
```

More info: <https://linuxhint.com/add-git-to-path-windows/>

Continue set up git as per documentation: <https://git-scm.com/book/en/v2/Getting-Started-First-Time-Git-Setup>

Suggest to set VS Code as Git editor:

```bash
git config --global core.editor "code --wait"
```

More info: <https://code.visualstudio.com/docs/sourcecontrol/overview#_vs-code-as-git-editor>

### Item 7: Generate SSH key pair for Github and config

At home holder (usually `C:\Users\[PC USERNAME]`, commonly shortened to just `~`)

```bash
cd ~
```

Run following command:

```bash
ssh-keygen -t rsa -b 4096 -C your_email@email.com
```

**REMEMBER TO MODIFY ABOVE COMMAND WITH YOUR OWN EMAIL ADDRESS.**

SSH key pair with name `id_rsa` and `id_rsa.pub` should appear in `~/.ssh`

At `~/.ssh` create file with name `config` (without file extension) with following content:

```config
Host gitlab.com
    HostName gitlab.com
    User git
    IdentityFile C:\Users\[PC USERNAME]\.ssh\id_rsa
    IdentitiesOnly yes
    PubkeyAcceptedAlgorithms +ssh-rsa
    HostkeyAlgorithms +ssh-rsa
```

Modify the above content of config file according to the SSH key directory in your workstation computer setup.

Try: Git clone repo.

10: Optional: Set up multiple SSH keys (for github and gitlab)

If you choose to have separate/different SSH keys for multiple services (e.g. one for github, another one for gitlab), follow below instructions. Otherwise, you may just copy the previously generated SSH public key and use it.

Use Item 7 to generate another SSH key, but this time give it a special name (e.g. adding `_github` at the end).

Most likely generated key pair will reside in home folder root. Move the pairs into the `~/.ssh` folder.

The following part about ssh-agent is based on: <https://bobbyhadz.com/blog/unable-to-start-ssh-agent-service-error-1058-powershell>

Open Powershell in Administrator Mode
Run:

```bash
Get-Service ssh-agent
```

Check that ssh-agent is stopped.

Run:

```bash
Get-Service ssh-agent | Select StartType
```

Should get that StartType is "Disabled".

Run:

```bash
Get-Service -Name ssh-agent | Set-Service -StartupType Manual
```

Note: -Start**up**Type, different from previous command.

Run:

```bash
Get-Service ssh-agent | Select StartType
```

Should get that StartType is "Manual" now.

To start ssh-agent, run:

```bash
Start-Service ssh-agent
```

Check that ssh-agent is running:

```bash
Get-Service ssh-agent
```

Back to **user** Powershell. Run:

```bash
ssh-add -l
```

If no errors other than saying file empty, run:

```bash
ssh-add [path to identity file]
```

In config file, modify it too look as below:

```bash
Host gitlab.com
    HostName gitlab.com
    User git
    IdentityFile C:\Users\[PC USERNAME]\.ssh\id_rsa
    IdentitiesOnly yes
    PubkeyAcceptedKeyTypes +ssh-rsa
    HostkeyAlgorithms +ssh-rsa

Host github.com
    HostName github.com
    IdentityFile C:\Users\[PC USERNAME]\.ssh\id_ed25519_github
    PreferredAuthentications publickey
```

Modify above IdentityFile paths to your actual identity files' paths (directory, filename, etc).

Test connection to github:

```bash
ssh -T git@github.com
```

You should see:

```bash
Hi [registered username on github]! You've successfully authenticated, but GitHub does not provide shell access.
```

More info: <https://gist.github.com/MatheusPoliCamilo/b66c6d49a3c8d39193175db0bce77b73>

### Item 11: Install yarn

Run the following in any directory:

```bash
npm install --global yarn
```

Check if `yarn` can be run from Powershell:

```bash
yarn --version
```

If a block of red error appears, open Powershell in admin mode, then run:

```bash
Set-ExecutionPolicy -ExecutionPolicy Unrestricted
```

More info: <https://bobbyhadz.com/blog/yarn-cannot-be-loaded-running-scripts-disabled>

If still doesn't work, add following to PATH:

```bash
%USERPROFILE%\AppData\Roaming\npm\node_modules\yarn\bin
```

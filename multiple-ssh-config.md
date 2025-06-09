# Configuring Multiple SSH Keys for Git Platforms on macOS

This guide explains how to set up distinct SSH keys on your Mac for different Git platforms (like GitHub and GitLab), allowing you to securely manage access without using the same key everywhere.

## 1. Generate Separate SSH Keys

If you don't already have distinct SSH keys for each platform, you'll need to generate them. It's good practice to use unique names for each key.

**For GitHub:**

1.  Open your Terminal.
2.  Generate a new SSH key, using your GitHub registered email and a distinct filename:
    ```bash
    ssh-keygen -t ed25519 -C "your_email@example.com_github" -f ~/.ssh/id_ed25519_github
    ```
    * `-t ed25519`: Specifies the key type (Ed25519 is generally recommended as more secure than RSA).
    * `-C "your_email@example.com_github"`: Adds a comment to the public key for easy identification.
    * `-f ~/.ssh/id_ed25519_github`: Specifies the filename for the private key (`id_ed25519_github`) and public key (`id_ed25519_github.pub`).
3.  When prompted, you can enter a passphrase for added security. It's highly recommended to use one.

**For GitLab:**

1.  Generate another new SSH key, using your GitLab registered email and a distinct filename:
    ```bash
    ssh-keygen -t ed25519 -C "your_email@example.com_gitlab" -f ~/.ssh/id_ed25519_gitlab
    ```
2.  Again, enter a passphrase when prompted.

You should now have two pairs of SSH keys in your `~/.ssh/` directory:
* `id_ed25519_github` and `id_ed25519_github.pub`
* `id_ed25519_gitlab` and `id_ed25519_gitlab.pub`

## 2. Add Public Keys to GitHub and GitLab

You need to tell GitHub and GitLab about your new public keys so they can authenticate you.

**For GitHub:**

1.  Copy your GitHub public key to your clipboard:
    ```bash
    pbcopy < ~/.ssh/id_ed25519_github.pub
    ```
2.  Go to [GitHub.com](https://github.com/), log in.
3.  Click your profile picture in the top right corner, then select **Settings**.
4.  In the left sidebar, click **SSH and GPG keys**.
5.  Click the **New SSH key** or **Add SSH key** button.
6.  Give it a descriptive **Title** (e.g., "My Mac - GitHub Key").
7.  Paste the public key you copied into the "Key" field.
8.  Click **Add SSH key**.

**For GitLab:**

1.  Copy your GitLab public key to your clipboard:
    ```bash
    pbcopy < ~/.ssh/id_ed25519_gitlab.pub
    ```
2.  Go to [GitLab.com](https://gitlab.com/) (or your self-hosted GitLab instance), log in.
3.  Click your avatar in the top right corner, then select **Preferences**.
4.  In the left sidebar, click **SSH Keys**.
5.  Paste the public key you copied into the "Key" field.
6.  Give it a descriptive **Title** (e.g., "My Mac - GitLab Key").
7.  Click **Add key**.

## 3. Configure Your SSH Agent

The SSH agent manages your SSH private keys and lets you use them without re-entering passphrases repeatedly.

1.  **Start the SSH agent (if it's not already running):**
    This usually happens automatically when you open a new terminal session on macOS. You can check if it's running with:
    ```bash
    eval "$(ssh-agent -s)"
    ```
    If it says `Agent pid ...`, it's running. If it gives you output like `SSH_AUTH_SOCK=/tmp/ssh-XXXXXX/agent.XXXX; export SSH_AUTH_SOCK; SSH_AGENT_PID=XXXX; export SSH_AGENT_PID; echo Agent pid XXXX;`, then it has started the agent.

2.  **Add your private keys to the agent:**
    ```bash
    ssh-add ~/.ssh/id_ed25519_github
    ssh-add ~/.ssh/id_ed25519_gitlab
    ```
    You will be prompted for the passphrase for each key if you set one.

    **Note:** For macOS Sierra 10.12.2 and later, you can have your SSH agent remember your passphrases in the macOS Keychain so you don't have to enter them every time you restart your Mac. If you're prompted to add it to the keychain, choose yes. If not, you might need to configure it by adding `UseKeychain yes` to your `~/.ssh/config` file (see next step).

## 4. Create and Configure Your SSH Config File

This is the most crucial step for telling Git which key to use for which platform.

1.  Create or open the SSH config file:
    ```bash
    touch ~/.ssh/config
    chmod 600 ~/.ssh/config # Ensure correct permissions
    open ~/.ssh/config
    ```

2.  Add the following configurations to the `~/.ssh/config` file:

    ```ssh-config
    # GitHub configuration
    Host github.com
      HostName github.com
      User git
      IdentityFile ~/.ssh/id_ed25519_github
      IdentitiesOnly yes
      UseKeychain yes # Optional, but recommended for macOS to remember passphrases

    # GitLab configuration
    Host gitlab.com
      HostName gitlab.com
      User git
      IdentityFile ~/.ssh/id_ed25519_gitlab
      IdentitiesOnly yes
      UseKeychain yes # Optional, but recommended for macOS to remember passphrases
    ```

    * `Host github.com` / `Host gitlab.com`: This is the alias that Git will use to match the remote URL.
    * `HostName`: The actual domain name of the Git platform.
    * `User git`: Always `git` when using SSH with Git platforms.
    * `IdentityFile`: Specifies the path to the private key to use for this host. **This is the key part.**
    * `IdentitiesOnly yes`: Tells SSH to only use the keys specified by `IdentityFile` for this host, preventing it from trying other keys.
    * `UseKeychain yes`: (macOS specific) Integrates with the macOS Keychain to store your passphrase, so you don't have to type it every time you add the key to the agent.

    Save and close the `~/.ssh/config` file.

## 5. Test Your Connection

Finally, test that your SSH connections are working correctly for each platform.

**For GitHub:**

```bash
ssh -T git@github.com
```

You should see a message like: `Hi your_username! You've successfully authenticated, but GitHub does not provide shell access.`

**For GitLab:**

```bash
ssh -T git@gitlab.com
```

You should see a message like: `Welcome to GitLab, @your_username!`.

If you see "Permission denied (publickey)" or similar errors, double-check:

- That your public keys are correctly added to the respective platforms.
- The `IdentityFile` paths in your `~/.ssh/config` file are correct.
- That your keys are added to the SSH agent (`ssh-add -l`).
- The permissions of your `~/.ssh` directory and files are correct (e.g., `chmod 700 ~/.ssh` and `chmod 600 ~/.ssh/*`).


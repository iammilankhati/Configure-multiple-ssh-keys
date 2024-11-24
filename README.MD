## Configuring multiple SSH Keys

#### Usecases

If you are working for two different github account, your single ssh keys doesn't work, for that you have to setup multiple ssh keys

## Steps to configure multiple ssh keys

### Generate SSH Keys for Each Account

- Open your terminal
- Generate a SSH key for the first Github account

  ```
  ssh-keygen -t rsa -b 4096 "your_first_github_account_email@example.com"
  ```

  when promoted to save the key , name it something like ` id_rsa_github_personal` and leave other fields blank

- Generate a SSH key for the second Github account

  ```
  ssh-keygen -t -rsa -b 4096 "your_second_github_account_email@example.com"
  ```

  when promoted to save the key, name it something like id_rsa_github_work and leave other fields blank

### Add SSH Keys to the SSH Agent

- Start the SSH agent

  ```
  eval "$(ssh-agent -s)"
  ```

- And both keys to the agent

  ```
  ssh-add ~/.ssh/id_rsa_github_personal
  ssh-add ~/.ssh/id_rsa_github_work
  ```

### Add Keys to Github Account

- Go to your GitHub account settings → **SSH and GPG keys** and add the corresponding key for each account.
- Personal

  ```
  pbcopy < ~/.ssh/id_rsa_github_personal.pub
  ```

- Work

  ```
  pbcopy < ~/.ssh/id_rsa_github_personal.pub
  ```

### Configure SSH Config File

- Create or edit the SSH configuration file

  ```
  nano ~/.ssh/config
  ```

  - Add the following configurations

  ```
  #Personal GitHub account
  Host github-personal
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_rsa_github_personal
  #Work GitHub account
  Host github-work
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_rsa_github_work
  ```

- Save and close the file

### Update the git remote URLs

For each repository, update the `git` remote URL to use the appropriate host from the SSH config:

- For repositories associated with your personal account:

  ```
  git remote set-url origin git@github-personal:username/repo.git
  ```

- For repositories associated with your work account:

  ```
  git remote set-url origin git@github-work:username/repo.git
  ```

### Test the Setup

Test both the configurations to ensure they work:

```
ssh -T github-personal
ssh -T github-work
```

To check which remote URL you are attached `git remote -v`

With this setup, you can seamlessly work with multiple GitHub accounts on the same machine.

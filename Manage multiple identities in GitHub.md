---
tags:
  - "#github"
---
## Overview

You might have multiple GitHub accounts and depending on the project you are working on you might need to authenticate from `account A` or `account B`. 

> [!Note]
> Git Credential Manager (**GCM**) can help you manage multiple accounts based on the URL of the repository

## Description (skip if in a hurry)

You might use one GitHub identity for your personal work, another for your open source work, and a third for your employer's work. You can ask Git to assign a different credential to different repositories hosted on the same provider. HTTPS URLs include an optional "name" part before an `@` sign in the domain name, and you can use this to force Git to distinguish multiple users. This should likely be your username on the Git hosting service, since there are cases where **GCM** will use it like a username.

## Usage

> [!Warning]
> Make sure you have `git-credential-manager` installed

### Login to your GitHub account

```bash
git-credential-manager github login
```

A pop-up window will allow you to login to your GitHub account. After that, `git-credential-manager` will be able to manage the authentication of that user account for you.

You can check the accounts managed by `git-credential-manager` as follows:

```bash
git-credential-manager github list
```

This will display a list of usernames for all the managed accounts.

### Configure GCM for a particular remote

To set a default account for a particular remote you can simply set the following Git configuration:

```bash
git config --global credential.<URL>.username <USERNAME>
```

Where:
- `<URL>`: `https://github.com/Asus-Robotics-and-AI-Center`
- `<USERNAME>`: `AlejandroMoreno-ARAIC`

Then you can clone repository (e.g. `https://github.com/Asus-Robotics-and-AI-Center/gnc-dam-c-src`) and **GCM** will know how to authenticate for you.
 
### Configure GCM for an existing repository

If you want to want to use a specific account for a specific (existing) repository:

```bash
# Go to the directory where your existing repository is located
cd <your-repo-path>
# Update the existing clone URL
git remote set-url origin https://<USERNAME>@github.com/<REPOSITORY-URL>
```

- `<REPOSITORY-URL>`: `Asus-Robotics-and-AI-Center/gnc-dam-c-src`
- `<USERNAME>`: `AlejandroMoreno-ARAIC`

## References 

- [Manage multiple identities in GitHub](https://github.com/git-ecosystem/git-credential-manager/blob/main/docs/multiple-users.md)

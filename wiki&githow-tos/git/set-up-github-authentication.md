# Set Up GitHub Authentication

## Overview

To enhance security, GitHub no longer allows simple authentication such as entering your password on the command line to push to repositories. There are other ways around this, such as using Personal Access Tokens, but those get complicated. We suggest using [Git Credential Manager](https://github.com/GitCredentialManager/git-credential-manager).

## Setup

1. Install GitHub Credential Manager (installation instructions [here](https://github.com/GitCredentialManager/git-credential-manager/blob/release/docs/install.md)).
1. Run the installer.

After this, whenever you try to push your changes, it will prompt you to log into GitHub through your browser.

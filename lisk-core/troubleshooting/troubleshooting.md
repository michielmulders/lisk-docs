# Lisk Core Troubleshooting
 
## Overview

### Setup
- **[Binary]** [Installation script fails](#installation-script-fails-binary)
- **[Source]** ['npm install' fails with error 'Failed at the sodium@2.0.1 preinstall script.'](#npm-install-fails-with-error-source)
- **[Source]** [Nothing shown in console after starting Lisk Core](#nothing-shown-in-console-after-starting-lisk-core)

### Administration
- [Enable forging: delegate not found](#enable-forging-delegate-not-found)
- [Enable forging: Invalid password and public key combination](#enable-forging-invalid-password-and-public-key-combination)

## Setup

### Installation script fails (Binary)
#### Problem:
After running `bash installLisk.sh install -r test` the installation script is aborted with the following output:
```shell
Coldstarting Lisk for the first time
√ Postgresql is running.
X Failed to create Postgresql user.
Installation failed. Cleaning up...
Stopping Lisk components before cleanup
√ Lisk stopped successfully.
X Postgresql failed to stop.
√ Postgresql Killed.
```
#### Solution:
PostgreSQL is already installed on your system.
To solve this issue simply remove postgres by running the following command:
```bash
sudo apt-get --purge remove postgresql postgresql-doc postgresql-common
```

### npm install fails with error (Source)

Info | Note 
--- | --- 
![info note](../../info-icon.png "Info Note") | This issue should not be present anymore since Lisk Core `1.2`

#### Problem:
`npm install` fails with error `Failed at the sodium@2.0.1 preinstall script.`
When trying to install the necessary node modules for Lisk Core, the install script fails while trying to build sodium.
This happens for newer versions of npm, which are not supported by core 1.0.0, yet.
#### Solution:
Install npm version 3.10.10.
Check if you have the correct Node version installed by running `node -v`
If the version is not ^6.14.1, first install the supported Node version.
```bash
nvm install 6.14.1
```
With the right node version, you can proceed to install the right `npm` verison:
```bash
node -v
v6.14.1
npm install npm@3.10.10
```

### Nothing shown in console after starting Lisk Core (Source)
#### Problem: 
After installing from Source and starting Lisk Core with `node app.js`, no are logs visible in console.
This is in fact an expected behaviour, as the default console logging value in the config is `none`, which means no logs are shown in the console after starting the process.
#### Solution: 
To verify, that your installation works as expected, you can change the`consoleLogLevel` to `error`, `info` or `debug`.
Alternatively, you can check the log files located in `logs/`, which are on `info` logging level by default.

## Administration

### Enable forging: Delegate not found
#### Problem:
When trying to activate forging on your node, it answers with `Delegate with publicKey: xyz not found`
#### Solution 1: Node still syncing
Check the current height of your Node and compare it with the Height in Explorer.
If your Nodes' height is significantly lower than the height shown in the Explorer, it means your Node is still syncing / downloading the Lisk Blockchain. At this time, enabling forging might fail, because the Delegate registration has not been downloaded, yet.
To solve it, just wait until your Node is fully synced.
#### Solution 2: Missing data in config
Check your `config.json` in section `forging.delegates`.
If you want to enable forging for a particular delegate on your node, you need to store an object with the delegates' publickey and encrypted passphrase in that section as described in the [configuration](../user-guide/configuration/configuration.md#forging) section.

### Enable forging: Invalid password and public key combination
#### Problem:
When trying to activate forging on a node like described in section [User Guide/Configuration](../user-guide/configuration/configuration.md#enable-disable-forging), it responds with: `{"message":"\"Invalid password and public key combination\""}`.
#### Solution:
As the message states, the provided combination of your delegates publicKey and the password don't seem to be valid. Please ensure, both properties are set to the correct values, especially that you don't use the original passphrase of your account with that command.

We further explain the chosen naming in order to avoid confusion:
- **Passphrase** is always referring to your 12 word long mnemonic passphrase that was created at the same time as your Lisk ID. You should always keep this secure and private! For communication with the API, the passphrase is not passed in plaintext. Instead the password is passed so you use it to encrypt your passphrase and the encrypted passphrase is stored in your [config](../user-guide/configuration/configuration.md).
- **Password** is always referring to the secret word/s you use to encrypt your passphrase symmetrically, like described in this [section](../user-guide/configuration/configuration.md#forging)

Should you have any further queries please reach out to one of the team or the Lisk community on [Lisk Chat](https://lisk.chat/home)

# LockUp
A Linux script to automate encrypted synchronous backups to Google Drive

Information kept in the cloud should always be stored encrypted using private keys which you exclusively control.

## Prerequisites
### Install GNU Privacy Guard (GPG)

For apt based distributions we can install GPG with the following command for gnupg package.
`sudo apt install gnupg`

For yum based distributions we can install GPG with the following command for gnupg package.
`sudo yum install gnupg`

### Create a GPG public/private key pair

To create a new key pair run the following command `gpg --full-gen-key`

To import a new key `gpg --import {key-to-import}`

### Install and configure GDrive 

The latest gdrive release can be dowloaded from <a href="https://github.com/prasmussen/gdrive/releases">gdrive Github repository</a>

Once gdrive has been installed, run `gdrive about` and follow the instructions to give it permission to your Google Drive.

## Recommended installation
The installation steps require the external tools wget and unzip. If these are not present on your system they may be installed using your operating systems's package manager.
```
wget https://github.com/brightpathsofglory/LockUp/archive/refs/tags/v1.0.0.zip
unzip v1.0.0.zip
sudo mv ./LockUp-1.0.0/lockup /usr/bin/lockup
sudo chmod +x /usr/bin/lockup
mkdir -p ~/.config/lockup
mv ./LockUp-1.0.0/lockup.cfg.orig ~/.config/lockup/lockup.cfg
rm ./v1.0.0.zip
rm -fr ./LockUp-1.0.0/
```
### Setup a place in Google Drive for your backups
Run the following command, replacing {some-name} with the name of the destination folder where you would like to keep your backups.
```
gdrive mkdir {some-name}
```
You will receive output similar to the following:
```
Directory 10wDVBKhSj22IAXvU0fDLY9rkR3DWKxKy created
```
The random string between the word 'Directory' and 'created' is your file-id. This text will need to be placed in the lockup.cfg configuration file.
  
### Configure the directives in ~/.config/lockup/lockup.cfg
Edit ~/.config/lockup/lockup.cfg using your favourite text editor.

The lockup.cfg file is heavily documented.
Two directives worth mentioning are:
<ol>
  <li>RECIPIENT. This must be set to the email address specified when the public/private GPG key was created.</li>
  <li>REMOTE_FILEID. This must be set to the random string returned from the gdrive mkdir command.</li>
</ol>

LockUp is capable of creating encrypted backups of multiple directories. 

Each backup directory can be encrypted as either GPG binary or ASCII.

LockUp won't encrypt files which are already encrypted using GPG (purely determined by a file extension of .gpg or .asc).

### Running LockUp
From a terminal run the following:
```
lockup
```
The contents of each directory (and subdirectories) will be encrypted and uploaded to your Google Drive. 

Directory structures will be maintained. 

Subsequent execution of LockUp will result in a sync, so files changed and added locally will be added to the encrypted backup. Files removed will be removed from the encrypted backup.

### Verification
You can verify the lockup script using sha256sum. The checksum for v1.0.0 is:

```
d02a0a78bad827719ce50ef0ccff32df584eae367f9c2c3557374e0b619ad2fd  lockup
```

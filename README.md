# laravel-deployment-git

Scripts for zero-downtime deployments via git

The purpose of these scripts is to use a remote, bare git repository for deploying your Laravel application with zero-downtime. Somewhat mimicks Laravel Envoyer in terms of directory structure:

- creates `current` symlink pointing to release directory
- reloads nginx (ensure that user has sudo for that similar to Envoyer)
- cleans up release directories

## Dependencies

- Used primarily on Ubuntu 20/22. You're mileage my vary.
- requiries 'setfacl' : `sudo apt install acl -y`

## Setup

- Add scripts folder to root of your Laravel app.
- Create bare git repository: `mkdir /var/git/{app_name}.git && cd /var/git/{app_name}.git && git init --bare`.
- Assumes your `/var/git/...` directories are owned by the user you are connecting to SSH with.
- Assumes your `/var/www/apps/...` directories are also owned by that user. (I'm using a `deploy` user and ensuring that all relevant directories are owned by `deploy:www-data`).
- Replace `APP_NAME` value in `server-post-receive` with your app name and ensure the base directories exist.
- Copy `scripts/server-post-receive` hook to `/var/git/{app_name}.git/hooks/`
- Rename script and make executable: `mv /var/git/{app_name}.git/hooks/server-post-receive /var/git/{app_name}.git/hooks/post-receive && chmod +x /var/git/{app_name}.git/hooks/post-receive`

## TODO

- Put as much of the above as possible in a `setup.sh` script to automate setup steps.

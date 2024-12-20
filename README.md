# Twenty Twenty Four Spenpo Child Theme

This is a child theme for the Twenty Twenty Four theme. It is used to add custom functionality to the website.

## Development

To develop on this theme, you can clone the repository and sync the `src` directory to the `/wp-content/themes/twentytwentyfour-[YOUR_CHILD_THEME_NAME]/` directory. 
* Run WordPress locally however you normally do
    * Check out my [wp-env](https://github.com/spope851/wp-env) project if you don't know how to do that.
* Activate the theme via the admin dashboard.
* if you make your changes from inside the wordpress source code, sync your changes to the `src` directory by running `rsync -avz --delete . ../out/of/wordpress/twentytwentyfour-[YOUR_CHILD_THEME_NAME]/src/`

## Deployment

To deploy the theme, push to the `main` branch and the GitHub Actions workflow will deploy the theme to the live site. you will have to make sure your server has SSH enabled and that you generated a key on it for the actions script to use. details on that below:

### SSH Key

ssh into your server and generate a key with the following command:

```
ssh-keygen -t rsa -b 4096 -C "your_email@example.com" -f ~/.ssh/id_rsa -N ""
```

* use any key type you want, I'm showing RSA here
* you can use any email you want, it's just for your reference
* the key file should be created in your `.ssh` directory
* you will be prompted to enter a passphrase, just hit enter to skip it

Once you have the key, you can add it to the authorized keys file with the following command:

```
cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
```
Then find and copy your private key with this command:

```
cat ~/.ssh/id_rsa
```

Copy the entire output of the command:

```
-----BEGIN OPENSSH PRIVATE KEY-----
base64-encoded-key-data-goes-here
-----END OPENSSH PRIVATE KEY-----
```

Then you can add the private key to the GitHub Actions workflow by adding it to the `PRIVATE_KEY` secret in the repository settings.

### Additional secrets

* `REMOTE_PATH`: The path to the wordpress root directory on your server
* `USERNAME`: The username to use to connect to your server
* `HOST`: The host to use to connect to your server
* `PORT`: The port to use to connect to your server

### Staging

* If you want to deploy to the staging environment, you can create a new branch and push it to the repository.
* The workflow will automatically deploy the branch to the staging environment.
* You may have to reconfigure the `DEPLOY_PATH` logic in the workflow to match the staging path on your server.
* If your staging environment is on a separate server, you will have to add the new server's secrets to the repository secrets and update the workflow to use the new server and other secrets.
* It can easily be extended to support more environments on other branches by adding additional `if` statements to the workflow.

## Manual Deployment

If you'd rather deploy the theme manually, just add your local machine's public key to the authorized keys file on your server and run the opposite rsync command from the deployment workflow:

```
rsync -avz --delete -e "ssh -p PORT" ./src/ USERNAME@HOST:REMOTE_PATH/wp-content/themes/twentytwentyfour-spenpo/
```

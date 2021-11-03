# Heroku Buildpack: Custom SSH keys

Use *Custom SSH keys buildpack* if you need to, for example, download a dependencies stored in a private repositories.

Based on [http://stackoverflow.com/a/29677091/3303182](http://stackoverflow.com/a/29677091/3303182).

## Usage

- Add the buildpack to your app:
  `heroku buildpacks:add --index 1 https://github.com/chadsmith/custom-ssh-keys-buildpack`

- Generate a new SSH key (https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/)

  For this example I will assume that you named the key `deploy_key`.

- Add the ssh key to your private repository account.

  * Github: https://help.github.com/articles/adding-a-new-ssh-key-to-your-github-account/

  * Bitbucket: https://confluence.atlassian.com/bitbucket/add-an-ssh-key-to-an-account-302811853.html

- Encode the private key as a base64 string and add it as the `CUSTOM_SSH_KEY` environment variable of the heroku app. Set multiple keys by separaing them with commas.

- Make a comma separated list of the hosts for which the ssh key should be used and add it as the `CUSTOM_SSH_HOSTS` environment variable of the heroku app.

  ```
  # OSX
  $ heroku config:set CUSTOM_SSH_KEYS="$(base64 --input ~/.ssh/deploy_key_1),$(base64 --input ~/.ssh/deploy_key_2)" CUSTOM_SSH_HOSTS=bitbucket.org,github.com

  # Linux
  $ heroku config:set CUSTOM_SSH_KEYS="$(base64 --input ~/.ssh/deploy_key_1),$(base64 --input ~/.ssh/deploy_key_2)" CUSTOM_SSH_HOSTS=bitbucket.org,github.com
  ```

- Deploy your app and enjoy :)

## Motivation

I needed to install dependencies stored in private repositories but I didn't want to hardcode passwords in the code.
I found a solution in [StackOverflow](http://stackoverflow.com/a/29677091/3303182) but it only worked for the node buildpack
so I decided to create this technology agnostic buildpack.

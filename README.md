


# Demo Symfony App for Deploy Now

## What to do to make in running on Deploy now?
1. If you don't have one - create a free [Deploy Now account](https://www.ionos.com/hosting/deploy-now) You won't regret it - promised! :hand:
2. Because Symfony takes more than 50MB in deployment (that's the maximum storage quota of free projects),    
   you can join our [PHP alpha](https://docs.ionos.space/docs/php-alpha) and we give your more space to get it run :sunglasses:
3. If you just created a Deploy Now account - log in to it and connect your Github account
4. Now you have two choices...

### 5. Fork this repository to your own Github account
6. Then select this repository in Deploy Now under "Deploy from my own Github repository"
7. Select Static M as project package

### OR 5. click this button and set this project up as Static M
[![Deploy to IONOS](https://images.ionos.space/deploy-now-icons/deploy-to-ionos-btn.svg)](https://ionos.space/setup?repo=https://github.com/ionos-deploy-now/symfony-starter)

**That's all the magic.** While we building and deploying - have a cup of coffee. Okay, just a little one, because we're fast! :runner: After that - you can visit your deployed Symfony site, you find the link at your Deploy Now dashboard. And if you change something in your (new) repository and commit it - we build again, you drink a small coffee again, ... and your changes are online. :raised_hands:

## It's running now, but...tell me more, tell me more!
### What's the magic behind?
This repository is based on a completely normal composer created fresh Symfony installation. With a little pinch of tailwindcss and IONOS branding. In this [commit](https://github.com/ionos-deploy-now/symfony-starter/commit/a947d1581723d5b9b693e9fcc4e6a95c95969f87) you can find the files that are doing the magic. And if you are a great magician and understand it, you can adapt it to your own project. Please welcome with a big applause...

#### [`.htaccess`](https://github.com/ionos-deploy-now/symfony-starter/blob/a947d1581723d5b9b693e9fcc4e6a95c95969f87/.htaccess)
To put this file to your document root is important, because we can't change the web root in our infrastructure. Because of that we'll have to forward all requests to the directory that would be web root, if we could change it... :innocent: Furthermore we enable gzip-compression.

#### [`.ionos.yaml`](https://github.com/ionos-deploy-now/symfony-starter/blob/a947d1581723d5b9b693e9fcc4e6a95c95969f87/.ionos.yaml)
This file we take during project setup to create the Github workflow file in your repository. After you setup your project you find a commit at this repository that added `.github/workflow/deploy-now.yml` to your repo. If you want it that way, `.ionos.yaml` is a template file that helps Deploy Now to create the github workflow (with `composer install`, `npm install` and `npm run build`, ...). **Important**: changings at this file only working if you make them **before** setting up project with Deploy Now. **After** project is set up, this file is **ignored**. In this case you have to edit `.github/workflow/deploy-now.yml` directly.

If you edit this file **before** project creation, you can specify the build process on Github. Use `node` or not!? Set build params for composer!? Run artisan tests!? Specify vars for `.env`!?

**Imporant** All the things you specify here, especially the php commands, are **running on Github runners**, not on your actual webspace!

#### [`.deploy-now/config.yaml`](https://github.com/ionos-deploy-now/symfony-starter/blob/a947d1581723d5b9b693e9fcc4e6a95c95969f87/.deploy-now/config.yaml)
This file contains all the stuff that we do before/after every deployment **on your actual webspace**. In contrast to `.ionos.yaml`, changes at this file are taken into account at each deployment. Don't want to clear caches after deployment, because it's hard for you to let things go? :cold_sweat: No problem, just delete `APP_ENV=prod APP_DEBUG=0 php8.0-cli bin/console cache:clear` from `.deploy-now/config.yaml`, commit it and find your luck with not cleared caches! :grin:

There are two parts in this file that looks very similar, but are very different. On line 5 you find `bootstrap` - this part from line 5 to line 30 - is only taken at the first deployment of **each** branch. Here you can create for example a sqlite database.

On line 33 - 54 you find the `recurring` part. This part is executed at every deployment after the first deployment.

At both parts you have the same three keys:  
`excludes` - folders and files that shouldn't be synced to webspace, rsync syntax  
`pre-deployment-remote-commands` - commands/actions that should run on webspace **before** files from deployment are copied (e.g. enable maintenance mode, dump database, ...)  
`post-deployment-remote-commands` - commands/actions that should run on webspace **after** files from deployment are copied (e.g. migrate database, clear and warmup caches, set file permissions, ...)

### Can I use an existing Symfony project of mine and make it run?
Yeah, you can try it! Please read the paragraph below and add the three files to your repository. We would recommend you a feature branch. :wink: Then you can edit the files the way you need it.

### What about databases?
This sample comes with a sqlite database, that's working the Symfony way - migrations included. Feel free to use it!

If you want to use MySQL or MariaDB - we have to disappoint you, we can't offer you this kind of databases as a feature in Deploy Now **at the moment**. But we're working hard on it. :muscle:  
If you got a MySQL/MariaDB server that is reachable from outside, you can put the access data to it to the part of `.ionos.yaml`/`.github/workflow/deploy-now.yml` where the env vars file is generated. We recommend you to put the access data into Github secrets. :speak_no_evil: Now commit it and it should work!

### What about mailing?
It's - like the configuration of database - only an env variables job. Add your mail access data to the env vars part of `.ionos.yaml`/`.github/workflow/deploy-now.yml`. Best way to do it - via Github secrets.

### Go away with PHP 8, I'm in love with PHP 5.3 forever! :heartpulse:
It's okay, man! :kissing_heart: Our infrastructure is able to run applications with PHP 5.3 too. But at the moment there is no way for you to define the PHP version of the webserver. If you can't use PHP 8.0 - please contact us and we are setting up your environment with your wished PHP version. In future it will be possible to define the PHP version for each branch in the Deploy Now dashboard.

What you can change already is the cli-version of PHP. Just find and replace `php8.0-cli` in `.deploy-now/config.yaml` with `php5.3-cli`. Or `php7.4-cli`. Or....okay, we think you got it! **Important**: if you call `php` without any version, you got `php4.4` :grimacing: That's the reason you can't call `bin/console` directly.

The PHP version that is used on the Github runners in composer action you can specify in `.github/workflow/deploy-now.yml`. Please take a look at the documentation of this [composer action](https://github.com/php-actions/composer#passing-arguments).

### One last thing...
If you have any suggestions, requests, questions, please get in touch with us. Write us directly via Deploy Now dashboard or send a mail to [info@DeployNow.dev](mailto:info@DeployNow.dev). We do our best to make you happy! :carousel_horse:

### Ah, almost forgotten -  nice-to-know-it links

- [Deploy Now docs](https://docs.ionos.space/)
- [Deploy Now product info](https://www.ionos.com/hosting/deploy-now)

_IONOS Deploy Now - made with :heart: by [us](https://docs.ionos.space/about-us/) in Karlsruhe, Germany_
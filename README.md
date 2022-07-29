# WordPress 4.9.6-ja + pg4wp2 Heroku

This project is a template for installing and running [WordPress](http://wordpress.org/) on [Heroku](http://www.heroku.com/). The repository comes bundled with:
* [PostgreSQL for WordPress](http://wordpress.org/extend/plugins/postgresql-for-wordpress/)
* [Amazon S3 and Cloudfront](https://wordpress.org/plugins/amazon-s3-and-cloudfront/)
* [WP Sendgrid](https://wordpress.org/plugins/wp-sendgrid/)
* [Wordpress HTTPS](https://wordpress.org/plugins/wordpress-https/)

## Installation

Use the Deploy to Heroku button, or use the old fashioned way described below.

[Use Crypt  type for htpasswd Generator is here.](https://macminiosx.github.io/passwd-generator/)

<a href="https://heroku.com/deploy?template=https://github.com/datyb/wordpress-ja-pg4wp2-heroku/tree/master">
  <img src="https://www.herokucdn.com/deploy/button.png" alt="Deploy">
</a>

### Install Heoku Cli


```
   sudo curl https://cli-assets.heroku.com/install-ubuntu.sh | sh
   heroku login 
```


### Use the Deploy to Heroku button Issue

You've deployed an application to Heroku and when you attempt to clone the project you receive the error 'You appear to have cloned an empty repository'

Clone the repo with heroku

    $ heroku git:clone -a <YOUR-APP-NAME>
    it will be reported as empty - that's ok

cd into the project and add a git remote pointing at the original source,

    $ git remote add origin https://github.com/datyb/wordpress-ja-pg4wp2-heroku/

pull from the remote origin

    $ git pull origin master

## Clone the repository from Github

    $ git clone git@github.com:datyb/wordpress-ja-pg4wp2-heroku.git

With the [Heroku gem](http://devcenter.heroku.com/articles/heroku-command), create your app

    cd wordpress-heroku
    r=$RANDOM; echo $r
    heroku create ss-wp$r --ssh-git --stack cedar-14
    
get this log

```
    Creating strange-turtle-1234... done, stack is cedar
    http://strange-turtle-1234.herokuapp.com/ | git@heroku.com:strange-turtle-1234.git
    Git remote heroku added
```

Add a database to your app

    heroku addons:create heroku-postgresql:hobby-dev -a ss-wp$r # -a <app Name> which here ss-wp12 is appname
get thi log:

```
    Adding heroku-postgresql:dev to strange-turtle-1234... done, v2 (free)
    Attached as HEROKU_POSTGRESQL_COLOR
    Database has been created and is available
    Use `heroku addons:docs heroku-postgresql:dev` to view documentation
    
```

Promote the database (replace COLOR with the color name from the above output)

    heroku pg:promote HEROKU_POSTGRESQL_COLOR -a ss-wp$r  # -a <app Name> which here ss-wp12 is appname
    Promoting HEROKU_POSTGRESQL_COLOR to DATABASE_URL... done

Add the ability to send email (i.e. Password Resets etc)

    $ heroku addons:add sendgrid:starter -a ss-wp$r  # -a <app Name> which here ss-wp12 is appname
    Adding sendgrid:starter on your-app... done, v14 (free)
    Use `heroku addons:docs sendgrid` to view documentation.



Store unique keys and salts in Heroku environment variables. Wordpress can provide random values [here](https://api.wordpress.org/secret-key/1.1/salt/).

    heroku config:set AUTH_KEY='put your unique phrase here' \
      SECURE_AUTH_KEY='put your unique phrase here' \
      LOGGED_IN_KEY='put your unique phrase here' \
      NONCE_KEY='put your unique phrase here' \
      AUTH_SALT='put your unique phrase here' \
      SECURE_AUTH_SALT='put your unique phrase here' \
      LOGGED_IN_SALT='put your unique phrase here' \
      NONCE_SALT='put your unique phrase here'  -a ss-wp$r  # -a <app Name> which here ss-wp12 is appname
or use this:

    heroku config:set AUTH_KEY='^%RH5z>.rM=9A+oH(6n,`+F99Z|3V@_ArpWy%{;+y|pFcCuKwl/<VP!#4oJ0+p2t' \
    SECURE_AUTH_KEY='(<ofl_w;1k(tpsPF<].GW|p@rq|=0Mc<d~u[N8S!1C|{obdleN{+1&(;/mTTD0yh' \
    LOGGED_IN_KEY='Thwf<)Ey^9EdtpxD?Z5TlO9-Pc|v)~La1BBRPk=Ey|%jPUc%A!SxVo6lxQ6uitK ' \
    NONCE_KEY='BNx YjS{|[jtE,eHXh.m0{F=86uW<92),uU8}Yk)dz)j@bXqj@mEt!q|^.HU-<<w' \
    AUTH_SALT='y1CFU=<RNO5Y_Io-}aovd}L:o-I{HdNMrt/=RR peqTn/%_@#U3uD^~]=8#z(`a' \
    SECURE_AUTH_SALT='Ez~bty^ZCop.RV_)&zVb3:U MeDx1+m>Yz@m#>M5wpIk|5hoRQ~Z&m`r mJd69(U' \
    LOGGED_IN_SALT='~R]Xaq<WE-j9Bc-ggAhQZdE|p]q bBolv$]YXjIu:7P;/)WP}R3Ys,*>%4Eqv[,/' \
    NONCE_SALT='KR~5 NWctd2l^f>(f9~oxhMT?I7JcTM]^>NEzKZL.U+9yc^2hZujh~PALNs$Vdua' -a ss-wp$r  # -a <app Name> which here ss-wp12 is   appname
  



Create .htpasswd

    $ echo "USERNAME:CRYPT PASSWORD" > .htpasswd
    git init
    git add .
    git remote add origin https://git.heroku.com/ss-wp$r.git
    git commit -am "start"
    
Create a new branch for any configuration/setup changes needed

    $ git checkout -b production
    
    



Deploy to Heroku

    $ git push heroku production:master
    -----> Deleting 0 files matching .slugignore patterns.
    -----> PHP app detected

     !     WARNING: No composer.json found.
           Using index.php to declare PHP applications is considered legacy
           functionality and may lead to unexpected behavior.

    -----> No runtime requirements in composer.json, defaulting to PHP 5.6.2.
    -----> Installing system packages...
           - PHP 5.6.2
           - Apache 2.4.10
           - Nginx 1.6.0
    -----> Installing PHP extensions...
           - zend-opcache (automatic; bundled, using 'ext-zend-opcache.ini')
    -----> Installing dependencies...
           Composer version 1.0-dev (ffffab37a294f3383c812d0329623f0a4ba45387) 2014-11-05 06:04:18
           Loading composer repositories with package information
           Installing dependencies
           Nothing to install or update
           Generating optimized autoload files
    -----> Preparing runtime environment...
           NOTICE: No Procfile, defaulting to 'web: vendor/bin/heroku-php-apache2'
    -----> Discovering process types
           Procfile declares types -> web

    -----> Compressing... done, 78.5MB
    -----> Launcing... done, v5
           http://strange-turtle-1234.herokuapp.com deployed to Heroku

    To git@heroku:strange-turtle-1234.git
      * [new branch]    production -> master

After deployment WordPress has a few more steps to setup and thats it!

all in  Three !!:

```
   sudo curl https://cli-assets.heroku.com/install-ubuntu.sh | sh
   heroku login 
```
``` 
 git clone https://github.com/datyb/wordpress-ja-pg4wp2-heroku
    r=$RANDOM; echo $r
    heroku create ss-wp$r --ssh-git --stack cedar-14
    heroku addons:create heroku-postgresql:hobby-dev -a ss-wp$r # -a <app Name> which here ss-wp12 is appname

heroku addons:add sendgrid:starter -a ss-wp$r # -a <app Name> which here ss-wp@r is


heroku config:set AUTH_KEY='^%RH5z>.rM=9A+oH(6n,`+F99Z|3V@_ArpWy%{;+y|pFcCuKwl/<VP!#4oJ0+p2t' \
SECURE_AUTH_KEY='(<ofl_w;1k(tpsPF<].GW|p@rq|=0Mc<d~u[N8S!1C|{obdleN{+1&(;/mTTD0yh' \
LOGGED_IN_KEY='Thwf<)Ey^9EdtpxD?Z5TlO9-Pc|v)~La1BBRPk=Ey|%jPUc%A!SxVo6lxQ6uitK ' \
NONCE_KEY='BNx YjS{|[jtE,eHXh.m0{F=86uW<92),uU8}Yk)dz)j@bXqj@mEt!q|^.HU-<<w' \
AUTH_SALT='y1CFU=<RNO5Y_Io-}aovd}L:o-I{HdNMrt/=RR peqTn/%_@#U3uD^~]=8#z(`a' \
SECURE_AUTH_SALT='Ez~bty^ZCop.RV_)&zVb3:U MeDx1+m>Yz@m#>M5wpIk|5hoRQ~Z&m`r mJd69(U' \
LOGGED_IN_SALT='~R]Xaq<WE-j9Bc-ggAhQZdE|p]q bBolv$]YXjIu:7P;/)WP}R3Ys,*>%4Eqv[,/' \
NONCE_SALT='KR~5 NWctd2l^f>(f9~oxhMT?I7JcTM]^>NEzKZL.U+9yc^2hZujh~PALNs$Vdua' -a ss-wp$r  # -a <app Name> which here ss-wp@r is

echo "USERNAME:CRYPT PASSWORD" > .htpasswd
    git init
    git add .
    git remote add origin https://git.heroku.com/ss-wp$r.git
    #git config user.name "someone"
    #git config user.email "someone@someplace.com"
    git commit -am "start"
    git checkout -b production
    heroku git:remote -a ss-wp$r
    #heroku stack:set heroku-18 --remote origin
    git push heroku production:master
 ```
 

## Usage

Because a file cannot be written to Heroku's file system, updating and installing plugins or themes should be done locally and then pushed to Heroku.

## Updating

pull the entire app into local:
`
heroku git:clone -a ancient-reef-55168
`
or
`heroku git:clone -a <your app name>`
then
`cd <your app name`
`git remote add origin  https://git.heroku.com/ancient-reef-55168.git`
or 
`git remote add origin  https://git.heroku.com/<your app name>.git`
finally:
` git pull origin master`

Or you could do it by `git clone git@heroku.com:<your app name>.git`

**Note**: if use deply button (<img src="https://www.herokucdn.com/deploy/button.png" alt="Deploy">) you can not use ```heroku git:clone -a <your app name>``` or ``` git pull origin master```
  

Updating your WordPress version is just a matter of merging the updates into
the branch created from the installation.

    $ git pull # Get the latest

Using the same branch name from our installation:

    $ git checkout production
    $ git merge master # Merge latest
    $ git push heroku production:master

WordPress needs to update the database. After push, navigate to:

    http://your-app-url.herokuapp.com/wp-admin

WordPress will prompt for updating the database. After that you'll be good
to go.

## Deployment optimisation

If you have files that you want tracked in your repo, but do not need deploying (for example, *.md, *.pdf, *.zip). Then add path or linux file match to the `.slugignore` file & these will not be deployed.

Examples:
```
path/to/ignore/
bin/
*.md
*.pdf
*.zip
```

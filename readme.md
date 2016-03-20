# Deployment

## Learning Objectives

- Define 'deployment', and contrast different methods of deploying an application
- Describe the difference between development, test, and production environments
- Deploy a rails application using heroku
- Run migrations on heroku
- Debug errors on heroku (using logs)
- Describe the major points of a `12-factor` application as applied to deployment
- Use environment variables to keep sensitive data out of code
- List common pitfalls and their solutions when deploying to heroku
- Describe the role of the asset pipeline in rails

## Framing

Deployment is the act of putting our app up on one or more servers connected to
the internet, such that people can use our app.

## [You Read: Rails Environments](about-environments.md) (10 min)

## [You Read: Deployment](about-deployment.md) (10 min)

## You Read: The Heroku Rails Guide (15 min)

Take 10 minutes to pair up with a partner and skim the [Getting Started with
Rails 4 on Heroku](https://devcenter.heroku.com/articles/getting-started-with-rails4).

We'll take 5 minutes for discussion, and for much of the rest of this lesson,
we'll be walking through many of these steps.

## [You Do: Deploying to Heroku](deploying-your-first-app.md) (30 min)

## [You Read: Common Errors](common-errors.md) (10 min)

## [Optional We Do: Rails Asset Pipeline](asset-pipeline.md) (30 min)

## Recap

The whole series of commands for deploying to Heroku is:

Add to the bottom of your Gemfile:

```rb
group :production do
  gem 'rails_12factor'
end
```

```bash
$ bundle install
$ git add .
$ git commit -m "added 12factor"
$ heroku create my-sweet-app
# wait...
$ git push heroku master
# wait...
$ heroku run rake db:migrate
$ heroku run rake db:seed
$ heroku open
```

Then, to view your app's server log:

```bash
$ heroku logs -t
```

## To change your app

```bash
$ git add .
$ git commit -m "your message"
$ git push heroku master
```

Note that this will *not* update Github. If you want to push your changes to Github as well, you need to run `git push origin master` as usual.

## To change your migrations

Do *not* edit an existing migration file. Instead:

```bash
$ rails g migration yourMigrationName
# Edit the new migration file
$ rake db:migrate
$ git add .
$ git commit -m "added migration"
$ git push heroku master
$ heroku run rake db:migrate
```

## To switch your Heroku app over to development

```bash
$ heroku config:set RAILS_ENV=development
```

...and to change it back:

```bash
$ heroku config:set RAILS_ENV=production
```

## Deleting apps

You're likely to end up with a bunch of Heroku apps. To delete all of them at once, you can add this function to your `.bash_profile`:

```sh
function happ(){
  for app in $(heroku apps)
    do heroku apps:destroy --app $app --confirm $app
  done
}
```

...and then run `happ` from anywhere on your computer.

## Resources
- [Getting Started Deploying Rails on Heroku](https://devcenter.heroku.com/articles/getting-started-with-rails4)
- [The Twelve-Factor App](http://12factor.net)
- Screencasts
  - WDI7
    - [Part 1](https://youtu.be/8NZsSxFSFLM)
    - [Part 2](https://youtu.be/EFDy2sAHFCw)
    - [Part 3](https://youtu.be/nx1gAA9tyog)
- 12Factor: What is it?
  > You may be wondering about the name of that gem... it's based on an idea called
  'The Twelve-Factor App', which is a set of 12 principles that modern apps should
  follow so that they can be deployed on any provider, and can scale up easily
  (i.e. can grow as the userbase grows).

  > We don't have time to go in-depth today, but you can find more info about this
  idea on the [Twelve-Factor Site](http://12factor.net).
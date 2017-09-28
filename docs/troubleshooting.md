## Troubleshooting

### `ERROR: YOUR SITE COULD NOT BE BUILT` During install, either locally or on Heroku.

You likely need to add `exclude: [vendor]` to `_config.yml` in your branch's root directory (create the file if it does not exist already). If you still have problems on the *local* install, you may have better luck using `bundle install --deployment`, but be sure to add the resulting 'vendor' directory to .gitignore. For completeness, the full error may look something like this:


```
remote:        Configuration file: none
remote:                     ERROR: YOUR SITE COULD NOT BE BUILT:
remote:                            ------------------------------------
remote:                            Invalid date '0000-00-00': Post '/vendor/bundle/ruby/2.0.0/gems/jekyll-2.5.3/lib/site_template/_posts/0000-00-00-welcome-to-jekyll.markdown.erb' does not have a valid date in the filename.
```

### Pushing to heroku

If you are working from a new GitHub-cloned repo (where you have not run `heroku create`), you may also want to push to Heroku. Instead of adding the remote in the standard way with Git, do this:


```
heroku git:remote -a my-site
```

### Upgrading from Jekyll Auth &lt; 0.1.0

1. `cd` to your project directory
2. `rm config.ru`
3. `rm Procfile`
4. Remove any Jekyll Auth specific requirements from your `Gemfile`
5. Follow [the instructions above](https://github.com/benbalter/jekyll-auth#add-jekyll-auth-to-your-site) to get started
6. When prompted, select "n" if Heroku is already set up

### Solving "Not Found" or 404 issues after Heroku Deploy

If you've completely configured the app but after deploying to Heroku you're receiving "Not Found" or 404 status codes the issue may be a missing `Procfile`. Because you're serving the application from the `jekyll-auth` server you have to explicitly tell Heroku to use that server versus the default server (WEBrick) it would use. 

* In your root file create a `Procfile` and put `web: bundle exec jekyll-auth serve --port $PORT --host 0.0.0.0` at the top of this file. 
* The `web` key declares the command `bundle exec jekyll-auth serve --port $PORT --host 0.0.0.0` when it instantiates a web dyno. 
* The `--port $PORT` will use the port whatever the Heroku environment sets when instantiating the dyno. 
* `--host 0.0.0.0` will take advantage of the host that the jekyll-auth app is living on.

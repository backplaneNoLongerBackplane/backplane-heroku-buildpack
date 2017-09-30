# \\\ Backplane Heroku Buildpack

NOTE: This buildpack is currently experimental.

For use with backplane.io in conjunction with heroku.com.

## How it works

The Backplane buildpack will do the following things at compile time:

* Install the latest agent
* Install a .profile.d/backplane.sh script - This will start the agent at runtime.

At runtime:

* Start the Backplane agent `backplane connect` - This will get its
	  labels from the BACKPLANE_LABELS environment variable and its
authentication token from BACKPLANE_TOKEN.

## Use

### Setup backplane routing

If you haven't already created a route now is a good time to do so:

	$ backplane route myendpoint.com "some=label"

### Add the backplane buildpack

Add the Backplane buildpack to the list of buildpacks for your Heroku app and
configure:

	$ heroku buildpacks:add https://github.com/backplane/backplane-heroku-buildpack
	$ heroku config:set BACKPLANE_LABELS="some=label" $(backplane generate env | grep TOKEN)

You are now ready to deploy:

	$ git push heroku master

> NOTE: You may now turn off inbound traffic from Heroku's router by changing
> your process name from `web` to `www` or whatever you see fit. Each process
> should still bind to `127.0.0.1:$PORT` where the backplane agent will be
> looking for it.

Test http://myendpoint.com.

For more information on setting up backplane see `backplane help`.

### Ignore process types

Use enviroment variable BACKPLANE_IGNORE_PROCESS_TYPE to ignore process types
which do not serve http traffic.

Example:

	$ heroku config:set BACKPLANE_IGNORE_PROCESS_TYPE=run,worker,jobqueue


## Known Issues

### Fetch Error

At times Heroku may report:

	Push rejected, failed to detect set buildpack https://github.com/backplane/backplane-heroku-buildpack

This happens intermittently without changes to the buildpack and "fixes itself"
after some time due to what we believe to be issues on Heroku's end. Please
contact support@backplane.io and also file a ticket with [Heroku
support](https://help.heroku.com) the issue does not resolve itself in a timely
manner.

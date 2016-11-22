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

If you haven't already created a route now is a good time to do so:

	$ backplane route myendpoint.com "some=label"

Add the Backplane buildpack to the list of buildpacks for your Heroku app and
configure:

	$ heroku buildpacks:add https://github.com/backplaneio/backplane-heroku-buildpack
	$ heroku config:set BACKPLANE_LABELS="some:label" BACKPLANE_TOKEN=$(backplane generate env | grep TOKEN)

You are now ready to deploy:

	$ git push heroku master

> NOTE: You may now turn off inbound traffic from Heroku's router by changing
> your process name from `web` to `www` or whatever you see fit. Each process
> should still bind to `127.0.0.1:$PORT` where the backplane agent will be
> looking for it.

Test http://myendpoint.com.

For more information on setting up backplane see `backplane help`.

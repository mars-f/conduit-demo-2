# conduit-demo
Interactive demo of Mozilla's code submission pipeline.

## Running the demo

 1. Edit your `/etc/hosts` file so that brower traffic to the `phabricator.dev`
    and `bmo.dev` addresses is sent to your docker host. (e.g. `127.0.0.1` on
    Linux or the docker-machine IP on OSX.)
 1. Run `docker-compose run demo`
 1. In the shell opened by `docker-compose run demo`, create a repo and use 
    `arc diff` to submit a review.
 1. Visit `http://phabricator.dev:7788` in your browser and log in with
    `user:phab` and `password:phab` to work with your shiny new review.


## Updating the pre-loaded demo

As noted in [this Phabricator ticket](https://secure.phabricator.com/T5310),
the only way we can set up an out-of-the-box demo of Phabricator is to pre-load
the application database with the settings we want.

To update the pre-loaded database with new settings:
 
 1. **Important:** Run `docker-compose down` and 
    `docker volume rm conduitdemo_phabricator-mysql-db` to ensure you have a 
    fresh DB!
 1. Start the application with `docker-compose up` and log in with the 
    appropriate user ("admin" to update global settings, "phab" for 
    things like API keys).
 1. Change the desired setting.
 1. Run `docker-compose run phabricator dump > demo.sql` to dump the
    database.
 1. Edit `demo.sql` and delete the extra shell output at the beginning and at
    the end of the file.
 1. `gzip demo.sql`
 1. `cp demo.sql.gz docker/phabricator/demo.sql.gz`
 1. Submit a [PR](https://github.com/mozilla-conduit/conduit-demo/pulls)
    with the changes.


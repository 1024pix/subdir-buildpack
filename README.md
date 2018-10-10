# Subdir buildpack

This is a mashup of https://github.com/Pagedraw/heroku-buildpack-select-subdir and
https://github.com/timanovsky/subdir-heroku-buildpack. The API is that of
https://github.com/Pagedraw/heroku-buildpack-select-subdir but the implementation, taken
from https://github.com/timanovsky/subdir-heroku-buildpack, actually moves all files from
the designated subdirectory to the build root before invoking the requested buildpack.

Allows you to have a monolithic repo with multiple Heroku/Scalingo apps in different
subdirectories (like Google!).

## Usage

* Write a bunch of Procfiles and scatter them throughout your code base.
* Create a bunch of Heroku/Scalingo apps.
* For each app do `heroku buildpacks:set -a <app> https://github.com/1024pix/subdir-buildpack` /
  `scalingo env-set -a <app> BUILDPACK_URL=https://github.com/1024pix/subdir-buildpack`
* For each app, specify a `<subdir>` and a buildpack by running
`heroku config:add BUILDPACK='<subdir>=https://github.com/desired/heroku-buildpack' -a <app>` /
`scalingo env-set -a <app> BUILDPACK='<subdir>=https://github.com/desired/heroku-buildpack'`
* Deploy your app as usual (`git push`, automatic deploy…)

# How it works

The buildpack takes subdirectory you configured, erases everything else, and
copies that subdirectory to project root. Then it downloads the requested buildpack and
hands the slug compilation process over to it.

# Subdir buildpack

This is a mashup of https://github.com/Scalingo/multi-buildpack and
https://github.com/timanovsky/subdir-heroku-buildpack.

This buildpack uses the `.buildpacks` file as
https://github.com/Scalingo/multi-buildpack does, but before that, like
https://github.com/timanovsky/subdir-heroku-buildpack, it moves all files from
the subdirectory designated by the `BUILDPACK_SUBDIR` variable to the build
root.

Allows you to have a monolithic repo (like Google!) with multiple
Heroku/Scalingo apps in different subdirectories.

## Usage

* For each app that lives in your repository, create a `.buildpacks` file in the app's
directory listing the required buildpack(s) for this app as described
in https://github.com/Scalingo/multi-buildpack/blob/master/README.md
* Create a bunch of Heroku/Scalingo apps to host your repository's apps.
* For each app do `heroku buildpacks:set -a <app> https://github.com/1024pix/subdir-buildpack` /
  `scalingo env-set -a <app> BUILDPACK_URL=https://github.com/1024pix/subdir-buildpack`
* For each app, specify a `<subdir>` by running
`heroku config:add BUILDPACK_SUBDIR='<subdir>' -a <app>` /
`scalingo env-set -a <app> BUILDPACK='<subdir>'`
* Deploy your app as usual (`git push`, automatic deploy…)

# How it works

The buildpack takes the subdirectory you configured, erases everything else, and
copies that subdirectory to project root. Then it downloads each of the
requested buildpacks and hands the slug compilation process over to them.

If `BUILDPACK_SUBDIR` is not set, this buildpack will behave like
https://github.com/Scalingo/multi-buildpack .

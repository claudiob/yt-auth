Authenticate users with their Google account
============================================

Yt::Auth lets you easily authenticate users of your website by means of
their Google-based email address.

With Yt::Auth, it is easy to limit access to your app to a few users without
the need for them to create a username and password.

The **source code** is available on [GitHub](https://github.com/claudiob/yt-auth) and the **documentation** on [RubyDoc](http://www.rubydoc.info/gems/yt-auth/frames).

[![Build Status](http://img.shields.io/travis/claudiob/yt-auth/master.svg)](https://travis-ci.org/claudiob/yt-auth)
[![Coverage Status](http://img.shields.io/coveralls/claudiob/yt-auth/master.svg)](https://coveralls.io/r/claudiob/yt-auth)
[![Dependency Status](http://img.shields.io/gemnasium/claudiob/yt-auth.svg)](https://gemnasium.com/claudiob/yt-auth)
[![Code Climate](http://img.shields.io/codeclimate/github/claudiob/yt-auth.svg)](https://codeclimate.com/github/claudiob/yt-auth)
[![Online docs](http://img.shields.io/badge/docs-✓-green.svg)](http://www.rubydoc.info/gems/yt-auth/frames)
[![Gem Version](http://img.shields.io/gem/v/yt-auth.svg)](http://rubygems.org/gems/yt-auth)

Yt::Auth.url_for
----------------

With the `url_for` class method, you can obtain a URL where to redirect users
who need to authenticate with their Google account in order to use your
application:

```ruby
redirect_uri = 'https://example.com/auth' # REPLACE WITH REAL ONE
scope = %i(yt-analytics.readonly youtube)
Yt::Auth.url_for(redirect_uri: redirect_uri, scope: scope, force: true)
 # => https://accounts.google.com/o/oauth2/auth?client_id=...&scope=email&redirect_uri=https%3A%2F%2Fexample.com%2Fauth&response_type=code
```

Yt::Auth.create
----------------

After users have authenticated with their Google account, they will be
redirected to the `redirect_uri` you indicated, with an extra `code` query
parameter, e.g. `https://example.com/auth?code=1234`

With the `create` class method, you can create an instance for that
authentication:

```ruby
redirect_uri = 'https://example.com/auth' # REPLACE WITH REAL ONE
code = 'dfwe7r9djd234ffdjf3009dfknfd98re' # REPLACE WITH REAL ONE
auth = Yt::Auth.create(redirect_uri: redirect_uri, code: code)
 # => #<Yt::Auth:0x007fe61d…>
```

Yt::Auth#email
--------------

Once you have an instance of `Yt::Auth`, you can obtain the verified email
of the authenticated user:

```ruby
auth.email
 # => "user@example.com"
```

Yt::Auth#access_token
---------------------

Once you have an instance of `Yt::Auth`, you can also obtain the access token
of the authenticated user:

```ruby
auth.access_token
 # => "ya29.df8er8e9r89er"
```

Yt::Auth#refresh_token
----------------------

Once you have an instance of `Yt::Auth`, you can also obtain the refresh token
of the authenticated user:

```ruby
auth.refresh_token
 # => "sdf7f7erre98df"
```

Yt::Auth.find_by
----------------

If you already know the refresh token of a Google account, you can obtain its
complete authentication object:

```ruby
auth = Auth.find_by(refresh_token: "sdf7f7erre98df")
auth.email
 # => "user@example.com"
```


Yt::HTTPError
-------------

`Yt::HTTPError` will be raised whenever something goes wrong during the
authentication process. The message of the error will include the details:

```ruby
redirect_uri = 'https://example.com/auth' # REPLACE WITH REAL ONE
code = 'this-is-not-a-valid-code'
Yt::Auth.new(redirect_uri: redirect_uri, code: code).email
 # => Yt::HTTPError: Invalid authorization code.
```

How to contribute
=================

Contribute to the code by forking the project, adding the missing code,
writing the appropriate tests and submitting a pull request.

To run the tests correctly, set up the environment variables `YT_ACCOUNT_CLIENT_ID`
and `YT_ACCOUNT_CLIENT_SECRET` with the credentials of an existing Google OAuth app.

In order for a PR to be approved, all the tests need to pass and all the public
methods need to be documented and listed in the guides. Remember:

- to run all tests locally: `bundle exec rspec`
- to generate the docs locally: `bundle exec yard`
- to list undocumented methods: `bundle exec yard stats --list-undoc`

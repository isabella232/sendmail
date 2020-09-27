# sendmail

[![Build Status][gh-actions-badge]][gh-actions] [![Erlang Versions][erlang badge]][versions] [![Tags][github tags badge]][github tags]

[![][logo]][logo-large]

*Richard Carlsson's sendmail for Erlang, packaged*


## Contents

* [Introduction](#introduction-)
* [Dependencies](#dependencies-)
* [Features](#features-)
* [Using sendmail](#using-sendmail-)
* [License](#license-)


## Introduction [&#x219F;](#contents)

This is a fork of Richard Carlsson's project, packaged so that it's easily useable in Erlang/LFE release-based projects.

It is based on `sendmail.erl` by Klacke and `smtp.erl` by Johan Bevemyr, with code for RFC1522 by `Håkan Stenholm`. Major cleanup and rewrites by Richard Carlsson. Single argument-as-map support added by Duncan McGreggor (as well as some test and deprecation message cleanups).

## Dependencies [&#x219F;](#contents)

* Erlang must be installed (only versions 19 and above are tested; due to the use of `timestamp` instead of the deprecated `now` function, versions of Erlang older than 18 will not be able to use the latest release).
* `sendmail` must be installed and is expected to be at `/usr/sbin/sendmail`.

`rebar3` is not expected or required, but this project is structured according to rebar3 project standards.

## Features [&#x219F;](#contents)

* Create email payloads
* Send email payloads
* Send message (wraps payload creation and sending)

## Using `sendmail` [&#x219F;](#contents)

### New API

The new API just uses a map. Create a message:

```erlang
1> Data = sendmail:create(#{
           to => "alice@example.com",
           from => "bob@example.com",
           subject => "Hey, Alice!",
           message => "Where's Carol?"}).
```
```erlang
["Subject: =?ISO-8859-1?Q?Hey=2C_Alice=21?=\n",
 "From: alice@example.com\n","To: bob@example.com\n",
 ["Content-Type: text/plain\n",
  "Content-Transfer-Encoding: 8bit\n","\n","Where's Carol?"]]
```

Send a message:

```erlang
2> {ExitCode, CmdOutput} = sendmail:send(#{
           to => "alice@example.com",
           from => "bob@example.com",
           subject => "Hey, Alice!",
           message => "Where's Carol?"}).
```
```erlang
{0,[]}
```

### Old API

Create a payload:

```erlang
3> Data = sendmail:create("alice@example.com", "bob@example.com", "Hey, Alice!",
      "Where's Carol?").
```
```erlang
["Subject: =?ISO-8859-1?Q?Hey=2C_Alice=21?=\n",
 "From: alice@example.com\n","To: bob@example.com\n",
 ["Content-Type: text/plain\n",
  "Content-Transfer-Encoding: 8bit\n","\n","Where's Carol?"]]
```

Send a message directly:

```erlang
4> {ExitCode, CmdOutput} = sendmail:send("alice@example.com", "bob@example.com",
      "Hey, Alice!", "Where's Carol?").
```
```erlang
{0,[]}
```

## License [&#x219F;](#contents)

BSD 3-Clause License

Copyright © 2004, Johan Bevemyr

Copyright © 2005, Klacke <klacke@hyber.org>

Copyright © 2009, Håkan Stenholm

Copyright © 2009, Richard Carlsson

Copyright © 2020, Duncan McGreggor

<!-- Named page links below: /-->

[logo]: priv/images/logo-small.png
[logo-large]: priv/images/logo-large.png
[org]: https://github.com/lfe-rebar3
[github]: https://github.com/erlsci/sendmail
[gh-actions-badge]: https://github.com/erlsci/sendmail/workflows/ci%2Fcd/badge.svg
[gh-actions]: https://github.com/erlsci/sendmail/actions
[erlang badge]: https://img.shields.io/badge/erlang-19%20to%2023-blue.svg
[versions]: https://github.com/erlsci/sendmail/blob/master/.github/workflows/cicd.yml
[github tags]: https://github.com/erlsci/sendmail/tags
[github tags badge]: https://img.shields.io/github/tag/erlsci/sendmail.svg
[github downloads]: https://img.shields.io/github/downloads/atom/atom/total.svg

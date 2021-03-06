---
layout: post
title: "Brakeman 3.6.2 Released"
date: 2017-05-19 09:01
comments: true
categories: 
---

*Changes since 3.6.1:*

* Remove `--rake` option
* By default, do not honor additional check paths in config
* Properly handle template names without `.html` or `.js`
* Catch YAML parsing errors in session settings check ([#1046](https://github.com/presidentbeef/brakeman/issues/1046))
* Better handling of `if` expressions in HAML rendering ([#1032](https://github.com/presidentbeef/brakeman/issues/1032))
* Avoid warning about SQLi with `to_s` in `exists?` ([#1045](https://github.com/presidentbeef/brakeman/issues/1045))
* Handle safe call operator in checks ([#1031](https://github.com/presidentbeef/brakeman/issues/1031))
* Handle empty `if` expressions when finding return values
* Set template file names during rendering for better errors
* Limit Slim dependency to before 3.0.8
* Update RubyParser to 3.9.0

### Rake Option Removed

The Rake task generated by `--rake` has caused quite a few problems. When Rake is run with a Rails application, it loads all of the app's dependencies, which may conflict with Brakeman's dependencies.

It is recommended to either not use a Rake task to run Brakeman or just shell out to Brakeman instead of using it as a library.

([changes](https://github.com/presidentbeef/brakeman/pull/1038))

### Check Paths in Config Files

Brakeman allows loading custom checks with `--add-checks-path`.
To avoid silently loading arbitary code, Brakeman will not support this option in configuration files unless explicitly enabled with `--allow-check-paths-in-config`.

([changes](https://github.com/presidentbeef/brakeman/pull/1052))

### Templates without Format Extension

The [3.5.0 release](http://brakemanscanner.org/blog/2017/01/31/brakeman-3-dot-5-0-released/) added support for templates with a bare extension (like `my_template.haml`) but template names derived internally did not handle these bare extensions properly. When rendering templates, Brakeman was not able to match render names to the correct files.

([changes](https://github.com/presidentbeef/brakeman/pull/1041))

### YAML Errors

When checking session settings, Brakeman parses `config/secrets.yml`. Sometimes this file has unsafe values or interpolated code which causes the parsing to fail. Brakeman will now only output a notice about this failure instead of an error.

([changes](https://github.com/presidentbeef/brakeman/pull/1047))

### If Expressions in HAML

Typically Brakeman assumes all `if` branches in templates are taken and ignores the condition. This was not happening in all cases in rendered HAML templates.

([changes](https://github.com/presidentbeef/brakeman/pull/1035/files))

### `to_s` False Positive with `exists?`

Brakeman will no longer warn about arguments calling `to_s` in `exists?`, since that is the recommended way to avoid SQL injection with that particular method.

([changes](https://github.com/presidentbeef/brakeman/pull/1049))

### Better Safe Call Handling

The safe call operation `&.` will be handled better in all checks instead of being ignored.

([changes](https://github.com/presidentbeef/brakeman/pull/1033))

### Empty Ifs

This release fixes an issue when finding return values from methods ending in an empty `if` expression.

([changes](https://github.com/presidentbeef/brakeman/pull/1053))

### More Template Names

Template file names will now be set when passing code to template rendering libraries, in order to produce better error messages when something goes wrong.

([changes](https://github.com/presidentbeef/brakeman/pull/1042))

### Dependencies

RubyParser has been updated to `3.9.0` which resolves some issues.

([changes](https://github.com/presidentbeef/brakeman/pull/1048))

Slim is limited to `<3.0.8` since the `3.0.8` gem requires Ruby 2.0.

([changes](https://github.com/presidentbeef/brakeman/pull/1050))


### Checksums

The SHA256 sums for this release are:

    ba89440a5e94f463ad9b6f3602e83d16313857753a5cc9b754757bd3e58e2202  brakeman-3.6.2.gem
    adae09f9aa3a4d311fe2de41fee5d9b821eff600c1c05e314b3b930adb85b4d7  brakeman-min-3.6.2.gem
    d3da0a86dedcee84c35a14e00b7a9d22874aed89d7d031d1fe60b68ce4ae7c7a  brakeman-lib-3.6.2.gem

### Reporting Issues

Thank you to everyone who reported bugs and contributed to this release.

Please report any [issues](https://github.com/presidentbeef/brakeman/issues) with this release! Take a look at [this guide](https://github.com/presidentbeef/brakeman/wiki/How-to-Report-a-Brakeman-Issue) to reporting Brakeman problems.

Follow [@brakeman](https://twitter.com/brakeman) on Twitter and hang out [on Gitter](https://gitter.im/presidentbeef/brakeman) for questions and discussion.

If you find Brakeman valuable and want to support its development, check out [Brakeman Pro](https://brakemanpro.com/).

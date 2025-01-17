# AWS CloudWatch log CLI viewer [![Latest version](https://img.shields.io/crates/v/cw-axe.svg)](https://crates.io/crates/cw-axe/)
Axe is another tool to get logs from AWS CloudWatch.

## Features
* multiple ways to define datetime range,
* build-in regex support,
* aliases support.

## Installation

The easiest option is to install [cargo through rustup](https://rustup.rs/) and then run `cargo install cw-axe`.

## Usage
### Global options
```
 -p, --prodile AWS profile
 -c, --config-path <CONFIG_PATH>
```
### Commands
```
  log      show logs
  groups   show log groups
  streams  show log group streams
  alias    add or rewrite alias, use with with -- after alias to pass args
  aliases  print all aliases
  help     Print this message or the help of the given subcommand(s)
```
### Log
```
Arguments:
  <GROUP>   group name
  <STREAM>  stream name

Options:
  -s, --start <START>
          start time, the time can be defines as
          * RFC 3339, ex:
              * 2024-01-02T03:04:05.678Z
              * 2024-01-02T03:04:05+1
          * offset from now, in days(d), hours(h), minutes(m), seconds(s) ex:
              * 10m - 10 minutes
              * 100 - 100 seconds
              * 1m30s
          * local time of day, ex:
              * 12:34
          * UTC time of day, ex:
              * 12:34Z
          * [default: 60m]
  -e, --end <END>
          end time, format is the same as for start
  -l, --length <LENGTH>
          either length or end is used, the format is same as offset for start
  -f, --filter <FILTER>
          AWS CloudWatch filter https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/FilterAndPatternSyntax.html
          Examples:
          * 'A B' - the log has to contain A and B
          * '"A B"' - exact match
          * '?A ?B' - matches with A or B
          * 'A -B' - matches with lines contains A and no B
          * '%\s[A-Z]{4}\s%' - regex
  -r, --message-regexp <MESSAGE_REGEXP>
          replace regexp
          '<delimiter char><regexp><the same delimiter char><replacement>'
          example: '/(\d{4} [^|]+/$1'
  -d, --datetime-format <DATETIME_FORMAT>
          ouput datetime format https://docs.rs/chrono/latest/chrono/format/strftime/index.html [default: "%d%b %H:%M:%S%.3f"]
  -u, --ui
          show results in UI
      --chunk-size <CHUNK_SIZE>
          number records in a chunk, maximum is 10k [default: 1000]
```
### Alias
When you want to save your command just create an alias the next way.
For `cw-axe -p my-profile log my-group my-setream -r '#^2024#24'` insert `alias <name> --` after `cw-axe`: `cw-axe alias my-alias --  -p my-profile log my-group my-setream -r '#^2024#24`.
And now you can just use `cw-axe my-alias`.

## Supported platforms
* Linux,
* macOS,
* Should work on other Unix-like systems.

It might work on Windows as well, but config is in the common Unix like directory, so it might requre some minor changes.

## Known bugs:
* AWS CloudWatch filter is not always working correctly, sometimes it misses some commands, but it's on the AWS side, so no way to fix it except get all events and grep.

## UI

 Results can be shown in the UI ([egui](https://github.com/emilk/egui)), if the `ui` feature is enabled. But currently it's limited to the current query results only. I have plans to implement a fully functional UI one day.
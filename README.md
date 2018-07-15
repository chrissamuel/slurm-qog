# slurm-qog
Perl script to generate "quality-of-guess" feedback for users about their requested Slurm walltimes.

https://github.com/chrissamuel/slurm-qog

This script counts up the requested walltime and actual walltime of all Slurm
jobs that are state COMPLETED or TIMEOUT.  This means we do not hold the user
responsible for jobs that crash or were cancelled for some reason as they
ended prior to their normal completion.

# Usage

```
$ qog --help
Usage: ./qog --help | --all --days [number-of-days] | --days [number-of-days] --user $USER --account $ACCOUNT
```

*You must have `sacct` in your $PATH of course!*

# Examples

For the root user it will summarise the QOG for the last 7 days of usage.

```
# qog 
WARNING: when run as root without options it defaults to --all
Capping look back to 7 days for --all

Over the last 7 days all jobs used 8,592 days 11:19:07 over 67,033 jobs
They requested a total time of 51,758 days 16:47:00

This equates to an overestimate of 6.0 times the actual usage
```

For a normal user it will default to summarising their last 30 days of usage.

```
$ qog

Over the last 30 days your 82 jobs requested 06:50:00 in time
These jobs only used 00:53:06 of total time

This equates to an overestimate of 7.7 times the actual usage

**** PLEASE REVIEW YOUR REQUESTED TIMELIMITS ****

```

A user can summarise over less time than the default.

```
$ ./qog --days=5

Over the last 5 days your jobs used 00:05:08 over 4 jobs
They requested a total time of 00:20:00

This equates to an overestimate of 3.9 times the actual usage

**** PLEASE REVIEW YOUR REQUESTED TIMELIMITS ****

```

You can also specify a Slurm account, if you are in that account (or root).

```
# ./qog --account=hpcadmin

Over the last 30 days their jobs used 01:39:54 over 96 jobs
They requested a total time of 13:40:00

This equates to an overestimate of 8.2 times the actual usage

```

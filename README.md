# No more cron generated emails!
My goal is to join an existing production engineering team and find zero cron generated emails in the team mailing lists.  This project is not intended to have a production ready solution, but more of a calls-to-arms to stop the emails!

## Introduction

[Cron](https://en.wikipedia.org/wiki/Cron) is default batch job schedule used on POSIX based systems.  Cron's secondary function is to generate thousands of unread emails.

## Known Problems

* [Crontabs](https://www.freebsd.org/cgi/man.cgi?query=crontab&format=html) are odd to maintain.
* Missing feature to only store job output when there is an execution error.
* Missing feature to enforce some kinda primitive job ordering.
* Using emails, to alert on job failures, does not scale to lots of machines and is often a source of toil.

## Proposed Solution

Use the [periodic](https://www.freebsd.org/cgi/man.cgi?query=periodic&format=html) utility, instead of [cron](https://www.freebsd.org/cgi/man.cgi?query=cron&format=html).  This will provide the missing features:
* Only store job output when there is an execution error -- see [man periodic.conf](https://www.freebsd.org/cgi/man.cgi?query=periodic.conf&format=html)
* Enforce some kinda primitive job ordering -- scripts are run in lexicographical order.
* Emails can be turned off -- see [man periodic.conf](https://www.freebsd.org/cgi/man.cgi?query=periodic.conf&format=html)

Use some extra scripts to:
* enforce a global serial execution ordering
* [reports job execution status](http://www.robustperception.io/monitoring-batch-jobs-in-python/) to the [Prometheus](http://prometheus.io/) monitoring system
* [use labels](http://www.robustperception.io/how-to-have-labels-for-machine-roles/) to indicate if a job is enabled or disable

## References

* [man cron](https://www.freebsd.org/cgi/man.cgi?query=cron&format=html)
* [man periodic](https://www.freebsd.org/cgi/man.cgi?query=periodic&format=html)
* [Prometheus](http://prometheus.io/)
* http://www.robustperception.io/monitoring-batch-jobs-in-python/
* http://www.robustperception.io/how-to-have-labels-for-machine-roles/

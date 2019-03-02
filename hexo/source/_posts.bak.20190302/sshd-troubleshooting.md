---
title: sshd troubleshooting
date: 2017-11-10 11:25:01
categories: ssh
tags: [ssh,Linux]
---

If you have ruled out any "external" factors, the following set of steps usually helps to narrow it down. So while this doesn't directly answer your question, it may help tracking down the error cause.

Troubleshooting sshd

What I find generally very useful in any such cases is to start sshd without letting it daemonize. The problem in my case was that neither syslog nor auth.log showed anything meaningful.

When I started it from the terminal I got:

```
# $(which sshd) -Ddp 10222
```

/etc/ssh/sshd_config line 8: address family must be specified before ListenAddress.
Much better! This error message allowed me to see what's wrong and fix it. Neither of the log files contained this output.

NB: at least on Ubuntu the $(which sshd) is the best method to satisfy sshd requirement of an absolute path. Otherwise you'll get the following error: sshd re-exec requires execution with an absolute path. The -p 10222 makes sshd listen on that alternative port, overriding the configuration file - this is so that it doesn't clash with potentially running sshd instances. Make sure to choose a free port here.

Finally: connect to the alternative port (ssh -p 10222 user@server).

This method has helped me many many times in finding issues, be it authentication issues or other types. To get really verbose output to stdout, use $(which sshd) -Ddddp 10222 (note the added dd to increase verbosity). For more debugging goodness check man sshd.


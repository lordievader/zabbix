# Design philosophy
This document tries to describe some of the design philosophies behind the
scripts in the `zabbix-scripts` repository.
These philosophies have occured to me while writing, and updating, the scripts.
By no means are they "rules", and if you disagree, or think of improvements,
please let me know.

## KISS
This may seem obvious, but the Keep-It-Simple-Stupid principle applies to
Zabbix extension scripts too. The main reason is that you want these scripts
to be as fast as possible. I have had it happen so many times that once a
server was busy a script took a bit longer than usual and got killed because it
timed out. From the perspective of the agent.

## Files over programs
I prefer to read out a file to get a certain metric than fire off a program. Of
course there are exceptions, lvm..., but typically it is easier to read a file
than start a new process to do, more or less, the same.

## Modular design
Since these are external scripts, there are two problems. What happens when the
monitored hosts doesn't know your item key? And what happens when it does know
the item key but doesn't use/have the subject you are trying to monitor?

The first question can not really be solved in code. I personally use Puppet to
make sure item keys are known. However, I am thinking of moving to a git based
approach, like the `zabbix-agent-config` repo.

The second question we can solve by assuming our code can fail. You always want
to return something. What you return upon error should make sense. For example,
when a discovery script fails you might want to return an empty set. True, this
could mean things fail silently. This is a tradeoff between many hosts
reporting your discovery is failing against a script silently failing.
For an item script you might return a sane value (0 is frequently sane), or an
error value (-1?) and adjust your template to raise a warning when such a value
is received.

## Combine where you can
Rather than having the Zabbix server request multiple similar values from a
host it is better to request all the data at once and define multiple dependent
items. Using the JSON functionality this can quite easily be done. However, the
script should output it in JSON.

Possible downside here is that you retrieve more than you asked for. If you
really only want <10% of all the data it would be wise to ignore this 'rule'
and only add the data you want.

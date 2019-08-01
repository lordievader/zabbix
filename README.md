# Zabbix
This repository forms the collection of all my Zabbix related repositories.
It consists of a bunch of submodules which pull in all the other repositories.
This way you are free to choose to pull in everything or a specific repository.

## zabbix-scripts
This is the bread and butter. Code for extending the agent goes in here.
Discovery scripts and item scripts, mainly.

## zabbix-agent-config
This repository contains the link between the `zabbix-scripts` repository and
the Zabbix Agent.

## zabbix-templates
In here template exports are given which make use of the above two
repositories.

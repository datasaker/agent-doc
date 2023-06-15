# Ansible Datasaker Role

The Ansible Datasaker role installs and configures the Datasaker Agent and integrations.

## Setup

### Requirements

- Requires Ansible v2.6+.
- Supports most Debian Linux distributions.

### Installation

Install the [Datasaker role] from Ansible Galaxy on your Ansible server:

```shell
ansible-galaxy install dsk_bot.datasaker
```

To deploy the Datasaker Agent on hosts, add the Datasaker role and your API key to your playbook:

```yml
- hosts: servers
  become: true
  roles:
    - role: dsk_bot.datasaker
  vars:
    datasaker_api_key: "<YOUR_API_KEY>"
    datasaker_agents: ["<AGENT_NAME>"]
```

#### Role variables

| Variable                                   | Description                                                                                                                                                                                                                                                                                                                                                        |
|--------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|`datasaker_api_key`|Your Datasaker API key.|
|`datasaker_agents`|Set to Datasaker Agent.<br>`dsk-node-agent` `dsk-trace-agent` `dsk-log-agent` `dsk-postgres-agent` `dsk-plan-postgres-agent`<br>(Default) `dsk-node-agent`|
|`datagate_url`|The site of the Datasaker intake to send Agent data to.<br>(Default) `gate.kr.datasaker.io`|
|`datagate_trace_url`|Override the `dsk-trace-agent` datagate url. <br>(Default) `datagate_url`|
|`datagate_trace_port`|Override the `dsk-trace-agent` datagate port. <br>(Default) `31300`|
|`datagate_trace_timeout`|Override the `dsk-trace-agent` data expiration time. <br>(Default) `5s`|
|`datagate_manifest_url`|Override the `dsk-manifest-agent` datagate url. <br>(Default) `datagate_url`|
|`datagate_manifest_port`|Override the `dsk-manifest-agent` datagate port. <br>(Default) `31301`|
|`datagate_manifest_timeout`|Override the `dsk-manifest-agent` data expiration time. <br>(Default) `5s`|
|`datagate_metric_url`|Override the `dsk-metric-agent` datagate url. <br>(Default) `datagate_url`|
|`datagate_metric_port`|Override the `dsk-metric-agent` datagate port. <br>(Default) `31302`|
|`datagate_metric_timeout`|Override the `dsk-metric-agent` data expiration time. <br>(Default) `5s`|
|`datagate_plan_url`|Override the `dsk-plan-agent` datagate url. <br>(Default) `datagate_url`|
|`datagate_plan_port`|Override the `dsk-plan-agent` datagate port. <br>(Default) `31303`|
|`datagate_plan_timeout`|Override the `dsk-plan-agent` data expiration time. <br>(Default) `5s`|
|`datagate_loggate_url`|Override the `dsk-log-agent` datagate url. <br>(Default) `datagate_url`|
|`datagate_loggate_port`|Override the `dsk-log-agent` datagate port. <br>(Default) `31304`|
|`datagate_loggate_timeout`|Override the `dsk-log-agent` data expiration time. <br>(Default) `5s`|
|`datasaker_api_url`|Override the datasaker api server url. <br>(Default) `api.kr.datasaker.io`|
|`datasaker_api_send_interval`|Override the datasaker api server data expiration time. <br>(Default) `1m`|

## Uninstallation

However for more control over the uninstall parameters, the following code can be used.
In this example:

```yml
- hosts: servers
  become: true
  roles:
    - role: dsk_bot.datasaker
  vars:
    datasaker_agents: ["<AGENT_NAME>"]
    uninstall: True
    datasaker_clean: True
```

## Troubleshooting

### Debian stretch

**Note:** 

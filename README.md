Sumo Logic Agent
=========

This role installs to Sumo Logic collector agent on RHEL-based Linux.

Requirements
------------

The Sumo Logic-provided RPM includes all software dependencies, however, it would be worth reviewing the [current sys reqs.](https://help.sumologic.com/Start-Here/01About-Sumo-Logic/System-Requirements/Installed-Collector-Requirements)

Role Variables
--------------

Required:

* sumologic_rpm_url - For some reason Sumo's RPMs are pod-specific, meaning you have to download a different RPM depending on where your pod is hosted. I would recommend downloading the RPM for your [pod's region](https://help.sumologic.com/Send-Data/Installed-Collectors/05Reference-Information-for-Collector-Installation/02Download-a-Collector-from-a-Static-URL) and hosting it in S3 or something similar.
* sumologic_access_id - What it says on the tin.
* sumologic_access_key - Same as above.

Optional:

* sumologic_ephemeral_agent - Defaults to true. Otherwise, you'll be manually deleting un-used and non-existent collectors out of the Sumo console. If you'd prefer that, set to false.
* env_timezone - Defaults to UTC. If you'd like something, set using the TZ format.
* sumologic_tracked_logs - The linux secure, messages, and yum logs are tracked by default. 


Dependencies
------------

None

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

      ---
      - hosts: servers
        become: yes
        vars_files:
          - group_vars/env.yml
        roles:
           - { role: chrisdodds.sumologic-agent }

Inside your group_vars env file

    ---
    # env vars
    sumologic_rpm_url: 'https://bucketname.s3.aws.amazon.com/sumo_collector.rpm
    sumologic_access_id: 'XXXXXXXX'
    sumologic_access_key: 'XXXXXXX'
    sumologic_tracked_logs:
      - name: "nginx access" 
        description: "nginx access log"
        category: "application/prod/nginx"
        path: "/var/log/nginx/access.log
        filters: "{ json block per Sumo Logic's documentation }"        

License
-------

BSD

Author Information
------------------

Chris Dodds - @liquid_chickents - chrisdodds.net

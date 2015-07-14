ansible-riak
========

This role will setup Riak on a node. It will ***not*** build a cluster.  In order to build a cluster, use one of the example playbooks defined below.

## **NOTE!** Huge update coming on dev-2.x branches

Don't be surprised if this branch isn't working. Master will stay safe. 

## Description

[Riak](http://basho.com/riak/) is an open source, distributed database that focuses on high availability, horizontal scalability, and *predictable* latency.

This repository is **community supported**. We both appreciate and need your contribution to keep it stable. For more on how to contribute, [take a look at the contribution process](#contribution).

Thank you for being part of the community! We love you for it.

Requirements
------------

Tested with PackageCloud install of Riak on CentOS 6 & 7 and Ubuntu Precise.

Role Variables
--------------

If you have trouble seeing the tables below, please [read the documentation on Github](https://github.com/basho/ansible-riak/blob/master/README.md).


**Pay close attention** to the following variables as they will tune, affect, and possible **make your system unusable**.  They are settings to tune the operating system and filesystem.

| Name           | Default Value | Description                        |
| -------------- | ------------- | -----------------------------------|
| riak_tune_disks | no | enables  disk tunings |
| riak_filesystem | ext4         | filesystem used on Riak's data volume |
| riak_mountpoint| /             | mountpoint where Riak data is installed
| riak_partition |  /dev/mapper/VolGroup-lv_root   | device/partition that holds mounted Riak data
| riak_physical_disks| [sda]     | A list of the physical disks that make up Riak's data volume|


For example, if you installed Riak onto a system with a single root partition, then riak_mountpoint would be "/".  If you had a dedicated volume for riak data, riak_mountpoint would be "/path/to/that/mounted/volume."  

riak_partition: is the device that the riak_mountpoint is/will be mounted to.  You can gather that by running: 

	df /var/lib/riak

 
 riak_physical_disks is a list of physical disks that make up the riak_partition.  It could be one, or could be many depending if you are using, RAID, LVM, etc.
 
 If you wish to avoid any of the tuning, just leave riak_tune_disks to its default value of no.
 


### All of the Variables


Variables listed with "OS Specific" and "Install specific" have values defined in `vars/<ansible_os_family>.yml`.


| Name           | Default Value | Description                        |
| -------------- | ------------- | -----------------------------------|
| riak_aae       | on            | turn on Riak's active anti-entropy |
| riak_backend   | bitcask       | default backend for Riak           |
| riak_cluster_mgr_bind_ip| 0.0.0.0       | the IP address of the interface used for Riak cluster manager (riak-ee) |
| riak_custom_beams| false       | a path to pre-compiled custom beams |
| riak_custom_package| no     | specify a path on the Ansible control, or HTTP URL, to a custom Riak package.|
| riak_filesystem | ext4         | filesystem that's used on Riak's data volume |
| riak_handoff_port | 8099       | handoff port                       |
| riak_handoff_wait | 600       | The number of seconds to wait until hand offs are finished |   
| riak_http_bind_ip | 0.0.0.0          | The IP address used to listen for Riak HTTP requests. |
| riak_http_port | 8098          | port that Riak's http interface will listen |
| riak_iface     | eth0          | interface that Riak will use |
| riak_ip_addr   | "{{ hostvars[inventory_hostname]['ansible_' + riak_iface]['ipv4']['address'] }}"| shortcut for the ip address associated with riak_iface
| riak_log_rotate| 5             | number of days for log rotation |
| riak_mount_options |noatime,barrier=0,errors=remount-ro | optmized mount options for Riak |
| riak_mountpoint| /             | mountpoint where Riak data is installed|
| riak_net_speed| 1Gb             |speed of network which Riak using, used to optimize sysctl tunings.|
| riak_node_name | riak@{{ riak_ip_addr }} | the name you give the Riak node |
| riak_package_release| 1        | the package release version |
| riak_partition |  /dev/mapper/VolGroup-lv_root   | device/partition that holds mounted Riak data
| riak_pb_backlog| 256           | the number of pending protocol buffer connections |
| riak_pb_bind_ip   | 0.0.0.0          | the IP that is listening for protocol buffer connections
| riak_pb_port   | 8087          | the port that is listening for protocol buffer connections
| riak_physical_disks| [sda]     | A list of the physical disks that make up Riak's data volume(s).  Used in tuning some disk parameters. |
| riak_ring_size | 64            | the number of partitions in your Riak ring. |
| riak_scheduler | noop          | the disk scheduler to use for your Riak-related disks. |
| riak_search (solr) | "false"       | enable/disable for configuring Riak search. |
| riak_tune_disks | no | enables  disk tunings |
| riak_usr_lib   |OS specific    | the path to Riak libraries.


Playbooks
------------

There are some sample playbooks in the [examples/](https://github.com/basho/ansible-riak/tree/master/examples) directory as well as a typical hosts file.
Take a look at [setup_riak.yml](https://github.com/basho/ansible-riak/blob/master/examples/setup_riak.yml).

First we set the role to the riak_cluster group as defined in the inventory.  Then we call the [form_cluster.yml](https://github.com/basho/ansible-riak/blob/master/examples/form_cluster.yml) playbook to actually join the cluster.  The following happens:

* the Riak node name of the first node is established
* all nodes in the group join that node
* the last node performs a cluster plan, and cluster commit


	

Dependencies
------------

Depends on the [basho.riak-common](https://github.com/basho-labs/ansible-riak-common) role.


License
-------

Apache

##Contributions 

Basho Labs repos survive because of community contribution. Here’s how to get started.

* Fork the appropriate sub-projects that are affected by your change
* Create a topic branch for your change and checkout that branch
   `git checkout -b some-topic-branch`
* Make your changes and run the test suite if one is provided (see below)
* Commit your changes and push them to your fork
* Open a pull request for the appropriate project
* Contributors will review your pull request, suggest changes, and merge it when it’s ready and/or offer feedback
* To report a bug or issue, please open a new issue against this repository

### Maintainers
* Bryan Hunt ([GitHub](https://github.com/binarytemple), <bhunt@basho.com>)
* and You! [Read up](https://github.com/basho-labs/the-riak-community/blob/master/config-mgmt-strategy.md) and get involved

You can [read the full guidelines](http://docs.basho.com/riak/latest/community/bugs/) for bug reporting and code contributions on the Riak Docs. And **thank you!** Your contribution is incredible important to us.

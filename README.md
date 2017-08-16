1. Using Fake PXE Driver as RH-Enter-V doesnt support pxe-ssh:

Add fake_pxe driver to /etc/ironic/ironic.conf
sudo systemctl restart openstack-ironic-conductor 

##
# developer documentation online. (list value)
#enabled_drivers = pxe_ipmitool
enabled_drivers = pxe_ipmitool,fake_pxe,pxe_ssh,pxe_drac,pxe_ilo
##

2. Import nodes with:
 
openstack baremetal import instack.env

3. Incase of failures with PXE-Ironic check DNSMAQ service running

4. Run introspection on all nodes:
 
openstack overcloud node introspect --all-manageable --provide

5. SET BOOT_MODE=UEFI on computes in the ironic-node capabilities

SET BOOT_MODE=UEFI on computes
 
openstack baremetal node set --property capabilities="profile:compute,cpu_vt:true,cpu_hugepages:true,boot_option:local,cpu_txt:true,cpu_aes:true,cpu_hugepages_1g:true,boot_mode:uefi" ehccompute0002

6. Openstack Flavors:

#####################################################################################################


[stack@ehcdirector0001 ~]$ openstack flavor list
+--------------------------+---------------------------+------+------+-----------+-------+-----------+
| ID                       | Name                      |  RAM | Disk | Ephemeral | VCPUs | Is Public |
+--------------------------+---------------------------+------+------+-----------+-------+-----------+
| 354ef4f7-ff44-4bbe-      | c`ntrol                   | 4096 |   40 |         0 |     1 | True      |
| bb81-b6252cd809a8        |                           |      |      |           |       |           |
| 4d7f8ff6-dc5a-469d-      | contrailanalytics         | 4096 |   40 |         0 |     1 | True      |
| 8f64-71cb85335230        |                           |      |      |           |       |           |
| 7eed4e39-ffb5-42d0-9775- | baremetal                 | 4096 |   40 |         0 |     1 | True      |
| a42b8b9d6d89             |                           |      |      |           |       |           |
| 927ffc7b-d150-495d-944b- | ceph-storage              | 4096 |   40 |         0 |     1 | True      |
| bb769c535dc2             |                           |      |      |           |       |           |
| b4847756-5a08-4d7c-86bf- | contrailcontroller        | 4096 |   40 |         0 |     1 | True      |
| 699a96be5b4c             |                           |      |      |           |       |           |
| c771f8b5-d3b7-425d-802d- | block-storage             | 4096 |   40 |         0 |     1 | True      |
| feec8845a245             |                           |      |      |           |       |           |
| cba2db57-b264-4ece-a680- | compute                   | 4096 |   40 |         0 |     1 | True      |
| 7f199df74a81             |                           |      |      |           |       |           |
| e49d60f9-8024-43c9-b561- | contrailanalyticsdatabase | 4096 |   40 |         0 |     1 | True      |
| 63148d8c5082             |                           |      |      |           |       |           |
| f5aa914a-4b95-4cd1-ac0b- | swift-storage             | 4096 |   40 |         0 |     1 | True      |
| 59289f97d54d             |                           |      |      |           |       |           |
+--------------------------+---------------------------+------+------+-----------+-------+-----------+

[stack@ehcdirector0001 ~]$ openstack flavor show contrailcontroller
+----------------------------+-----------------------------------------------------------------------------+
| Field                      | Value                                                                       |
+----------------------------+-----------------------------------------------------------------------------+
| OS-FLV-DISABLED:disabled   | False                                                                       |
| OS-FLV-EXT-DATA:ephemeral  | 0                                                                           |
| access_project_ids         | None                                                                        |
| disk                       | 40                                                                          |
| id                         | b4847756-5a08-4d7c-86bf-699a96be5b4c                                        |
| name                       | contrailcontroller                                                          |
| os-flavor-access:is_public | True                                                                        |
| properties                 | capabilities:boot_option='local', capabilities:profile='contrailcontroller' |
| ram                        | 4096                                                                        |
| rxtx_factor                | 1.0                                                                         |
| swap                       |                                                                             |
| vcpus                      | 1                                                                           |
+----------------------------+-----------------------------------------------------------------------------+

[stack@ehcdirector0001 ~]$ openstack flavor show contrailanalytics
+----------------------------+----------------------------------------------------------------------------+
| Field                      | Value                                                                      |
+----------------------------+----------------------------------------------------------------------------+
| OS-FLV-DISABLED:disabled   | False                                                                      |
| OS-FLV-EXT-DATA:ephemeral  | 0                                                                          |
| access_project_ids         | None                                                                       |
| disk                       | 40                                                                         |
| id                         | 4d7f8ff6-dc5a-469d-8f64-71cb85335230                                       |
| name                       | contrailanalytics                                                          |
| os-flavor-access:is_public | True                                                                       |
| properties                 | capabilities:boot_option='local', capabilities:profile='contrailanalytics' |
| ram                        | 4096                                                                       |
| rxtx_factor                | 1.0                                                                        |
| swap                       |                                                                            |
| vcpus                      | 1                                                                          |
+----------------------------+----------------------------------------------------------------------------+

[stack@ehcdirector0001 ~]$ openstack flavor show contrailanalyticsdatabase
+----------------------------+------------------------------------------------------------------------------------+
| Field                      | Value                                                                              |
+----------------------------+------------------------------------------------------------------------------------+
| OS-FLV-DISABLED:disabled   | False                                                                              |
| OS-FLV-EXT-DATA:ephemeral  | 0                                                                                  |
| access_project_ids         | None                                                                               |
| disk                       | 40                                                                                 |
| id                         | e49d60f9-8024-43c9-b561-63148d8c5082                                               |
| name                       | contrailanalyticsdatabase                                                          |
| os-flavor-access:is_public | True                                                                               |
| properties                 | capabilities:boot_option='local', capabilities:profile='contrailanalyticsdatabase' |
| ram                        | 4096                                                                               |
| rxtx_factor                | 1.0                                                                                |
| swap                       |                                                                                    |
| vcpus                      | 1                                                                                  |
+----------------------------+------------------------------------------------------------------------------------+

[stack@ehcdirector0001 ~]$ openstack flavor show compute
+----------------------------+------------------------------------------------------------------+
| Field                      | Value                                                            |
+----------------------------+------------------------------------------------------------------+
| OS-FLV-DISABLED:disabled   | False                                                            |
| OS-FLV-EXT-DATA:ephemeral  | 0                                                                |
| access_project_ids         | None                                                             |
| disk                       | 40                                                               |
| id                         | cba2db57-b264-4ece-a680-7f199df74a81                             |
| name                       | compute                                                          |
| os-flavor-access:is_public | True                                                             |
| properties                 | capabilities:boot_option='local', capabilities:profile='compute' |
| ram                        | 4096                                                             |
| rxtx_factor                | 1.0                                                              |
| swap                       |                                                                  |
| vcpus                      | 1                                                                |
+----------------------------+------------------------------------------------------------------+

[stack@ehcdirector0001 ~]$ openstack flavor show control
+----------------------------+------------------------------------------------------------------+
| Field                      | Value                                                            |
+----------------------------+------------------------------------------------------------------+
| OS-FLV-DISABLED:disabled   | False                                                            |
| OS-FLV-EXT-DATA:ephemeral  | 0                                                                |
| access_project_ids         | None                                                             |
| disk                       | 40                                                               |
| id                         | 354ef4f7-ff44-4bbe-bb81-b6252cd809a8                             |
| name                       | control                                                          |
| os-flavor-access:is_public | True                                                             |
| properties                 | capabilities:boot_option='local', capabilities:profile='control' |
| ram                        | 4096                                                             |
| rxtx_factor                | 1.0                                                              |
| swap                       |                                                                  |
| vcpus                      | 1                                                                |
+----------------------------+------------------------------------------------------------------+

#####################################################################################################

7. Set Flavor, Properties and Capabilities to Flavor:

openstack flavor create contrailcontroller --disk 40 --ram 4096 --vcpus 1

openstack flavor create contrailanalyticsdatabase —disk 40 —ram 4096 —vcpus 1

openstack flavor create contrailanalytics —disk 40 —ram 4096 -vcpus 1

openstack flavor create contrailtsn —disk 40 —ram 4096 —vcpus 1



openstack flavor set --property capabilities:boot_option="local" contrailcontroller

openstack flavor set --property capabilities:boot_option="local" contrailanalytics

openstack flavor set --property capabilities:boot_option="local" contrailanalyticsdatabase

openstack flavor set --property capabilities:boot_option="local" contrailtsn



openstack flavor set --property capabilities:profile='contrailanalyticsdatabase' contrailanalyticsdatabase

openstack flavor set --property capabilities:profile='contrailanalytics' contrailanalytics

openstack flavor set --property capabilities:profile='contrailcontroller' contrailcontroller

openstack flavor set --property capabilities:profile='contrailtsn' contrailtsn



8. OPenstack Baremetal Node List:

#####################################################################################################

[stack@ehcdirector0001 ~]$ openstack baremetal node list
+--------------------------------------+---------------------------+--------------------------------------+-------------+--------------------+-------------+
| UUID                                 | Name                      | Instance UUID                        | Power State | Provisioning State | Maintenance |
+--------------------------------------+---------------------------+--------------------------------------+-------------+--------------------+-------------+
| baaf1caa-96fe-41e6-8ce7-13f64a2340d6 | ehccontroller0001         | 45acc506-8c3f-4eed-a418-8624bd2600f6 | power on    | active             | False       |
| 1623d501-5618-40bd-8355-d7a849b26a6c | ehccontroller0002         | None                                 | power off   | available          | True        |
| 1cce500e-2104-4e1b-85d6-e11add0d42a7 | ehccontroller0003         | None                                 | power off   | available          | True        |
| 4ef18c3e-4509-41e8-9b3e-6b78e580c5e4 | ehccompute0001            | None                                 | power off   | available          | True        |
| beea12a7-bf60-4208-9de4-4015cc2ea0cc | ehccompute0002            | 13a089a5-7f3f-424a-85a7-c1fe4347aae0 | power on    | active             | False       |
| 168818af-998b-4a44-a671-542f01ad2ec7 | ehccompute0003            | None                                 | power off   | available          | True        |
| 0190f512-7100-4722-afb5-db96815af3dc | ehccompute0004            | None                                 | power off   | available          | True        |
| 010f294b-0459-4e66-bb61-3ce6729b9e36 | ehcceph0001               | None                                 | power off   | available          | True        |
| 2b67b62b-3e7f-4ab9-82af-ce5d041cde19 | ehcceph0002               | None                                 | power off   | available          | True        |
| 55604d57-caae-4ede-ba56-b11c4b238ba8 | ehcceph0003               | None                                 | power off   | available          | True        |
| 2d5b412d-4e91-4be1-a417-6d095532d471 | ehccontrail-controller01  | e2514c2b-c909-4d56-9d50-4b6465b33776 | power on    | active             | False       |
| 3b178799-79fc-4fd0-bfd2-81a2ee5dfd39 | ehccontrail-analytics01   | d27c08a0-d3dc-4508-b7a0-820b0e8f968d | power on    | active             | False       |
| 92c46740-a9ca-4723-8493-9e17f955ec4a | ehccontrail-analyticsdb01 | 7fe085a2-f29b-4e53-bce7-2b3961473abf | power on    | active             | False       |
+--------------------------------------+---------------------------+--------------------------------------+-------------+--------------------+-------------+

[stack@ehcdirector0001 ~]$ openstack baremetal node show ehccontrail-controller01
+------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------+
| Field                  | Value                                                                                                                                         |
+------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------+
| clean_step             | {}                                                                                                                                            |
| console_enabled        | False                                                                                                                                         |
| created_at             | 2017-08-05T21:03:29+00:00                                                                                                                     |
| driver                 | fake_pxe                                                                                                                                      |
| driver_info            | {u'deploy_ramdisk': u'864b682e-b47c-4504-9e0e-9fcdb3bb5417', u'deploy_kernel': u'd6bf8df4-e08c-49fd-9697-4db8b1069383'}                       |
| driver_internal_info   | {u'agent_url': u'http://192.168.2.135:9999', u'root_uuid_or_disk_id': u'fa9e939e-9e3c-4f1c-a07c-3f506756ad7b', u'is_whole_disk_image': False, |
|                        | u'agent_last_heartbeat': 1501983443}                                                                                                          |
| extra                  | {u'hardware_swift_object': u'extra_hardware-2d5b412d-4e91-4be1-a417-6d095532d471'}                                                            |
| inspection_finished_at | None                                                                                                                                          |
| inspection_started_at  | None                                                                                                                                          |
| instance_info          | {u'root_gb': u'40', u'display_name': u'overcloud-contrailcontroller-0', u'image_source': u'951302b9-fbf3-4f15-822b-832883a7d7fc',             |
|                        | u'capabilities': u'{"profile": "contrailcontroller", "boot_option": "local"}', u'memory_mb': u'4096', u'vcpus': u'1', u'local_gb': u'299',    |
|                        | u'configdrive': u'******', u'swap_mb': u'0'}                                                                                                  |
| instance_uuid          | e2514c2b-c909-4d56-9d50-4b6465b33776                                                                                                          |
| last_error             | None                                                                                                                                          |
| maintenance            | False                                                                                                                                         |
| maintenance_reason     | None                                                                                                                                          |
| name                   | ehccontrail-controller01                                                                                                                      |
| ports                  | [{u'href': u'http://192.168.2.110:13385/v1/nodes/2d5b412d-4e91-4be1-a417-6d095532d471/ports', u'rel': u'self'}, {u'href':                     |
|                        | u'http://192.168.2.110:13385/nodes/2d5b412d-4e91-4be1-a417-6d095532d471/ports', u'rel': u'bookmark'}]                                         |
| power_state            | power on                                                                                                                                      |
| properties             | {u'memory_mb': u'65536', u'cpu_arch': u'x86_64', u'local_gb': u'299', u'cpus': u'6', u'capabilities':                                         |
|                        | u'profile:contrailcontroller,cpu_vt:true,cpu_hugepages:true,boot_option:local,cpu_txt:true,cpu_aes:true,cpu_hugepages_1g:true'}               |
| provision_state        | active                                                                                                                                        |
| provision_updated_at   | 2017-08-06T01:38:26+00:00                                                                                                                     |
| raid_config            | {}                                                                                                                                            |
| reservation            | None                                                                                                                                          |
| states                 | [{u'href': u'http://192.168.2.110:13385/v1/nodes/2d5b412d-4e91-4be1-a417-6d095532d471/states', u'rel': u'self'}, {u'href':                    |
|                        | u'http://192.168.2.110:13385/nodes/2d5b412d-4e91-4be1-a417-6d095532d471/states', u'rel': u'bookmark'}]                                        |
| target_power_state     | None                                                                                                                                          |
| target_provision_state | None                                                                                                                                          |
| target_raid_config     | {}                                                                                                                                            |
| updated_at             | 2017-08-06T01:38:26+00:00                                                                                                                     |
| uuid                   | 2d5b412d-4e91-4be1-a417-6d095532d471                                                                                                          |
+------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------+


[stack@ehcdirector0001 ~]$ openstack baremetal node show ehccontrail-analytics01
+------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------+
| Field                  | Value                                                                                                                                         |
+------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------+
| clean_step             | {}                                                                                                                                            |
| console_enabled        | False                                                                                                                                         |
| created_at             | 2017-08-05T21:03:29+00:00                                                                                                                     |
| driver                 | fake_pxe                                                                                                                                      |
| driver_info            | {u'deploy_ramdisk': u'864b682e-b47c-4504-9e0e-9fcdb3bb5417', u'deploy_kernel': u'd6bf8df4-e08c-49fd-9697-4db8b1069383'}                       |
| driver_internal_info   | {u'agent_url': u'http://192.168.2.129:9999', u'root_uuid_or_disk_id': u'fa9e939e-9e3c-4f1c-a07c-3f506756ad7b', u'is_whole_disk_image': False, |
|                        | u'agent_last_heartbeat': 1501983436}                                                                                                          |
| extra                  | {u'hardware_swift_object': u'extra_hardware-3b178799-79fc-4fd0-bfd2-81a2ee5dfd39'}                                                            |
| inspection_finished_at | None                                                                                                                                          |
| inspection_started_at  | None                                                                                                                                          |
| instance_info          | {u'root_gb': u'40', u'display_name': u'overcloud-contrailanalytics-0', u'image_source': u'951302b9-fbf3-4f15-822b-832883a7d7fc',              |
|                        | u'capabilities': u'{"profile": "contrailanalytics", "boot_option": "local"}', u'memory_mb': u'4096', u'vcpus': u'1', u'local_gb': u'299',     |
|                        | u'configdrive': u'******', u'swap_mb': u'0'}                                                                                                  |
| instance_uuid          | d27c08a0-d3dc-4508-b7a0-820b0e8f968d                                                                                                          |
| last_error             | None                                                                                                                                          |
| maintenance            | False                                                                                                                                         |
| maintenance_reason     | None                                                                                                                                          |
| name                   | ehccontrail-analytics01                                                                                                                       |
| ports                  | [{u'href': u'http://192.168.2.110:13385/v1/nodes/3b178799-79fc-4fd0-bfd2-81a2ee5dfd39/ports', u'rel': u'self'}, {u'href':                     |
|                        | u'http://192.168.2.110:13385/nodes/3b178799-79fc-4fd0-bfd2-81a2ee5dfd39/ports', u'rel': u'bookmark'}]                                         |
| power_state            | power on                                                                                                                                      |
| properties             | {u'memory_mb': u'65536', u'cpu_arch': u'x86_64', u'local_gb': u'299', u'cpus': u'6', u'capabilities':                                         |
|                        | u'profile:contrailanalytics,cpu_vt:true,cpu_hugepages:true,boot_option:local,cpu_txt:true,cpu_aes:true,cpu_hugepages_1g:true'}                |
| provision_state        | active                                                                                                                                        |
| provision_updated_at   | 2017-08-06T01:38:18+00:00                                                                                                                     |
| raid_config            | {}                                                                                                                                            |
| reservation            | None                                                                                                                                          |
| states                 | [{u'href': u'http://192.168.2.110:13385/v1/nodes/3b178799-79fc-4fd0-bfd2-81a2ee5dfd39/states', u'rel': u'self'}, {u'href':                    |
|                        | u'http://192.168.2.110:13385/nodes/3b178799-79fc-4fd0-bfd2-81a2ee5dfd39/states', u'rel': u'bookmark'}]                                        |
| target_power_state     | None                                                                                                                                          |
| target_provision_state | None                                                                                                                                          |
| target_raid_config     | {}                                                                                                                                            |
| updated_at             | 2017-08-06T01:38:18+00:00                                                                                                                     |
| uuid                   | 3b178799-79fc-4fd0-bfd2-81a2ee5dfd39                                                                                                          |
+------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------+


[stack@ehcdirector0001 ~]$ openstack baremetal node show ehccontrail-analyticsdb01
+------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------+
| Field                  | Value                                                                                                                                         |
+------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------+
| clean_step             | {}                                                                                                                                            |
| console_enabled        | False                                                                                                                                         |
| created_at             | 2017-08-05T21:03:29+00:00                                                                                                                     |
| driver                 | fake_pxe                                                                                                                                      |
| driver_info            | {u'deploy_ramdisk': u'864b682e-b47c-4504-9e0e-9fcdb3bb5417', u'deploy_kernel': u'd6bf8df4-e08c-49fd-9697-4db8b1069383'}                       |
| driver_internal_info   | {u'agent_url': u'http://192.168.2.122:9999', u'root_uuid_or_disk_id': u'fa9e939e-9e3c-4f1c-a07c-3f506756ad7b', u'is_whole_disk_image': False, |
|                        | u'agent_last_heartbeat': 1501983439}                                                                                                          |
| extra                  | {u'hardware_swift_object': u'extra_hardware-92c46740-a9ca-4723-8493-9e17f955ec4a'}                                                            |
| inspection_finished_at | None                                                                                                                                          |
| inspection_started_at  | None                                                                                                                                          |
| instance_info          | {u'root_gb': u'40', u'display_name': u'overcloud-contrailanalyticsdatabase-0', u'image_source': u'951302b9-fbf3-4f15-822b-832883a7d7fc',      |
|                        | u'capabilities': u'{"profile": "contrailanalyticsdatabase", "boot_option": "local"}', u'memory_mb': u'4096', u'vcpus': u'1', u'local_gb':     |
|                        | u'299', u'configdrive': u'******', u'swap_mb': u'0'}                                                                                          |
| instance_uuid          | 7fe085a2-f29b-4e53-bce7-2b3961473abf                                                                                                          |
| last_error             | None                                                                                                                                          |
| maintenance            | False                                                                                                                                         |
| maintenance_reason     | None                                                                                                                                          |
| name                   | ehccontrail-analyticsdb01                                                                                                                     |
| ports                  | [{u'href': u'http://192.168.2.110:13385/v1/nodes/92c46740-a9ca-4723-8493-9e17f955ec4a/ports', u'rel': u'self'}, {u'href':                     |
|                        | u'http://192.168.2.110:13385/nodes/92c46740-a9ca-4723-8493-9e17f955ec4a/ports', u'rel': u'bookmark'}]                                         |
| power_state            | power on                                                                                                                                      |
| properties             | {u'memory_mb': u'65536', u'cpu_arch': u'x86_64', u'local_gb': u'299', u'cpus': u'6', u'capabilities':                                         |
|                        | u'profile:contrailanalyticsdatabase,cpu_vt:true,cpu_hugepages:true,boot_option:local,cpu_txt:true,cpu_aes:true,cpu_hugepages_1g:true'}        |
| provision_state        | active                                                                                                                                        |
| provision_updated_at   | 2017-08-06T01:38:22+00:00                                                                                                                     |
| raid_config            | {}                                                                                                                                            |
| reservation            | None                                                                                                                                          |
| states                 | [{u'href': u'http://192.168.2.110:13385/v1/nodes/92c46740-a9ca-4723-8493-9e17f955ec4a/states', u'rel': u'self'}, {u'href':                    |
|                        | u'http://192.168.2.110:13385/nodes/92c46740-a9ca-4723-8493-9e17f955ec4a/states', u'rel': u'bookmark'}]                                        |
| target_power_state     | None                                                                                                                                          |
| target_provision_state | None                                                                                                                                          |
| target_raid_config     | {}                                                                                                                                            |
| updated_at             | 2017-08-06T01:38:22+00:00                                                                                                                     |
| uuid                   | 92c46740-a9ca-4723-8493-9e17f955ec4a                                                                                                          |
+------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------+


[stack@ehcdirector0001 ~]$ openstack baremetal node show ehccompute0002
+------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------+
| Field                  | Value                                                                                                                                         |
+------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------+
| clean_step             | {}                                                                                                                                            |
| console_enabled        | False                                                                                                                                         |
| created_at             | 2017-07-23T21:32:00+00:00                                                                                                                     |
| driver                 | pxe_ipmitool                                                                                                                                  |
| driver_info            | {u'ipmi_password': u'******', u'ipmi_address': u'10.241.162.82', u'deploy_ramdisk': u'864b682e-b47c-4504-9e0e-9fcdb3bb5417',                  |
|                        | u'deploy_kernel': u'd6bf8df4-e08c-49fd-9697-4db8b1069383', u'ipmi_username': u'administrator'}                                                |
| driver_internal_info   | {u'agent_url': u'http://192.168.2.131:9999', u'root_uuid_or_disk_id': u'fa9e939e-9e3c-4f1c-a07c-3f506756ad7b', u'is_whole_disk_image': False, |
|                        | u'agent_last_heartbeat': 1501983680}                                                                                                          |
| extra                  | {u'hardware_swift_object': u'extra_hardware-beea12a7-bf60-4208-9de4-4015cc2ea0cc'}                                                            |
| inspection_finished_at | None                                                                                                                                          |
| inspection_started_at  | None                                                                                                                                          |
| instance_info          | {u'root_gb': u'40', u'display_name': u'overcloud-novacompute-0', u'image_source': u'951302b9-fbf3-4f15-822b-832883a7d7fc', u'capabilities':   |
|                        | u'{"profile": "compute", "boot_option": "local"}', u'memory_mb': u'4096', u'vcpus': u'1', u'local_gb': u'278', u'configdrive': u'******',     |
|                        | u'swap_mb': u'0'}                                                                                                                             |
| instance_uuid          | 13a089a5-7f3f-424a-85a7-c1fe4347aae0                                                                                                          |
| last_error             | None                                                                                                                                          |
| maintenance            | False                                                                                                                                         |
| maintenance_reason     | None                                                                                                                                          |
| name                   | ehccompute0002                                                                                                                                |
| ports                  | [{u'href': u'http://192.168.2.110:13385/v1/nodes/beea12a7-bf60-4208-9de4-4015cc2ea0cc/ports', u'rel': u'self'}, {u'href':                     |
|                        | u'http://192.168.2.110:13385/nodes/beea12a7-bf60-4208-9de4-4015cc2ea0cc/ports', u'rel': u'bookmark'}]                                         |
| power_state            | power on                                                                                                                                      |
| properties             | {u'cpu_arch': u'x86_64', u'root_device': {u'serial': u'600508b1001c7d83639f83279dd24816'}, u'cpus': u'32', u'capabilities':                   |
|                        | u'profile:compute,cpu_vt:true,cpu_hugepages:true,boot_option:local,cpu_txt:true,cpu_aes:true,cpu_hugepages_1g:true,boot_mode:uefi',           |
|                        | u'memory_mb': u'393216', u'local_gb': u'278'}                                                                                                 |
| provision_state        | active                                                                                                                                        |
| provision_updated_at   | 2017-08-06T01:44:01+00:00                                                                                                                     |
| raid_config            | {}                                                                                                                                            |
| reservation            | None                                                                                                                                          |
| states                 | [{u'href': u'http://192.168.2.110:13385/v1/nodes/beea12a7-bf60-4208-9de4-4015cc2ea0cc/states', u'rel': u'self'}, {u'href':                    |
|                        | u'http://192.168.2.110:13385/nodes/beea12a7-bf60-4208-9de4-4015cc2ea0cc/states', u'rel': u'bookmark'}]                                        |
| target_power_state     | None                                                                                                                                          |
| target_provision_state | None                                                                                                                                          |
| target_raid_config     | {}                                                                                                                                            |
| updated_at             | 2017-08-06T01:44:01+00:00                                                                                                                     |
| uuid                   | beea12a7-bf60-4208-9de4-4015cc2ea0cc                                                                                                          |
+------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------+


[stack@ehcdirector0001 ~]$ openstack baremetal node show ehccontroller0001
+------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------+
| Field                  | Value                                                                                                                                         |
+------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------+
| clean_step             | {}                                                                                                                                            |
| console_enabled        | False                                                                                                                                         |
| created_at             | 2017-07-23T21:32:00+00:00                                                                                                                     |
| driver                 | pxe_ipmitool                                                                                                                                  |
| driver_info            | {u'ipmi_password': u'******', u'ipmi_address': u'10.241.162.16', u'deploy_ramdisk': u'864b682e-b47c-4504-9e0e-9fcdb3bb5417',                  |
|                        | u'deploy_kernel': u'd6bf8df4-e08c-49fd-9697-4db8b1069383', u'ipmi_username': u'administrator'}                                                |
| driver_internal_info   | {u'agent_url': u'http://192.168.2.128:9999', u'root_uuid_or_disk_id': u'fa9e939e-9e3c-4f1c-a07c-3f506756ad7b', u'is_whole_disk_image': False, |
|                        | u'agent_last_heartbeat': 1501983628}                                                                                                          |
| extra                  | {u'hardware_swift_object': u'extra_hardware-baaf1caa-96fe-41e6-8ce7-13f64a2340d6'}                                                            |
| inspection_finished_at | None                                                                                                                                          |
| inspection_started_at  | None                                                                                                                                          |
| instance_info          | {u'root_gb': u'40', u'display_name': u'overcloud-controller-0', u'image_source': u'951302b9-fbf3-4f15-822b-832883a7d7fc', u'capabilities':    |
|                        | u'{"profile": "control", "boot_option": "local"}', u'memory_mb': u'4096', u'vcpus': u'1', u'local_gb': u'278', u'configdrive': u'******',     |
|                        | u'swap_mb': u'0'}                                                                                                                             |
| instance_uuid          | 45acc506-8c3f-4eed-a418-8624bd2600f6                                                                                                          |
| last_error             | None                                                                                                                                          |
| maintenance            | False                                                                                                                                         |
| maintenance_reason     | None                                                                                                                                          |
| name                   | ehccontroller0001                                                                                                                             |
| ports                  | [{u'href': u'http://192.168.2.110:13385/v1/nodes/baaf1caa-96fe-41e6-8ce7-13f64a2340d6/ports', u'rel': u'self'}, {u'href':                     |
|                        | u'http://192.168.2.110:13385/nodes/baaf1caa-96fe-41e6-8ce7-13f64a2340d6/ports', u'rel': u'bookmark'}]                                         |
| power_state            | power on                                                                                                                                      |
| properties             | {u'cpu_arch': u'x86_64', u'root_device': {u'serial': u'600508b1001c51ac37dc579332232db3'}, u'cpus': u'32', u'capabilities':                   |
|                        | u'profile:control,cpu_vt:true,cpu_hugepages:true,boot_option:local,cpu_txt:true,cpu_aes:true,cpu_hugepages_1g:true', u'memory_mb': u'393216', |
|                        | u'local_gb': u'278'}                                                                                                                          |
| provision_state        | active                                                                                                                                        |
| provision_updated_at   | 2017-08-06T01:43:11+00:00                                                                                                                     |
| raid_config            | {}                                                                                                                                            |
| reservation            | None                                                                                                                                          |
| states                 | [{u'href': u'http://192.168.2.110:13385/v1/nodes/baaf1caa-96fe-41e6-8ce7-13f64a2340d6/states', u'rel': u'self'}, {u'href':                    |
|                        | u'http://192.168.2.110:13385/nodes/baaf1caa-96fe-41e6-8ce7-13f64a2340d6/states', u'rel': u'bookmark'}]                                        |
| target_power_state     | None                                                                                                                                          |
| target_provision_state | None                                                                                                                                          |
| target_raid_config     | {}                                                                                                                                            |
| updated_at             | 2017-08-06T01:43:11+00:00                                                                                                                     |
| uuid                   | baaf1caa-96fe-41e6-8ce7-13f64a2340d6                                                                                                          |
+------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------+

Commands to set state:

openstack baremetal node maintenance unset ehccompute0003

openstack baremetal node maintenance set ehccompute0002

openstack baremetal node set --property capabilities="profile:compute,cpu_vt:true,cpu_hugepages:true,boot_option:local,cpu_txt:true,cpu_aes:true,cpu_hugepages_1g:true,boot_mode:uefi" ehccompute0002

openstack baremetal node set --property capabilities="profile:compute,cpu_vt:true,cpu_hugepages:true,boot_option:local,cpu_txt:true,cpu_aes:true,cpu_hugepages_1g:true,boot_mode:uefi" ehccompute0003


9. If vrouter-agent is inactive:

ldconfig
restart the vrouter-agent service

10. To check the failures in stack overcloud:

openstack stack failures list overcloud

11. Abort Introspection

openstack baremetal introspection abort ehccontrail-controller01

12. List all the heat resources

heat resource-list overcloud

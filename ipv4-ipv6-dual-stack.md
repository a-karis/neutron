[root@rhel7 ~(keystone_demo)]# neutron net-create dual-stack-net
Created a new network:
+-----------------+--------------------------------------+
| Field           | Value                                |
+-----------------+--------------------------------------+
| admin_state_up  | True                                 |
| id              | 2562dc8c-e27d-4739-bdae-20907e0d5166 |
| mtu             | 0                                    |
| name            | dual-stack-net                       |
| router:external | False                                |
| shared          | False                                |
| status          | ACTIVE                               |
| subnets         |                                      |
| tenant_id       | d41f16f76ac4416f9b78317d90ae50f7     |
+-----------------+--------------------------------------+
[root@rhel7 ~(keystone_demo)]# neutron subnet-create^C
[root@rhel7 ~(keystone_demo)]# neutron subnet-create --name ipv4-dhcp-subnet dual-stack-net 10.10.10.0/24
Created a new subnet:
+-------------------+------------------------------------------------+
| Field             | Value                                          |
+-------------------+------------------------------------------------+
| allocation_pools  | {"start": "10.10.10.2", "end": "10.10.10.254"} |
| cidr              | 10.10.10.0/24                                  |
| dns_nameservers   |                                                |
| enable_dhcp       | True                                           |
| gateway_ip        | 10.10.10.1                                     |
| host_routes       |                                                |
| id                | 6c5ca4d5-1ae7-4613-80c9-ebaa500a5085           |
| ip_version        | 4                                              |
| ipv6_address_mode |                                                |
| ipv6_ra_mode      |                                                |
| name              | ipv4-dhcp-subnet                               |
| network_id        | 2562dc8c-e27d-4739-bdae-20907e0d5166           |
| subnetpool_id     |                                                |
| tenant_id         | d41f16f76ac4416f9b78317d90ae50f7               |
+-------------------+------------------------------------------------+
[root@rhel7 ~(keystone_demo)]# neutron subnet-create --ip-version 6 --ipv6-ra-mode dhcpv6-stateful --ipv6-address-mode dhcpv6-stateful --name ipv6-dhcp-subnet  dual-stack-net 2000:0:0:2::/64
Created a new subnet:
+-------------------+------------------------------------------------------------------+
| Field             | Value                                                            |
+-------------------+------------------------------------------------------------------+
| allocation_pools  | {"start": "2000:0:0:2::2", "end": "2000::2:ffff:ffff:ffff:fffe"} |
| cidr              | 2000:0:0:2::/64                                                  |
| dns_nameservers   |                                                                  |
| enable_dhcp       | True                                                             |
| gateway_ip        | 2000:0:0:2::1                                                    |
| host_routes       |                                                                  |
| id                | da268129-5726-497b-a6f7-9078dba52f67                             |
| ip_version        | 6                                                                |
| ipv6_address_mode | dhcpv6-stateful                                                  |
| ipv6_ra_mode      | dhcpv6-stateful                                                  |
| name              | ipv6-dhcp-subnet                                                 |
| network_id        | 2562dc8c-e27d-4739-bdae-20907e0d5166                             |
| subnetpool_id     |                                                                  |
| tenant_id         | d41f16f76ac4416f9b78317d90ae50f7                                 |
+-------------------+------------------------------------------------------------------+
[root@rhel7 ~(keystone_demo)]# neutron router-create dual-stack-router
Created a new router:
+-----------------------+--------------------------------------+
| Field                 | Value                                |
+-----------------------+--------------------------------------+
| admin_state_up        | True                                 |
| external_gateway_info |                                      |
| id                    | 94d373e9-4a39-48cb-ae8c-58df97a7cc34 |
| name                  | dual-stack-router                    |
| routes                |                                      |
| status                | ACTIVE                               |
| tenant_id             | d41f16f76ac4416f9b78317d90ae50f7     |
+-----------------------+--------------------------------------+
[root@rhel7 ~(keystone_demo)]# neutron router-interface-add dual-stack-router da268129-5726-497b-a6f7-9078dba52f67
Added interface d4c4c3d4-67a6-4634-aac0-d05b346062ec to router dual-stack-router.
[root@rhel7 ~(keystone_demo)]# neutron router-interface-add dual-stack-router 6c5ca4d5-1ae7-4613-80c9-ebaa500a5085
Added interface c8733cd6-b36c-48fa-94c4-f57864d07e51 to router dual-stack-router.
[root@rhel7 ~(keystone_demo)]# nova boot --image web-server --flavor m1.web --nic net-id=2562dc8c-e27d-4739-bdae-20907e0d5166 --key-name test ipv6-test
+--------------------------------------+---------------------------------------------------+
| Property                             | Value                                             |
+--------------------------------------+---------------------------------------------------+
| OS-DCF:diskConfig                    | MANUAL                                            |
| OS-EXT-AZ:availability_zone          |                                                   |
| OS-EXT-STS:power_state               | 0                                                 |
| OS-EXT-STS:task_state                | scheduling                                        |
| OS-EXT-STS:vm_state                  | building                                          |
| OS-SRV-USG:launched_at               | -                                                 |
| OS-SRV-USG:terminated_at             | -                                                 |
| accessIPv4                           |                                                   |
| accessIPv6                           |                                                   |
| adminPass                            | LsRkPuiPUo75                                      |
| config_drive                         |                                                   |
| created                              | 2016-02-29T18:39:15Z                              |
| flavor                               | m1.web (6)                                        |
| hostId                               |                                                   |
| id                                   | 672df080-48ef-477d-af27-f6b1169e3759              |
| image                                | web-server (9c0d8a6f-0fa3-4637-9013-60367941af51) |
| key_name                             | test                                              |
| metadata                             | {}                                                |
| name                                 | ipv6-test                                         |
| os-extended-volumes:volumes_attached | []                                                |
| progress                             | 0                                                 |
| security_groups                      | default                                           |
| status                               | BUILD                                             |
| tenant_id                            | d41f16f76ac4416f9b78317d90ae50f7                  |
| updated                              | 2016-02-29T18:39:15Z                              |
| user_id                              | 48e2602129284c609696273c993bb3bf                  |
+--------------------------------------+---------------------------------------------------+
[root@rhel7 ~(keystone_demo)]# nova list
+--------------------------------------+-----------+--------+------------+-------------+------------------------------------------+
| ID                                   | Name      | Status | Task State | Power State | Networks                                 |
+--------------------------------------+-----------+--------+------------+-------------+------------------------------------------+
| 672df080-48ef-477d-af27-f6b1169e3759 | ipv6-test | ACTIVE | -          | Running     | dual-stack-net=10.10.10.3, 2000:0:0:2::3 |
+--------------------------------------+-----------+--------+------------+-------------+------------------------------------------+

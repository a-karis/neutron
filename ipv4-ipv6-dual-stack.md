```
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

[root@rhel7 ~(keystone_admin)]# neutron net-list
neu+--------------------------------------+----------------+------------------------------------------------------+
| id                                   | name           | subnets                                              |
+--------------------------------------+----------------+------------------------------------------------------+
| 2562dc8c-e27d-4739-bdae-20907e0d5166 | dual-stack-net | 6c5ca4d5-1ae7-4613-80c9-ebaa500a5085 10.10.10.0/24   |
|                                      |                | da268129-5726-497b-a6f7-9078dba52f67 2000:0:0:2::/64 |
| e3b681e0-2ff4-45ba-bf2e-9f9769c03936 | public         | 71d7fd91-a415-402b-92e1-db919ee50138 172.24.4.224/28 |
| d0425727-8786-425e-8835-00991663e447 | private        | 81dad215-a0d2-4555-880d-fb44c44abd25 10.0.0.0/24     |
+--------------------------------------+----------------+------------------------------------------------------+
[root@rhel7 ~(keystone_admin)]# neutron subnet-list
+--------------------------------------+------------------+-----------------+------------------------------------------------------------------+
| id                                   | name             | cidr            | allocation_pools                                                 |
+--------------------------------------+------------------+-----------------+------------------------------------------------------------------+
| 6c5ca4d5-1ae7-4613-80c9-ebaa500a5085 | ipv4-dhcp-subnet | 10.10.10.0/24   | {"start": "10.10.10.2", "end": "10.10.10.254"}                   |
| 71d7fd91-a415-402b-92e1-db919ee50138 | public_subnet    | 172.24.4.224/28 | {"start": "172.24.4.228", "end": "172.24.4.238"}                 |
| 81dad215-a0d2-4555-880d-fb44c44abd25 | private_subnet   | 10.0.0.0/24     | {"start": "10.0.0.2", "end": "10.0.0.254"}                       |
| da268129-5726-497b-a6f7-9078dba52f67 | ipv6-dhcp-subnet | 2000:0:0:2::/64 | {"start": "2000:0:0:2::2", "end": "2000::2:ffff:ffff:ffff:fffe"} |
+--------------------------------------+------------------+-----------------+------------------------------------------------------------------+

[root@rhel7 ~(keystone_admin)]# neutron subnet-create --ip-version 6 --gateway 2000::1 public 2000::0/64
Created a new subnet:
+-------------------+----------------------------------------------------------+
| Field             | Value                                                    |
+-------------------+----------------------------------------------------------+
| allocation_pools  | {"start": "2000::2", "end": "2000::ffff:ffff:ffff:fffe"} |
| cidr              | 2000::/64                                                |
| dns_nameservers   |                                                          |
| enable_dhcp       | True                                                     |
| gateway_ip        | 2000::1                                                  |
| host_routes       |                                                          |
| id                | a9036931-a376-4cb1-8f70-bc3c2f2515bc                     |
| ip_version        | 6                                                        |
| ipv6_address_mode |                                                          |
| ipv6_ra_mode      |                                                          |
| name              |                                                          |
| network_id        | e3b681e0-2ff4-45ba-bf2e-9f9769c03936                     |
| subnetpool_id     |                                                          |
| tenant_id         | ded62b26f7d64284a981a6d8b2db4cc3                         |
+-------------------+----------------------------------------------------------+

root@rhel7 ~(keystone_admin)]# neutron router-gateway-set dual-stack-router public
Set gateway for router dual-stack-router
[root@rhel7 ~(keystone_admin)]# neutron router-list
+--------------------------------------+-------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------+-------+
| id                                   | name              | external_gateway_info                                                                                                                                                                                                                                                    | distributed | ha    |
+--------------------------------------+-------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------+-------+
| 211ced73-d16d-45bd-8c92-5ae435ff7239 | router1           | {"network_id": "e3b681e0-2ff4-45ba-bf2e-9f9769c03936", "enable_snat": true, "external_fixed_ips": [{"subnet_id": "71d7fd91-a415-402b-92e1-db919ee50138", "ip_address": "172.24.4.226"}, {"subnet_id": "a9036931-a376-4cb1-8f70-bc3c2f2515bc", "ip_address": "2000::3"}]} | False       | False |
| 94d373e9-4a39-48cb-ae8c-58df97a7cc34 | dual-stack-router | {"network_id": "e3b681e0-2ff4-45ba-bf2e-9f9769c03936", "enable_snat": true, "external_fixed_ips": [{"subnet_id": "71d7fd91-a415-402b-92e1-db919ee50138", "ip_address": "172.24.4.237"}, {"subnet_id": "a9036931-a376-4cb1-8f70-bc3c2f2515bc", "ip_address": "2000::5"}]} | False       | False |
+--------------------------------------+-------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-------------+-------+


[root@rhel7 ~(keystone_admin)]# neutron router-show dual-stack-router
+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Field                 | Value                                                                                                                                                                                                                                                                    |
+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| admin_state_up        | True                                                                                                                                                                                                                                                                     |
| distributed           | False                                                                                                                                                                                                                                                                    |
| external_gateway_info | {"network_id": "e3b681e0-2ff4-45ba-bf2e-9f9769c03936", "enable_snat": true, "external_fixed_ips": [{"subnet_id": "71d7fd91-a415-402b-92e1-db919ee50138", "ip_address": "172.24.4.237"}, {"subnet_id": "a9036931-a376-4cb1-8f70-bc3c2f2515bc", "ip_address": "2000::5"}]} |
| ha                    | False                                                                                                                                                                                                                                                                    |
| id                    | 94d373e9-4a39-48cb-ae8c-58df97a7cc34                                                                                                                                                                                                                                     |
| name                  | dual-stack-router                                                                                                                                                                                                                                                        |
| routes                |                                                                                                                                                                                                                                                                          |
| status                | ACTIVE                                                                                                                                                                                                                                                                   |
| tenant_id             | d41f16f76ac4416f9b78317d90ae50f7                                                                                                                                                                                                                                         |
+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
(reverse-i-search)`': ^C
[root@rhel7 ~(keystone_admin)]# ip netns exec qrouter-94d373e9-4a39-48cb-ae8c-58df97a7cc34 ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN 
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
67: qr-d4c4c3d4-67: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UNKNOWN 
    link/ether fa:16:3e:da:d0:f8 brd ff:ff:ff:ff:ff:ff
    inet6 2000:0:0:2::1/64 scope global 
       valid_lft forever preferred_lft forever
    inet6 fe80::f816:3eff:feda:d0f8/64 scope link 
       valid_lft forever preferred_lft forever
68: qr-c8733cd6-b3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UNKNOWN 
    link/ether fa:16:3e:bc:21:5f brd ff:ff:ff:ff:ff:ff
    inet 10.10.10.1/24 brd 10.10.10.255 scope global qr-c8733cd6-b3
       valid_lft forever preferred_lft forever
    inet6 fe80::f816:3eff:febc:215f/64 scope link 
       valid_lft forever preferred_lft forever
76: qg-b41a587b-35: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UNKNOWN 
    link/ether fa:16:3e:e9:23:d4 brd ff:ff:ff:ff:ff:ff
    inet 172.24.4.237/28 brd 172.24.4.239 scope global qg-b41a587b-35
       valid_lft forever preferred_lft forever
    inet6 2000::5/64 scope global 
       valid_lft forever preferred_lft forever
    inet6 fe80::f816:3eff:fee9:23d4/64 scope link 
       valid_lft forever preferred_lft forever
[root@rhel7 ~(keystone_admin)]# ip netns exec qrouter-94d373e9-4a39-48cb-ae8c-58df97a7cc34 ip r
default via 172.24.4.225 dev qg-b41a587b-35 
10.10.10.0/24 dev qr-c8733cd6-b3  proto kernel  scope link  src 10.10.10.1 
172.24.4.224/28 dev qg-b41a587b-35  proto kernel  scope link  src 172.24.4.237 
[root@rhel7 ~(keystone_admin)]# ip netns exec qrouter-94d373e9-4a39-48cb-ae8c-58df97a7cc34 ip -6 r
2000::/64 dev qg-b41a587b-35  proto kernel  metric 256 
2000:0:0:2::/64 dev qr-d4c4c3d4-67  proto kernel  metric 256 
fe80::/64 dev qr-d4c4c3d4-67  proto kernel  metric 256 
fe80::/64 dev qr-c8733cd6-b3  proto kernel  metric 256 
fe80::/64 dev qg-b41a587b-35  proto kernel  metric 256 
default via 2000::1 dev qg-b41a587b-35  metric 1024 

[root@rhel7 ~(keystone_demo)]# nova list
+--------------------------------------+-----------+--------+------------+-------------+------------------------------------------+
| ID                                   | Name      | Status | Task State | Power State | Networks                                 |
+--------------------------------------+-----------+--------+------------+-------------+------------------------------------------+
| 672df080-48ef-477d-af27-f6b1169e3759 | ipv6-test | ACTIVE | -          | Running     | dual-stack-net=10.10.10.3, 2000:0:0:2::3 |
+--------------------------------------+-----------+--------+------------+-------------+------------------------------------------+
[root@rhel7 ~(keystone_demo)]# nova floating-ip-create
+--------------------------------------+--------------+-----------+----------+--------+
| Id                                   | IP           | Server Id | Fixed IP | Pool   |
+--------------------------------------+--------------+-----------+----------+--------+
| 911df34b-3040-4564-bb3e-0d10fc37fd5a | 172.24.4.229 | -         | -        | public |
+--------------------------------------+--------------+-----------+----------+--------+
[root@rhel7 ~(keystone_demo)]# nova floating-ip-associate ipv6-test 172.24.4.238
ERROR (NotFound): floating ip not found (HTTP 404) (Request-ID: req-4ff86449-5e12-40dc-a407-34a9e602df73)
[root@rhel7 ~(keystone_demo)]# nova floating-ip-associate ipv6-test 172.24.4.229
[root@rhel7 ~(keystone_demo)]# nova list
+--------------------------------------+-----------+--------+------------+-------------+--------------------------------------------------------+
| ID                                   | Name      | Status | Task State | Power State | Networks                                               |
+--------------------------------------+-----------+--------+------------+-------------+--------------------------------------------------------+
| 672df080-48ef-477d-af27-f6b1169e3759 | ipv6-test | ACTIVE | -          | Running     | dual-stack-net=10.10.10.3, 2000:0:0:2::3, 172.24.4.229 |
+--------------------------------------+-----------+--------+------------+-------------+--------------------------------------------------------+

[root@rhel7 ~(keystone_demo)]# nova secgroup-list-rules default
+-------------+-----------+---------+-----------+--------------+
| IP Protocol | From Port | To Port | IP Range  | Source Group |
+-------------+-----------+---------+-----------+--------------+
| tcp         | 22        | 22      | ::/0      |              |
| tcp         | 22        | 22      | 0.0.0.0/0 |              |
| icmp        | -1        | -1      | 0.0.0.0/0 |              |
| icmp        | -1        | -1      | ::/0      |              |
|             |           |         |           | default      |
|             |           |         |           | default      |
+-------------+-----------+---------+-----------+--------------+


[root@rhel7 ~(keystone_demo)]# ping 172.24.4.229
PING 172.24.4.229 (172.24.4.229) 56(84) bytes of data.
64 bytes from 172.24.4.229: icmp_seq=1 ttl=63 time=4.79 ms

[root@rhel7 ~(keystone_demo)]# ping6 2000:0:0:2::3
PING 2000:0:0:2::3(2000:0:0:2::3) 56 data bytes
64 bytes from 2000:0:0:2::3: icmp_seq=1 ttl=64 time=5.68 ms



```




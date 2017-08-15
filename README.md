Ansible Role: Generate Self Signed Certificate
=========

This role help generate self signed certificate. It will create following combinations:

Case 1:
- ROOT CA
- Intermediate CA
- Server Cert (with SAN/without SAN)

Case 2:
- Root CA
- Server Cert (with SAN/without SAN)


Requirements
------------
yum

Role Variables
--------------

| Name                      | Default value                         |        Requird       | Description                                                                 |
|---------------------------|---------------------------------------|----------------------|-----------------------------------------------------------------------------|
| cert_base_dir             | /root/cert_base                       |         no           | Default Cert Base Directory                                                 |
| root_cert_bit             | 4096                                  |         no           | Default Root Cert Bit Size                                                  |
| intermediate_cert_bit     | 4096                                  |         no           | Default Intermediate Cert Bit Size                                          |
| server_cert_bit           | 2048                                  |         no           | Default Server Cert Bit Size                                                |
| serial_number             | 1000                                  |         no           | Cert Common Info - Serial Number                                            |
| countryName               | CA                                    |         no           | Cert Common Info - Country Name                                             |
| stateOrProvinceName       | ON                                    |         no           | Cert Common Info - Province Name                                            |
| localityName              | MILTON                                |         no           | Cert Common Info - Locality Name                                            |
| organizationName          | RED HAT                               |         no           | Cert Common Info - Org Name                                                 |
| organizationalUnitName    | SCE                                   |         no           | Cert Common Info - Org Unit Name                                            |
| emailAddress              | test@test.com                         |         no           | Cert Common Info - Email Address                                            |
| root_commonName           | Root CA                               |         no           | Root Cert Info - Root CN                                                    |
| intermediate_commonName   | Intermediate CA                       |         no           | Intermediate Cert Info - Intermediate CN                                    |
| cert_commonName           | lb.example.com                        |         no           | Server Cert Info - Server Cert CN                                           |
| use_intermediate_cert     | yes                                   |         no           | If no, it does not issue intermediate cert                                  |
| use_san                   | yes                                   |         no           | If yes, SAN info will be added with CN name                                 |
| san_dns                   |                                       |         no           | Add several SAN DNS List                                                    |
| san_ip                    |                                       |         no           | Add several SAN IP List                                                     |
| overwrite_server_cert     | yes                                   |         no           | Delete server cert directory that is based on CN name                       |
| clean_all                 | no                                    |         no           | Recreate all certs                                                          |



Dependencies
------------

None



Example Playbook
----------------
~~~
- name: Example Playbook
  hosts: localhost
  gather_facts: false

   roles:
      - { role: Jooho.generate-self-signed-cert }

~~~

Example Vars
------------
Wildcard Certificate:
~~~
cert_commonName: *.cloudapps.example.com
~~~

SAN DNS LIST:
~~~
san_dns:
 - { index: 1, dns: lb.example.com}
 - { index: 2, dns: master-cluster.example.com}
~~~

SAN IP LIST:
~~~
san_ip:
 - { index: 1, ip: 192.168.200.205}
~~~

NOTE
----
The private key & certificate permission should be 400/444 but this ansible role use 770/770 because these files will be copied to somewhere.
After you copy them, it is recommended to change permissions.

Useful Commands
---------------
~~~
openssl x509 -in {{cert_base_dir}/{{server_cert_commomName}}/{{server_cert_commonName}.cert.pem -text
openssl x509 -in /root/cert_base/lb.example.com/lb.example.com.cert.pem -text
~~~

Reference Doc
--------------
- https://jamielinux.com/docs/openssl-certificate-authority/

License
-------

BSD/MIT

Author Information
------------------

This role was created in 2017 by [Jooho Lee](http://github.com/jooho).


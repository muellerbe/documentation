=============
Webinterfaces
=============

The following services are accessible with a webbrowser. The used interface, nodes and port are
configurable in the configuration repository.

=============== ======== ====================== ================== ============ ===========================
**Service**     **Port** **Interface/Address**  **Node**           **Username** **Password**
--------------- -------- ---------------------- ------------------ ------------ ---------------------------
ARA             8120     ``console_interface``  manager            n/a          n/a
Ceph dashboard  7000     ``kolla_internal_vip`` network/controller n/a          n/a
Cockpit         8130     ``console_interface``  manager            dragon       ``operator_password``
Grafana         3000     ``kolla_external_vip`` network/controller grafana      ``grafana_admin_password``
Grafana         3000     ``kolla_internal_vip`` network/controller grafana      ``grafana_admin_password``
Horizon           80     ``kolla_external_vip`` network/controller admin        ``keystone_admin_password``
Horizon           80     ``kolla_internal_vip`` network/controller admin        ``keystone_admin_password``
Horizon w/ TLS   443     ``kolla_external_vip`` network/controller admin        ``keystone_admin_password``
Kibana          5601     ``kolla_external_vip`` network/controller kibana       ``kibana_password``
Kibana          5601     ``kolla_internal_vip`` network/controller kibana       ``kibana_password``
phpMyAdmin      8110     ``console_interface``  manager            root         ``database_password``
Prometheus      9090     ``kolla_internal_vip`` network/controller n/a          n/a
Rally           8090     ``console_interface``  manager            n/a          n/a
=============== ======== ====================== ================== ============ ===========================

``operator_password`` can be found in file ``environments/secrets.yml``. All other passwords can be found
in file ``environments/kolla/secrets.yml``.

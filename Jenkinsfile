node {
    stage 'Checkout Mastering OSA Repository'
       checkout scm
    stage 'Running Integration Tests'
       sh "sudo git clone -b 16.0.23 https://git.openstack.org/openstack/openstack-ansible /opt/openstack-ansible ; cd /opt/openstack-ansible ; scripts/bootstrap-ansible.sh"

}

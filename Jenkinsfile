node {
    stage 'Checkout Mastering OSA Repository'
       checkout scm
    stage 'Running Integration Tests'
       def folder = new File('/opt/openstack-ansible')
       if (!folder.exists()) {
       sh "sudo git clone -b 16.0.23 https://git.openstack.org/openstack/openstack-ansible /opt/openstack-ansible"
       }
       sh "cd /opt/openstack-ansible ; scripts/bootstrap-ansible.sh"

}

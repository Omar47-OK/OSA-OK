node {
    stage 'Checkout Mastering OSA Repository'
       checkout scm
    stage 'Running Integration Tests'

    stage 'Bootrasping Ansible OpenStack'
       def folder = new File('/opt/openstack-ansible')
       if (!folder.exists()) {
       sh "sudo git clone -b 16.0.23 https://git.openstack.org/openstack/openstack-ansible /opt/openstack-ansible"
       }
       sh "cd /opt/openstack-ansible ; sudo scripts/bootstrap-ansible.sh"
   
   stage 'Infastructure Syntax Playbooks Check'
       sh "sudo /usr/local/bin/openstack-ansible /opt/openstack-ansible/playbooks/setup-infrastructure.yml --syntax-check"

}

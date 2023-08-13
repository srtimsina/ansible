# Ansible
Create EC2 instance with ansible.
In the example we will have a ansible controller plane hosted in AWS. We will install `ansible` and `boto3` in that ec2 instance. Similarly we will create a `keypair` and `ec2 role` so that we won't need to configure `access keys` and `secret keys`.

#### Prerequisites and setup
Following prerequites must meet

Install `ansible`,`python3-pip` in ec2 instance.

```bash
  sudo apt update
  sudo apt install ansible python3-pip -y
  pip install boto3 boto
```

## Create configuration files
Clone the repository
```bash
git clone https://github.com/srtimsina/ansible.git
```
Change directory to project directory
```bash
cd ansible/ec2-automation/
ls
```
Update the `inventory` file contents if needed

Update the configurations in `create-ec2.yaml`

Run the playbook

## Debugging
if you get the following error, install `boto3`
```bash
ansible-playbook create-ec2.yaml -C

PLAY [Provision EC2 instance with Ansible] *******************************************************************

TASK [Create Security Group] *********************************************************************************
fatal: [localhost]: FAILED! => {"ansible_facts": {"discovered_interpreter_python": "/usr/bin/python3"}, "changed": false, "msg": "Failed to import the required Python library (botocore or boto3) on control-plane's Python /usr/bin/python3. Please read the module documentation and install it in the appropriate location. If the required library is installed, but Ansible is using the wrong Python interpreter, please consult the documentation on ansible_python_interpreter"}

PLAY RECAP ***************************************************************************************************
localhost                  : ok=0    changed=0    unreachable=0    failed=1    skipped=0    rescued=0    ignored=0
```

If you get following error, configure access keys and secret keys or create ec2 roles and attach the role to the ec2 control plane.
```bash
ansible-playbook create-ec2.yaml -C

PLAY [Provision EC2 instance with Ansible] *******************************************************************

TASK [Create Security Group] *********************************************************************************
An exception occurred during task execution. To see the full traceback, use -vvv. The error was: botocore.exceptions.NoCredentialsError: Unable to locate credentials
fatal: [localhost]: FAILED! => {"ansible_facts": {"discovered_interpreter_python": "/usr/bin/python3"}, "boto3_version": "1.28.25", "botocore_version": "1.31.25", "changed": false, "msg": "Error in describe_security_groups: Unable to locate credentials"}

PLAY RECAP ***************************************************************************************************
localhost                  : ok=0    changed=0    unreachable=0    failed=1    skipped=0    rescued=0    ignored=0
```

If you get below error, install `boto`
```bash
ansible-playbook create-ec2.yaml

PLAY [Provision EC2 instance with Ansible] *******************************************************************

TASK [Create Security Group] *********************************************************************************
ok: [localhost]

TASK [Launch EC2 instance] ***********************************************************************************
fatal: [localhost]: FAILED! => {"changed": false, "msg": "boto required for this module"}

PLAY RECAP ***************************************************************************************************
localhost                  : ok=1    changed=0    unreachable=0    failed=1    skipped=0    rescued=0    ignored=0
```

After the successful deployment, following will be seen.
```bash
ansible-playbook create-ec2.yaml

PLAY [Provision EC2 instance with Ansible] *******************************************************************

TASK [Create Security Group] *********************************************************************************
changed: [localhost]

TASK [Launch EC2 instance] ***********************************************************************************
changed: [localhost]

TASK [Add tag for EC2 instance] ******************************************************************************
changed: [localhost] => (item={'id': 'i-0598ec5e8317e2fc0', 'ami_launch_index': '0', 'private_ip': '172.31.90.110', 'private_dns_name': 'ip-172-31-90-110.ec2.internal', 'public_ip': '44.211.143.155', 'dns_name': 'ec2-44-211-143-155.compute-1.amazonaws.com', 'public_dns_name': 'ec2-44-211-143-155.compute-1.amazonaws.com', 'state_code': 16, 'architecture': 'x86_64', 'image_id': 'ami-053b0d53c279acc90', 'key_name': 'lab-ansible', 'placement': 'us-east-1b', 'region': 'us-east-1', 'kernel': None, 'ramdisk': None, 'launch_time': '2023-08-13T04:45:54.000Z', 'instance_type': 't2.small', 'root_device_type': 'ebs', 'root_device_name': '/dev/sda1', 'state': 'running', 'hypervisor': 'xen', 'tags': {}, 'groups': {'sg-048d959daa5c3561a': 'ansiblelabsg'}, 'virtualization_type': 'hvm', 'ebs_optimized': False, 'block_device_mapping': {'/dev/sda1': {'status': 'attached', 'volume_id': 'vol-0b1c456cdfd6cd619', 'delete_on_termination': True}}, 'tenancy': 'default'})

PLAY RECAP ***************************************************************************************************
localhost                  : ok=3    changed=3    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0

```

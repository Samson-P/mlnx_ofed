# mlnx_ofed
Ansible playbooks to prepare host with Mellanox CX-4 /dev/enp

Dependence to connect via password:

`sudo apt-get install sshpass`

To create deploy-user and add him our keys as authorized_keys:

`ansible-playbook -i users-setup users.yml`

Block ubuntu password:

`ansible-playbook -i inventory remove-ubuntu.yml`

Validate:

    (venv) deploy-user@spc:~/mlnx_ofed$ ansible -i inventory all -m ping
    192.168.0.10 | SUCCESS => {
        "ansible_facts": {
            "discovered_interpreter_python": "/usr/bin/python3"
        },
        "changed": false,
        "ping": "pong"
    }
    (venv) deploy-user@spc:~/mlnx_ofed$ ansible -i users-setup all -m ping
    192.168.0.10 | UNREACHABLE! => {
        "changed": false,
        "msg": "Invalid/incorrect password: Permission denied, please try again.",
        "unreachable": true
    }

To prepare nodes for sfp connections (installation firmware from [mlnx_ofed](https://linux.mellanox.com/public/repo/mlnx_ofed/5.8-3.0.7.0/ubuntu22.04/x86_64/)):

`ansible-playbook -i inventory sfp-mlnx-cx4-setup.yml`


Check it works!

    (venv) deploy-user@spc:~/mlnx_ofed$ ansible-playbook -i inventory sfp-mlnx-cx4-setup.yml

    PLAY [StartUp SFP MLNX CX-4 firmware and prep to tests] *****************

    TASK [add an apt Mellanox Technologies GPG-KEY's] ***********************
    changed: [192.168.0.10]

    TASK [add a mlnx_ofed repo signed-by key of last step] ******************
    changed: [192.168.0.10]

    TASK [apt update & install mlnx-ofed-all iperf3] ************************
    changed: [192.168.0.10]

    TASK [reboot] ***********************************************************
    changed: [192.168.0.10]

    PLAY RECAP **************************************************************
    192.168.0.10: ok=4    changed=4    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0

[Reference](https://docs.nvidia.com/networking/display/mlnxofedv461000/installing+mellanox+ofed#src-12013402_InstallingMellanoxOFED-ofedinstallationusingapt-get)

# Vagrant setup for corpusops

**NO LONGER MAINTAINED / NO RELEASE SECURITY FIXES**

## Vagrant setup for test VMs
- We provide a setup to deploy VMS and provision with [corpusops](https://github.com/corpusops/corpusops.bootstrap.git)
- The first thing you need is a ``setups.vagrant`` copy (one per ``local cluster``)

    ```sh
    somewhere=$(pwd)/myvm
    git clone  https://github.com/corpusops/setups.vagrant.git $somewhere
    cd $somewhere
    ```

- Then you have 2 options,
    1. Either you already have a working copy of corpusops.bootstrap, symlink it inside ``./local/corpusops.bootstrap``

        ```sh
        ln -s /path/to/working/corpusops.bootstrap local/corpusops.bootstrap
        ```
    2. Or you want another copy of corpusops.bootstrap, just run ``bin/install.sh``

        ```sh
        bin/install.sh
        ```

- maybe configure VM options [see this README](https://github.com/corpusops/corpusops.bootstrap/blob/master/hacking/vagrant/README.md)

    ```sh
    $EDITOR vagrant_config.yml
    ```

- After that, launch the vm

    ```sh
    ./vm_manage up
    ```

- You can of your clone/adapt this repo to configure in details your local cluster


# Run Manually ansible

- When you run vagrant up, it displays long ansible commands, just copy/adapt them, to something like:
- vagrant must had ran once for the inventory to be generated

```sh
    PYTHONUNBUFFERED=1 ANSIBLE_FORCE_COLOR=true \
    ANSIBLE_CONFIG='local/corpusops.bootstrap/hacking/vagrant/ansible.cfg' \
    ANSIBLE_HOST_KEY_CHECKING=false \
    ANSIBLE_SSH_ARGS='-o UserKnownHostsFile=/dev/null -o IdentitiesOnly=yes -o ControlMaster=auto -o ControlPersist=60s' \
        local/corpusops.bootstrap/bin/ansible-playbook \
            --connection=ssh --timeout=30 \
            --inventory-file=.vagrant/provisioners/ansible/inventory \
            --extra-vars="@playbooks/variables/vbox.yml" \
               -vvv -l <YOURVM_OR_,HOSTS> playbooks/YOURPLAYBOOK.yml
```

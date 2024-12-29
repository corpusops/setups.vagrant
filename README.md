# Vagrant setup for corpusops


DISCLAIMER - ABANDONED/UNMAINTAINED CODE / DO NOT USE
=======================================================
While this repository has been inactive for some time, this formal notice, issued on **December 10, 2024**, serves as the official declaration to clarify the situation. Consequently, this repository and all associated resources (including related projects, code, documentation, and distributed packages such as Docker images, PyPI packages, etc.) are now explicitly declared **unmaintained** and **abandoned**.

I would like to remind everyone that this project’s free license has always been based on the principle that the software is provided "AS-IS", without any warranty or expectation of liability or maintenance from the maintainer.
As such, it is used solely at the user's own risk, with no warranty or liability from the maintainer, including but not limited to any damages arising from its use.

Due to the enactment of the Cyber Resilience Act (EU Regulation 2024/2847), which significantly alters the regulatory framework, including penalties of up to €15M, combined with its demands for **unpaid** and **indefinite** liability, it has become untenable for me to continue maintaining all my Open Source Projects as a natural person.
The new regulations impose personal liability risks and create an unacceptable burden, regardless of my personal situation now or in the future, particularly when the work is done voluntarily and without compensation.

**No further technical support, updates (including security patches), or maintenance, of any kind, will be provided.**

These resources may remain online, but solely for public archiving, documentation, and educational purposes.

Users are strongly advised not to use these resources in any active or production-related projects, and to seek alternative solutions that comply with the new legal requirements (EU CRA).

**Using these resources outside of these contexts is strictly prohibited and is done at your own risk.**

Regarding the potential transfer of the project to another entity, discussions are ongoing, but no final decision has been made yet. As a last resort, if the project and its associated resources are not transferred, I may begin removing any published resources related to this project (e.g., from PyPI, Docker Hub, GitHub, etc.) starting **March 15, 2025**, especially if the CRA’s risks remain disproportionate.

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

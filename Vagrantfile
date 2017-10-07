# -*- mode: ruby -*-
$ABSFILE = File.absolute_path(__FILE__)
$ABSDIR = File.dirname($ABSFILE)
$COPS_DIR = ENV.fetch('COPS_DIR', File.join($ABSDIR, "local/corpusops.bootstrap"))

def load_glue()
    # test for vagrant glue to be in a project ./local subdir or if
    # we are already on corpusops.bootstrap
    [[$ABSDIR, $COPS_DIR], [$ABSDIR, $ABSDIR]].each do |cwd, cops_path|
        v = "#{cops_path}/hacking/vagrant/Vagrantfile_common.rb"
        if File.exists? v
          vagrant_common = v
          require vagrant_common
          return cops_dance({:cwd => cwd, :cops_path => cops_path})
        end
    end
    raise "Corpusops.bootstrap vagrantfile not found"
end
cfg = load_glue()

# add here post common modification like modifying played ansible playbooks
# please use file variables to let users a way to call manually ansible
ansible_vars = {
    :extra_vars => "playbooks/variables/vbox.yml"
}

# playbooks than plays everywhere
cfg = cops_inject_playbooks \
    :cfg => cfg,
    :playbooks => [
            # install docker
            {"playbooks/vbox_install_docker.yml" => ansible_vars}
    ]

# install rancher server only on first box
#cfg = cops_inject_playbooks \
#    :cfg => cfg,
#    :playbooks => [
#        # install rancher server
#        {"playbooks/vbox_server.yml" => ansible_vars},
#        # base configure server & register the vbox itself as an agent
#        {"playbooks/vbox_standalone.yml" => ansible_vars},
#        # cleanup
#        {"playbooks/vbox_rancher_cleanup.yml" => ansible_vars},
#    ],
#    :machine_num => cfg['MACHINE_NUM']

debug cfg.to_yaml
# vim: set ft=ruby ts=2 et sts=2 tw=0 ai:

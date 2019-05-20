nagios-config-deploy
====================

An Ansible role that deploys Nagios config and custom plugin files from a git repository

The role will checkout configuration files from a git repository into a working
directory and then symlink these files into the nagios config directory.

WARNING: This will delete your original nagios config files! Do not run on an existing server.

The role can also symlink custom plugin files.

To use this option you should look into using Git LFS for storing any larger plugin files. The role will install 
git-lfs on the server for you.


Nagios Config File Override System
----------------------------------

Config files specific to a particular server can be placed into a subfolder in the repository.
This can help when you have a generic set of services/hostgroups but need to configure hosts for multiple 
regions/datacenters/customers.

eg. Config files for the Dallas Datacentre in "nagios/dal09/" and Melbourne in "nagios/mel01/"

The path to the relevant subfolder can be set in a host variable (nagios_config_deploy_nag_override_dir).
The role will deploy any server specific config files that are present instead of the more 'generic' files of the 
same name in the main folder.


Nagios Plugins
---------------

Custom nagios plugin files can be added to a directory in the repo (default dir: plugins). Files in this directory 
will be symlinked to the nagios plugins directory.


Requirements
------------

Requires a working nagios installation and a git repository that contains your config files.

If your config repository is private (recommended) then look at setting up ssh-agent forwarding so that the git task 
can utilise your SSH keys without you having to leave your SSH keys on the nagios server:

https://developer.github.com/v3/guides/using-ssh-agent-forwarding/


Role Variables
--------------

See defaults/main.yml.


Dependencies
------------

No forced dependencies. Choose your preferred method of installing Nagios. You may wish to take a look at:

https://galaxy.ansible.com/oefenweb/nagios-server


Example Playbook
----------------

Obviously you will need to pass in your git repository details (not the example/default ones):

    - hosts: pabxservers
      vars: 
        nagios_config_deploy_repo_protocol: "ssh://" 
        nagios_config_deploy_repo_url: "github.com/myusername/myprivatenagiosconfigrepo"
        nagios_config_deploy_nag_dir: "nagios/"
        nagios_config_deploy_nag_override_dir: "nagios/nag01/"
      
      roles:
        - oefenweb.nagios-server
        - LukasGibb.nagios_config_deploy


License
-------

MIT


Author Information
------------------

This role was created in 2019 by:
[Lukas Gibb](https://github.com/LukasGibb) from [CloudJourneyman.com](http://www.cloudjourneyman.com/)

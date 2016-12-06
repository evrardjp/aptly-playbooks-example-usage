# Aptly usage

## Initial work to do

Aptly needs pgp keys. The requirements (generation of keys) are listed in aptly-snapshot-create.yml .

## How to use this on a regular basis

1. Git clone the aptly role in the proper folder, branch rax.

    ``
    cd /etc/ansible/roles/ && git clone https://github.com/evrardjp/ansible-role-aptly.git -b rax infOpen.aptly
    ``

1. Edit the ``aptly-install-and-mirror.yml`` to mirror what you need. It should be alright by default for Newton Trusty/Xenial. Target your inventory as needed.

1. Run the playbook to create the mirror/repos, and if ``do_update: True``, this will start the update process.

## How to release

1. Use aptly-snapshot-create.yml to create a snapshot for your release version. If you have linked your playbook to your osa inventory, it's gonna auto consume the variables openstack_release.
(You may want to change the target node). Else just give ``-e openstack_release=whatever`` in the CLI.

1. When your snapshots are ready, at the end of the playbook, there is gonna be a list of all the snapshot taken.

1. Release into your repo by running the next playbook: aptly-snapshot-merge-and-publish.yml . You may want to filter snapshots you don't want, by using the exclude list like this:
``ansible-playbook -e openstack_release=whatever (or openstack-ansible) aptly-snapshot-merge-and-publish.yml -e @skip_list.yml``

The skip_list file basically contain the variable: ``aptly_dont_merge_snapshot_list``, and the content is the list of snapshots to not merge.

1. This will publish into ``~aptly/public/`` . This can then be rsynced.

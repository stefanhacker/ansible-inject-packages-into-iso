Role ansible-inject-packages-into-iso

With this role, you can add packages to an live iso.
I need an live debian iso with german keyboard loyout and ssh login with root access.
So i build an role so we can build easy our custom iso ever an ever again, without pain or the question how


Example Playbook


---
- hosts: myhosts
  become: yes
  gather_facts: yes

  roles:
    - role: inject_packages_into_iso
      enable_download: True
      download_public_iso_url: https://cdimage.debian.org/debian-cd/current-live/amd64/iso-hybrid/debian-live-11.3.0-amd64-standard.iso
      live_folder_name: live/
      path_to_src_iso_file_with_filename: /tmp/iso/debian11.iso
      path_to_dest_iso_filename: /srv/tftp/images/debianlive_ssh/x86_64/debianlive_ssh.iso
      install_ssh_server: True
      #clean working
      clean_working_dir: False
      packages_to_install:
      - "htop"
      - "mc"
      set_root_pass: "installer10"
      cpu_count_for_make_squashfs_again: 10
      #keyboard
      XKBMODEL: pc105
      #language
      XKBLAYOUT: de
      #key variants
      XKBVARIANT: nodeadkeys
      #other options
      XKBOPTIONS: ""
      #backspace a few use guess
      BACKSPACE: ""
      hostname: installer-strange

Description of Variables to be set
---
#path were the source iso copied manual or will downloaded to when url is given in download public url. The sorce path is basedir from iso file. the tmp path
path_to_src_iso_file_with_filename: /tmp/iso/debian11.iso

#destination iso path with filename eg. to an tftpd folder. the folder must not exist. it will creates if not exist
path_to_dest_iso_filename: /srv/tftp/images/debianlive_ssh/x86_64/debianlive_ssh.iso

#this is the folder were squash initrd etc are. on debian this folder calls live and linux mint its called casper
live_folder_name: "live/"

#cleans only the workind dir where the extracted an recreates squashfs file is in at start
clean_working_dir: False

#clean tmp folder after fisnish all
clean_tmp_folder: False

#enable_download, if enable iso will be downloaded from source given in download_public_iso_url
enable_download: False

#how many cores should use for rebuild the squash.fs (with one core it clould take an hour or longer, but its an cpu expensive progress. So be carefull how many cores you use )
cpu_count_for_make_squashfs_again: 1

#url for iso file
download_public_iso_url: https://cdimage.debian.org/debian-cd/current-live/amd64/iso-hybrid/debian-live-11.3.0-amd64-standard.iso

#will install open_ssh server and enables root login per password, if it set true, default is false
install_ssh_server: false

#package list to inject
packages_to_install: []
#do not change
working_dir: "{{ path_to_src_iso_file_with_filename | dirname }}/work/"
#end do not change
#assign root pass. If root pass emty it will be  untouched
set_root_pass: ""
#if hostname empty hosts file and hostname file is untocuhed
hostname: ""
#keyboardlayout
#keyboadmodel
XKBMODEL: pc105
#language
XKBLAYOUT: de
#key variants
XKBVARIANT: nodeadkeys
#other options
XKBOPTIONS: ""
#backspace a few use guess
BACKSPACE: ""



#cloud-config
autoinstall:
version: 1
autoinstall:
  identity:
    hostname: ubuntu-external
    username: sosarx
    password: "$6$your_hashed_password_here"
  locale: en_US.UTF-8
  keyboard:
    layout: us
  ssh:
    install-server: true
    allow-pw: true
  storage:
    config:
      - id: disk0
        type: disk
        match:
          size: 320GB
        ptable: gpt
        wipe: superblock-recursive
        preserve: false
        grub_device: true
        partitions:
          - size: 512M
            type: efi
            mount: /boot/efi
          - size: 314G
            type: ext4
            mount: /
          - size: 4G
            type: swap
          - size: -1
            type: ext4
            mount: /home
  packages:
    - vim
    - curl
    - net-tools
    - htop
    - openssh-server
  late-commands:
    - echo "Ubuntu installed on external drive" > /home/joe/install.log
    user-data:
  autologin: true
  late-commands:
  - sed -i 's/GRUB_TIMEOUT=10/GRUB_TIMEOUT=2/' /etc/default/grub
  - update-grub

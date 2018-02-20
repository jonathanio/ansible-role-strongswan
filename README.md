# Ansible strongSwan Role

An Ansible role for the configuration of strongSwan will support for Arch Linux,
RHEL/CentOS, and Debian/Ubuntu.

## Requirements

This module as of right now only works on Arch, provided you install the package
via the AUR, and you have UFW managing the firewall and have patched it to
support GRE tunnels. This will need to be resolved before the role can be
released publicly and/or added to Ansible Galaxy.

strongSwan version >= 5.0.0 may also be required. This hasn't been confirmed
yet.

## Role Variables

    strongswan_packages:
      - strongswan

Current list of packages to be installed. At the moment this is Arch Linux
specific.

    strongswan_conn_default:
      auto: add
      type: tunnel
      authby: psk
      keyexchange: ike
      ikelifetime: 3h
      lifetime: 60m
      margintime: 15m
      keyingtries: 3
      dpdaction: restart
      dpddelay: 30

Current defaults placed into the `%default` connection. Most of these follow the typical defaults for strongSwan (as if version ~5.0.0).

    strongswan_conn: []
    # - name: connection_name
    #   conn:
    #     # connection options go here, e.g.
    #     ike: aes256gcm16-modp2048!
    #     esp: aes256gcm16-modp2048!
    #   left:
    #     address: local_address
    #     # further left-hand options here
    #   right:
    #     address: remote_address
    #     # further right-hand options here
    #   secret: abcde...z

Connection information to be installed into strongSwan.

## Example Playbook

    - hosts: ipsec_server
      roles:
         - { role: jonathanio.strongswan, tags: ['ipsec'] }

    ---
    strongswan_hosts:
      - name: example
        conn:
          auto: route
          type: tunnel
          authby: psk
          keyexchange: ikev2
          lifetime: 3h
          ike: aes256gcm16-modp2048!
          esp: aes256gcm16-modp2048!
          ikelifetime: 24h
        left:
          address: 0.0.0.0/0
          subnet: 192.168.100.0/24
          protoport: 47
          id: my
          updown: /usr/lib/ipsec/_updown_nat
        right:
          address: 87.65.43.21
          subnet: 192.168.101.0/24
          protoport: 47
          id: your
          updown: /usr/lib/ipsec/_updown_nat
        secret: something_needs_to_go_here

## License

GPLv2

## Author Information

Jonathan Wright.

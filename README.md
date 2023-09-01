# sleif.openldap_container

This role runs an OpenLDAP instance on Podman.

## Requirements

- a local directory that provides SSL certificates; it will be watched from cron; have a look on sleif.caddy_container

## Role Variables

- container_storage_dir_base: '/srv'

## Dependencies

None

## Example Playbook

```yml
- name: VM openldap.example.com
  hosts: "openldap.example.com"
  user: root
  vars:
    container_storage_dir_base: '/srv'
    site_posix_groups_example_de: {
      johndoe: {gid: 2001},
      janedoe: {gid: 2002}}

    # example_DE
    site_posix_users_example_de:
      johndoe:
        displayName: John Doe
        name: johndoe
        uid: 2001
        gid: 2001
        password: "{{ site_default_password }}"
        comment: John Doe
        givenname: John
        sn: Doe
        mail: John.Doe@example.de
        groups: []
      janedoe:
        displayName: Jane Doe
        name: janedoe
        uid: 2002
        gid: 2002
        password: "{{ site_default_password }}"
        comment: Jane Doe
        givenname: Jane
        sn: Doe
        mail: Jane.Doe@example.de
        groups: []

    site_gouns_example_de:
      - name: goun01
        members:
          - johndoe
          - janedoe
      - name: goun02
        members:
          - johndoe

  roles:
    - {role: sleif.openldap_container, tags: "openldap_container, ldap_example_de",
       container_name: openldap,
       site_domain_ou: "example_com",
       site_posix_users: "{{ site_posix_users_example_de }}",
       site_posix_groups: "{{ site_posix_groups_example_de }}",
       site_gouns: "{{ site_gouns_example_de }}"}
```

## License

MIT

## Author Information

Created in 2023 by Sebastian Berthold

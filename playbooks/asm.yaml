---

- name: An example ASM policy playbook
  hosts: bigip
  connection: local

  vars:
    validate_certs: no
    username: admin
    password: admin

  tasks:
    - name: Create ASM policy, compact XML file
      bigip_asm_policy:
        name: foo-policy
        file: ../files/v2_policy_compact.xml
        active: yes
        user: "{{ username }}"
        password: "{{ password }}"
        server: 10.1.1.4
        validate_certs: "{{ validate_certs }}"

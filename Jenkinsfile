---
 - hosts: all
   become: yes
   tasks:
    - name: Install supervisord
      apt:
        name: supervisor
        state: present
        update_cache: yes

    - name: Reloaded supervisord
      service:
          name: "supervisor"
          state: reloaded
          enabled: yes

    - name: Restart supervisord
      service:
          name: "supervisor"
          state: restarted
          enabled: yes

    - name: Install supervisord
      become: true
      apt:
        name: supervisor
        state: present
        update_cache: yes
      tags:
         supervisor
    
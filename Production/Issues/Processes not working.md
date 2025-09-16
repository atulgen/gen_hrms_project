```bash
frappe@srv938744:~/genhrms-bench-16$ sudo supervisorctl status 
eits-bench-redis:eits-bench-redis-cache                           RUNNING   pid 114360, uptime 4:16:52
eits-bench-redis:eits-bench-redis-queue                           RUNNING   pid 114361, uptime 4:16:52
eits-bench-web:eits-bench-frappe-web                              RUNNING   pid 114362, uptime 4:16:52
eits-bench-web:eits-bench-node-socketio                           RUNNING   pid 114363, uptime 4:16:52
eits-bench-workers:eits-bench-frappe-long-worker-0                RUNNING   pid 114374, uptime 4:16:52
eits-bench-workers:eits-bench-frappe-schedule                     RUNNING   pid 114364, uptime 4:16:52
eits-bench-workers:eits-bench-frappe-short-worker-0               RUNNING   pid 114368, uptime 4:16:52
genhrms-bench-16-redis:genhrms-bench-16-redis-cache               FATAL     Exited too quickly (process log may have details)
genhrms-bench-16-redis:genhrms-bench-16-redis-queue               FATAL     Exited too quickly (process log may have details)
genhrms-bench-16-web:genhrms-bench-16-frappe-web                  RUNNING   pid 122775, uptime 0:00:01
genhrms-bench-16-web:genhrms-bench-16-node-socketio               FATAL     Exited too quickly (process log may have details)
genhrms-bench-16-workers:genhrms-bench-16-frappe-long-worker-0    FATAL     can't find command 'None'
genhrms-bench-16-workers:genhrms-bench-16-frappe-schedule         FATAL     can't find command 'None'
genhrms-bench-16-workers:genhrms-bench-16-frappe-short-worker-0   FATAL     can't find command 'None'
frappe@srv938744:~/genhrms-bench-16$ 
```



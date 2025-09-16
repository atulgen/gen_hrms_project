 Full backup including files:
bash

```bash
bench --site hrms.gennextit.com backup --with-files --backup-path ./Backup
```



```bash
bench --site hrms.gennextit.com restore   ~/genhrms-bench-backup/backup-15-monday/20250915_165007-hrms_gennextit_com-database.sql.gz   --with-private-files ~/genhrms-bench-backup/backup-15-monday/20250915_165007-hrms_gennextit_com-private-files.tar   --with-public-files ~/genhrms-bench-backup/backup-15-monday/20250915_165007-hrms_gennextit_com-files.tar
```

---


Maintenance Off:


Updating

The system is being updated. Please refresh again after a few moments.

```bash
bench --site hrms.gennextit.com set-maintenance-mode off
```



```bash
bench --site hrms.gennextit.com restore   ~/genhrms-bench-backup/backup-15-monday/20250915_165007-hrms_gennextit_com-database.sql.gz   --with-private-files ~/genhrms-bench-backup/backup-15-monday/20250915_165007-hrms_gennextit_com-private-files.tar   --with-public-files ~/genhrms-bench-backup/backup-15-monday/20250915_165007-hrms_gennextit_com-files.tar
```


```bash
# Stop web services
sudo supervisorctl stop genhrms-bench-16-web:genhrms-bench-16-frappe-web
sudo supervisorctl stop genhrms-bench-16-web:genhrms-bench-16-node-socketio

# Stop redis services
sudo supervisorctl stop genhrms-bench-16-redis:genhrms-bench-16-redis-cache
sudo supervisorctl stop genhrms-bench-16-redis:genhrms-bench-16-redis-queue

# Stop worker services (even though they're already failed)
sudo supervisorctl stop genhrms-bench-16-workers:genhrms-bench-16-frappe-long-worker-0
sudo supervisorctl stop genhrms-bench-16-workers:genhrms-bench-16-frappe-schedule
sudo supervisorctl stop genhrms-bench-16-workers:genhrms-bench-16-frappe-short-worker-0
```

---

## Issues

```bash

Server Error

500: There was an error building this page

Traceback (most recent call last):
  File "apps/frappe/frappe/www/app.py", line 30, in get_context
    boot = frappe.sessions.get()
  File "apps/frappe/frappe/sessions.py", line 144, in get
    bootinfo = get_bootinfo()
  File "apps/frappe/frappe/boot.py", line 43, in get_bootinfo
    get_user(bootinfo)
    ~~~~~~~~^^^^^^^^^^
  File "apps/frappe/frappe/boot.py", line 345, in get_user
    bootinfo.user = frappe.get_user().load_user()
                    ~~~~~~~~~~~~~~~~~~~~~~~~~~~^^
  File "apps/frappe/frappe/utils/user.py", line 211, in load_user
    d = frappe.db.get_value(
    	"User",
    ...<18 lines>...
    	as_dict=True,
    )
  File "apps/frappe/frappe/database/database.py", line 554, in get_value
    result = self.get_values(
    	doctype,
    ...<13 lines>...
    	wait=wait,
    )
  File "apps/frappe/frappe/database/database.py", line 667, in get_values
    out = query.run(as_dict=as_dict, debug=debug, update=update, run=run, pluck=pluck)
  File "apps/frappe/frappe/query_builder/utils.py", line 85, in execute_query
    result = frappe.local.db.sql(query, params, *args, **kwargs)  # nosemgrep
  File "apps/frappe/frappe/database/database.py", line 271, in sql
    self.execute_query(query, values)
    ~~~~~~~~~~~~~~~~~~^^^^^^^^^^^^^^^
  File "apps/frappe/frappe/database/database.py", line 365, in execute_query
    return self._cursor.execute(query, values)
           ~~~~~~~~~~~~~~~~~~~~^^^^^^^^^^^^^^^
  File "env/lib/python3.13/site-packages/MySQLdb/cursors.py", line 179, in execute
    res = self._query(mogrified_query)
  File "env/lib/python3.13/site-packages/MySQLdb/cursors.py", line 330, in _query
    db.query(q)
    ~~~~~~~~^^^
  File "env/lib/python3.13/site-packages/MySQLdb/connections.py", line 280, in query
    _mysql.connection.query(self, query)
    ~~~~~~~~~~~~~~~~~~~~~~~^^^^^^^^^^^^^
MySQLdb.OperationalError: (1054, "Unknown column 'code_editor_type' in 'SELECT'")

The above exception was the direct cause of the following exception:

Traceback (most recent call last):
  File "apps/frappe/frappe/website/serve.py", line 20, in get_response
    return renderer_instance.render()
           ~~~~~~~~~~~~~~~~~~~~~~~~^^
  File "apps/frappe/frappe/website/page_renderers/template_page.py", line 84, in render
    html = self.get_html()
  File "apps/frappe/frappe/website/utils.py", line 536, in cache_html_decorator
    html = func(*args, **kwargs)
  File "apps/frappe/frappe/website/page_renderers/template_page.py", line 95, in get_html
    self.update_context()
    ~~~~~~~~~~~~~~~~~~~^^
  File "apps/frappe/frappe/website/page_renderers/template_page.py", line 163, in update_context
    data = self.run_pymodule_method("get_context")
  File "apps/frappe/frappe/website/page_renderers/template_page.py", line 223, in run_pymodule_method
    return method(self.context)
  File "apps/frappe/frappe/www/app.py", line 32, in get_context
    raise frappe.SessionBootFailed from e
frappe.exceptions.SessionBootFailed

```

Solution:
```bash
bench --site hrms.gennextit.com migrate
```


Install gen_hrms app

---


1. SSH in VPS

ssh frappe@82.29.164.162

2. cd genhrms-bench-16/

3. Go into apps

cd apps


4. Go into specific app to update


cd recruitment_app/

5. Check branch and Pull the changes

git branch 
git pull

6. Migrate the changes: 

bench --site siteName.gennextit.com migrate


---

Bench status: 


- sudo supervisorctl status



---

If server down:

- Check status from supervisorctl
- Go to bench 
    - bench start
    - this will run the server in developmen mode 



- Run production using supervisorctl:
    - sudo supervisorctl start all 


- Still not working: 
    - sudo reboot 
    - wait for 1 min
    - ssh into vps
    - check status in supervisorctl

    
    

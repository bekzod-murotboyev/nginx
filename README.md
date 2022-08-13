# *NGINX*

---

### *1. Create your own package for nginx*
- #### The index package for nginx is `/var/www/html`
- #### We need to create aur own package into `/var/www`
```
sudo mkdir /var/www/project
```
---

### *2. Copy all files from build to `project` package*
```
sudo cp -r <path>/build/* /var/www/project/
```
--

### *3. Change nginx configuration*
```
sudo vim /etc/nginx/sites-available/default
```
- #### *Change them like this:*
<img width="666" alt="old" src="https://user-images.githubusercontent.com/102135015/184506291-6cca37a2-c7af-40a0-925b-9c34b9b88ef6.png">


<img width="582" alt="new" src="https://user-images.githubusercontent.com/102135015/184506298-f9f689c8-07ec-4f8c-97f2-e7c2c542b425.png">


- After changing just restart nginx and website ready to work!
```
systemctl restart nginx
```

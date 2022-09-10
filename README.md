# Config-NGINX-Ubuntu
How To Install and Configuration NGINX in ubuntu


# NGINX
![image info](https://www.nusa.id/knowledge-base/content/images/2021/05/NGINX-logo-rgb-large.png)

### Step Following this bellow
1. Update Software Repositories
It is important to refresh the repository lists before installing new software. This helps make sure that the latest updates and patches are installed.
Open a terminal window and enter the following:
```cmd
sudo apt-get update
```


2: Install Nginx From Ubuntu Repositories
Nginx is included in the Ubuntu 20.04 default repositories. Install it by entering the following command:
```cmd
sudo apt-get install nginx
```


3: Verify the Installation
Verify that Nginx installed correctly by checking the software version. Enter the following:
```cmd
nginx -v
```


4: Controlling the Nginx Service
The behavior of Nginx can be adjusted. Use this to start or stop Nginx, or to enable or disable Nginx at boot.

Start by checking the status of the Nginx service:
```cmd
sudo systemctl status nginx
```

If the status displays active (running), Nginx has already been started. Press CTRL+z to exit the status display.

If Nginx is not running, use the following command to launch the Nginx service:
```cmd
sudo systemctl start nginx
```
To set Nginx to load when the system starts, enter the following:
```cmd
sudo systemctl enable nginx
```
To stop the Nginx service, enter the following:
```cmd
sudo systemctl stop nginx
```
To prevent Nginx from loading when the system boots:
```cmd
sudo systemctl disable nginx
```
To reload the Nginx service (used to apply configuration changes):
```cmd
sudo systemctl reload nginx
```
For a hard restart of Nginx:
```cmd
sudo systemctl restart nginx
```


5: Allow Nginx Traffic
Nginx needs access through the system’s firewall. To do this, Nginx installs a set of profiles for the Ubuntu default ufw (UnComplicated Firewall).

Start by displaying the available Nginx profiles:
```cmd
sudo ufw app list
```
Note: Other applications may be listed. They can be ignored.
To grant Nginx access through the default Ubuntu firewall, enter the following:
```cmd
sudo ufw allow 'nginx http'
```
The system should display Rules updated.
Updating firewall rules to include Nginx
Refresh the firewall settings by entering:
```cmd
sudo ufw reload
```
For encrypted (https) traffic, enter:
```cmd
sudo ufw allow 'nginx https'
```
To allow both, enter:
```cmd
sudo ufw allow 'nginx full'
```

Note: It is recommended that you only allow the bare minimum required traffic through the firewall. For this process, only basic HTTP traffic is needed. Other configurations may require HTTPS (encrypted) or other traffic. If the system uses a different firewall, it should be configured to allow traffic on Port 80 (HTTP), Port 443 (HTTPS), or whatever ports are required by the network.


6: Test Nginx
Make sure that the Nginx service is running, as in Step 4. Open a web browser, and navigate to the following web address:
```cmd
[sudo ufw allow 'nginx full'](http://127.0.0.1)
```

Note: If the system has a specific hostname or IP address, that may be used instead

If the system does not have a graphical interface, the Nginx Welcome page can be loaded in the terminal using curl:
```cmd
sudo apt-get install curl
curl –i 127.0.0.1
```
The system should display the HTML code for the Nginx Welcome page.


7: Configure a Server Block (Optional)
In Nginx, a server block is a configuration that works as its own server. By default, Nginx has one server block preconfigured.

It is located at /var/www/html. However, it can be configured with multiple server blocks for different sites.

Note: This tutorial uses test_domain.com for the domain name. This may be replaced with your own domain name.


1. Create a Directory for the Test Domain
In a terminal window, create a new directory by entering the following:
```cmd
sudo mkdir -p /var/www/test_domain.com/html
```


2. Configure Ownership and Permissions
Use chmod to configure ownership and permission rules:
```cmd
sudo chown –R $USER:$USER /var/www/test_domain.com
sudo chmod –R 755 /var/www/test_domain.comCopied!
```


3. Create an index.html File for the Server Block
Open index.html for editing in a text editor of your choice (we will use the Nano text editor):
```cmd
sudo nano /var/www/test_domain.com/html/index.html
```
In the text editor, enter the following HTML code:
```cmd
<html>
<head>
<title>Welcome to test_domain.com!</title>
</head>
<body>
<h1>This message confirms that your Nginx server block is working. Great work!</h1>
</body>
</html>
```
Press CTRL+o to write the changes, then CTRL+x to exit.



4. Create Nginx Server Block Configuration
Open the configuration file for editing:
```cmd
sudo nano /etc/nginx/sites-available/test_domain.com
```
Enter the following code:
```cmd
server    {
listen 80;
 
root /var/www/test_domain.com/html;
index index.html index.htm index.nginx.debian.html;
 
server_name test_domain.com www.test_domain.com;
location /          {
try_files $uri $uri/ =404;
      }
}
```


5. Create Symbolic Link for Nginx to Read on Startup
Create a symbolic link between the server block and the startup directory by entering the following:
```cmd
sudo ln –s /etc/nginx/sites-available/test_domain.com /etc/nginx/sites-enabled
```


6. Restart the Nginx Service
Restart Nginx by running the following command:
```cmd
sudo systemctl restart nginx
```


7. Test the Configuration
```cmd
sudo nginx –t
```
The system should report that the configuration file syntax is OK, and that the configuration file test is successful.


8. Modify the Hosts File (Optional)
If you’re using a test domain name that isn’t registered or public, the /etc/hosts file may need to be modified to display the test_domain.com page.

Display the system’s IP address with the following command:
```cmd
hostname –i
```
Make a note of the IP address displayed.

Check your system's IP address
Next, open /etc/hosts for editing:
```cmd
sudo nano /etc/hosts
```
In an empty space just below the localhost information, add the following line:
```cmd
127.0.1.1 test_domain.com www.test_domain.com
```
Replace 127.0.0.1 with the IP address displayed above. Press CTRL+o to save the changes, then CTRL+x to exit.


9. Check test_domain.com in a Web Browser
Open a browser window and navigate to test_domain.com (or the domain name you configured in Nginx).

You should see the message you entered in Part 3.


Important Nginx File Locations
By default, Nginx stores different configuration and log files in the following locations:

/var/www/html – Website content as seen by visitors.
/etc/nginx – Location of the main Nginx application files.
/etc/nginx/nginx.conf – The main Nginx configuration file.
/etc/nginx/sites-available – List of all websites configured through Nginx.
/etc/nginx/sites-enabled – List of websites actively being served by Nginx.
/var/log/nginx/access.log – Access logs tracking every request to your server.
/var/log/ngins/error.log – A log of any errors generated in Nginx.

The code file you provided seems to outline the necessary steps for deploying a React application on a VPS using Nginx. It includes creating a non-root user, installing Node.js, Git, and Nginx, configuring SSH keys for GitHub, installing PM2 for process management, editing the Nginx default configuration, and setting permissions for the application files.

Here's a summary of the steps outlined in your code file:

Create a non-root user:

adduser jibran
usermod -aG sudo jibran
Install Node.js:

Update packages: sudo apt update
Install Node.js: curl -fsSL https://deb.nodesource.com/setup_lts.x | sudo -E bash - && sudo apt install -y nodejs
Verify Node.js and npm installation: node --version and npm --version
Install Git:

Update packages: sudo apt update
Install Git: sudo apt install git
Verify Git installation: git --version
Install Nginx:

Update packages: sudo apt update
Install Nginx: sudo apt install nginx
Start and enable Nginx service: sudo systemctl start nginx and sudo systemctl enable nginx
Configure SSH key for GitHub:

Generate SSH key: ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
Display the public key: cat ~/.ssh/id_rsa.pub
Set up email and name for Git: git config --global user.email "your_email@example.com" and git config --global user.name "Your Name"
Install PM2:

Update packages: sudo apt update
Install PM2 globally: sudo npm install -g pm2
Verify PM2 installation: pm2 --version
Edit Nginx default configuration:

Navigate to the Nginx sites-available directory: cd /etc/nginx/sites-available
Edit the default configuration: sudo nano default
Replace the configuration with the provided code that points to your React application build directory
Create a symbolic link to enable the site: sudo ln -s /etc/nginx/sites-available/your_config_file /etc/nginx/sites-enabled/
Test Nginx configuration: sudo nginx -t
Restart Nginx: sudo systemctl restart nginx
Set permissions:

Allow necessary ports through UFW: sudo ufw allow 8080 and sudo ufw allow 'Nginx HTTP'
Set permissions for the application files and directories:
sudo chmod +r /home/jibran/zx/build/index.html
sudo chmod +rx /home/jibran/zx
sudo chmod +rx /home/jibran/zx/build
sudo chown -R jibran:jibran /home/jibran/zx
sudo chmod -R 755 /home/jibran/zx
Restart Nginx: sudo systemctl restart nginx
Set ownership and permissions for the build directory:
sudo chown -R www-data:www-data /home/jibran/zx/build
sudo chmod -R 755 /home/jibran/zx/build
Set permissions for the home directory:
sudo chmod 755 /home/jibran
sudo chmod 755 /home
The code file seems to provide a comprehensive guide for deploying a React application on a VPS with Nginx. However, please note that it's important to adapt the instructions to match your specific setup and directory structure.


#adduser #howtodeployreactappication #react #mern

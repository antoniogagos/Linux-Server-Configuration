# Linux-Server-Configuration


## 1.- Download RSA Key, restrict key access, and ssh into instance:
 * <code>ssh -i "antonio.pem" grader@ec2-52-59-194-158.eu-central-1.compute.amazonaws.com</code>
 
 * <code>chmod 600 antonio.pem</code>
 
 * Live at: ec2-52-59-194-158.eu-central-1.compute.amazonaws.com
 
 * Accessible SSH port: 2200


## Installing & updating software
<h3> Updating and Upgrading Available Package List</h3>

  * <code>sudo apt-get update</code>
  
  * <code>sudo apt-get upgrade</code>
   
<h3>Installing finger</h3>

  * <em>This application will look up various pieces of information about an user and display it in an easy to read format
    </em>
  
  * <code>sudo apt-get install finger</code>
 
## Creating a new user
  * <code>sudo adduser grader</code>
  
  * <em>Connecting as the New User</em>
    <code>ssh -i "antonio.pem" grader@ec2-52-59-194-158.eu-central-1.compute.amazonaws.com</code>
  
  * <em>Giving sudo access to new user</em>
  <code>sudo cp /etc/sudoers.d/90-init-cloud-users /etc/sudoers.d/grader</code>
  
  * <code>sudo nano /etc/sudoers.d/grader</code>
  
  * <h4>Modify the vagrant text with grader</h4>
  
## Give SSH access to the new User
  
  * Log into your server as the new user you've just created
   
  * <code>mkdir .ssh</code>
    
  * <code>touch .ssh/authorized_keys</code>
        
  * <code>nano .ssh/authorized_keys</code>
    
  * Paste the private key content into <code>.ssh/authorized_keys</code> in your server with the new user.
    
  * <code>chmod 700 .ssh</code>
    
  * <code>chmod 644 .ssh/authorized_keys</code>
  
  * <code>vim /etc/ssh/sshd_config (Change ssh to 2200)</code>
  
  * <code>service ssh restart</code>
    
  * Log in using the Key pair: 
  
       <code>ssh -i "antonio.pem" grader@ec2-52-59-194-158.eu-central-1.compute.amazonaws.com</code>
   
        
## Forcing Key Based Authentication
   * <code>sudo nano /etc/ssh/sshd_config</code> and change <b>PasswordAuthentication</b> to <b>no</b>
     
   * Then restart de service by <code>sudo service ssh restart</code>
   
## Configuring Ports in UFW
   * <code>sudo ufw default deny incoming</code>
   
   * <code>sudo ufw default allow outgoing</code>
   
   * <code>sudo ufw allow ssh</code>
   
   * <code>sudo ufw allow 2222/tcp</code>
   
   * <code>sudo ufw allow www</code>
   
   * <code>sudo ufw enable</code>
   
## Web Application server
   * Install Apache HTTP Server <code>sudo apt-get install apache2</code> 
   
   * Install mod_wsgi <code>sudo apt-get install libapache2-mod-wsgi</code>
   
   * <code>sudo apt-get install postgresql postgresql-contrib</code>
   
   * <code>sudo a2enmod wsgi</code>
   
   * Then, change directory to <code>cd /var/www</code>
   
   * <code>sudo git clone https://github.com/balusus/Item-Catalog.git</code>
   
   * <code> sudo apt-get install python-pip </code>
   
     <code> sudo pip install virtualenv</code>
     
     <code>sudo virtualenv catalog</code>
     
     <code>source catalog/bin/activate</code>
     
     <code>sudo pip install Flask</code>
     
     <code>sudo pip install -r requirements.txt</code>
     
   * Then, To deactivate the environment, give the following command: <code>deactivate</code>
   
   * Change directory to <code>cd /etc/apache2/sites-available</code>
   
   * Create the file catalog.conf: <code>sudo touch catalog.conf</code> with this content:
      ```
      <VirtualHost *:80>
           ServerName ec2-52-59-194-158.eu-central-1.compute.amazonaws.com
           ServerAdmin antoniogagos@gmail.com
           WSGIScriptAlias / /var/www/Item-Catalog/catalog.wsgi
           <Directory /var/www/Item-Catalog/>
             Order allow,deny
             Allow from all
           </Directory>
           Alias /static /var/www/Item-Catalog/static
           <Directory /var/www/Item-Catalog/static/>
             Order allow,deny
             Allow from all
           </Directory>
           ErrorLog ${APACHE_LOG_DIR}/error.log
           LogLevel warn
           CustomLog ${APACHE_LOG_DIR}/access.log combined
      </VirtualHost> 
      ```
   * Enable catalog.conf with this command: <code>sudo a2ensite catalog</code>
   
   * Now restart apache2: <code>sudo service apache2 restart</code>
   
   * Configure postgres:
   
     - <code>sudo su - postgres</code>
     
     - <code>psql</code>
     
     - <code>CREATE USER grader WITH PASSWORD 'udacity'</code>
     
     - <code>ALTER USER grader CREATEDB</code>
 
 ## References
 
   * http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/managing-users.html
   
   * http://flask.pocoo.org/docs/0.12/deploying/mod_wsgi/
   
   * https://www.digitalocean.com/community/tutorials/how-to-deploy-a-flask-application-on-an-ubuntu-vps

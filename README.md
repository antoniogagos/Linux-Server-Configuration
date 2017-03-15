# Linux-Server-Configuration


## Installing & updating software
<h3> Updating and Upgrading Available Package Lists</h3>

  * <code>sudo apt-get update</code>
  
  * <code>sudo apt-get upgrade</code>
   
<h3>Installing finger</h3>
  * <em>This application will look up various pieces of information about an user and display it in an easy to read format
    </em>
  
  * <code>sudo apt-get install finger</code>
 
## Creating a new user
  * <code>sudo adduser {{name}}</code>
  
  * <em>Connecting as the New User</em>
    <code>ssh {{name}}@127.0.0.1 -p 22</code>
  
  * <em>Giving sudo access to new user</em>
  <code>sudo cp /etc/sudoers.d/vagrant /etc/sudoers.d/{{name}}</code>
  
  * <code>sudo nano /etc/sudoers.d/{{name}}</code>
  
  * <h4>Modify the vagrant text with the user name</h4>
  
## Generating Key pairs

  * In your local machine not in your server:
    * <code>ssh-keygen</code>
    
    * File where you need to save the key <em>/Users/Udacity/.ssh/linuxCourse</em>
    
  * Installing a Public Key
  
    * Log into your server as the new user you've just created
    
    * <code>mkdir .ssh</code>
    
    * <code>touch .ssh/authorized_keys</code>
    
    * <code>cat .ssh/linuxCourse.pub</code>
    
    * <code>nano .ssh/authorized_keys</code>
    
    * Paste the linuxCourse.pub content into .ssh/authorized_keys in your server with the new user.
    
    * <code>chmod 700 .ssh</code>
    
    * <code>chmod 644 .ssh/authorized_keys</code>
    
    <h3>Log in using the Key pair</h3>
    
        - <code>ssh {{name}}@127.0.0.1 -p2222 -i ~/.ssh/linuxCourse </code>
        
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
   
   * Modify file <code>nano /etc/apache2/sites-enabled/000-default.conf</code>
   
   * Add this<code>WSGIScriptAlias / /var/www/html/myapp.wsgi</code> line before the closing tag VirtualHost
   
   * Then restart Apache </code>sudo apache2ctl restart</code>
   
   * You will now install PostgreSQL to server your data using the command <code>sudo apt-get install postgresql</code>
   
   
   
   * Install:
      - sudo pip install sqalchemy
      - sudo pip install psycopg2
    

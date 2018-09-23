# LightSpeed Install with Ubuntu
## Notes
PHP Configuration -> /usr/local/lsws/lsphp72/etc/php/7.2/litespeed/php.ini

## Shell commands for installation.
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 011AA62DEDA1F085
sudo wget -O /etc/apt/trusted.gpg.d/lst_repo.gpg http://rpms.litespeedtech.com/debian/lst_repo.gpg
wget -O - http://rpms.litespeedtech.com/debian/enable_lst_debain_repo.sh | bash
apt-get install lsphp72

## Configuring LiteSpeed PHP7 Handler
### Web Interface Method
Navigate to WebAdmin console > Server > External App and click “Add”. Then select “LSAPI App” from drop down menu and click “Next”.

Now set your PHP 7 configuration. The following is an example of a typical setting, make sure you set the PHP binary path to the right location: set “command” to “$SERVER_ROOT/lsphp70/bin/lsphp”; the rest of configurations can be adjusted according to your situation.

Name: lsphp70
Address:uds://tmp/lshttpd/lsphp70.sock
Max Connections: 35
Environment: PHP_LSAPI_MAX_REQUESTS=500
             PHP_LSAPI_CHILDREN=35
Initial Request Timeout (secs): 60
Retry Timeout : 0
Response Buffering: no
Start By Server: yes
Command: $SERVER_ROOT/lsphp70/bin/lsphp
Back Log: 100
Instances: 1
Memory Soft Limit (bytes): 2047M
Memory Hard Limit (bytes):2047M
Process Soft Limit: 400
Process Hard Limit: 500

### Command Line Method 
If you prefer using the command line instead of the GUI tool, you can edit the LSWS configuration file (usually /usr/local/lsws/conf/httpd_config.xml) by adding the following to the <extProcessorList>… </extProcessorList> section:

<extProcessor>
    <type>lsapi</type>
    <name>lsphp72</name>
    <address>uds://tmp/lshttpd/lsphp72.sock</address>
    <note></note>
    <maxConns>35</maxConns>
    <env>PHP_LSAPI_MAX_REQUESTS=500</env>
    <env>PHP_LSAPI_CHILDREN=35</env>
    <initTimeout>60</initTimeout>
    <retryTimeout>0</retryTimeout>
    <persistConn></persistConn>
    <pcKeepAliveTimeout></pcKeepAliveTimeout>
    <respBuffer>0</respBuffer>
    <autoStart>1</autoStart>
    <path>$SERVER_ROOT/lsphp72/bin/lsphp</path>
    <backlog>100</backlog>
    <instances>1</instances>
    <extUser></extUser>
    <extGroup></extGroup>
    <runOnStartUp></runOnStartUp>
    <extMaxIdleTime></extMaxIdleTime>
    <priority></priority>
    <memSoftLimit>2047M</memSoftLimit>
    <memHardLimit>2047M</memHardLimit>
    <procSoftLimit>400</procSoftLimit>
    <procHardLimit>500</procHardLimit>
  </extProcessor>

Set script handler to PHP 7
Before you set up PHP 7 to handle your PHP applications, please be aware that PHP 7.0.0 is still in the Alpha 1 stage and is not recommended for production yet. We suggest setting this in your test or staging environment first.

Navigate to WebAdmin console > Server > Script Handler, where you might see that the “php” suffix already exists. Click “edit” for “php”, set “Handler Type” to “LiteSpeed LSAPI”, and then set the “Handler Name” to the one you just created. For example “ lsphp 70”. Lastly save the changes.

Again, if you are a command line Guru and prefer to make the changes through the LSWS configuration file, simply edit the httpd_config.xml file and add the following to your server side <scriptHandlerList>… </scriptHandlerList> section:

  <scriptHandler>
    <suffix>php</suffix>
    <type>lsapi</type>
    <handler>lsphp70</handler>
    <note></note>
  </scriptHandler>
Perform a Graceful restart of the server and you're all set. Your applications should now be served by PHP 7.
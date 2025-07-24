When a web server allows users to upload files to its filesystem without sufficiently validating user inputs and failing to properly enforce restrictions

In some cases, the act of uploading the file is in itself enough to cause damage. Other attacks may involve a follow-up HTTP request for the file, typically to trigger its execution by the server.

For uploading, usually is `POST` with headers like: `Content-Type, Content-Lenght, Content-Disposition`
# Impact

Depends on:
1. Which aspect of the file the website fails to validate properly
2. What restrictions are imposed on the file once uploaded

# Attack Vectors

What an attacker can control when uploading files?
## 1. File Name

This could allow an attacker to overwrite critical files simply by uploading a file with the same name. If the server is also vulnerable to [[Directory Traversal]], this could mean attackers are even able to upload files to unanticipated locations.

## 2. File Type

the worst case scenario, the file's type isn't validated properly, and the server configuration allows certain types of file (such as `.php` and `.jsp`) to be executed as code. In this case, an attacker could potentially upload a server-side code file that functions as a web shell, effectively granting them full control over the server.

- If this file type is non-executable, such as an image or a static HTML page, the server may just send the file's contents to the client in an HTTP response.
- If the file type is executable, such as a PHP file, **and** the server is configured to execute files of this type, it will assign variables based on the headers and parameters in the HTTP request before running the script. The resulting output may then be sent to the client in an HTTP response.
- If the file type is executable, but the server **is not** configured to execute files of this type, it will generally respond with an error. However, in some cases, the contents of the file may still be served to the client as plain text. Such misconfigurations can occasionally be exploited to leak source code and other sensitive information.
## 3. File Content

## 4. File Size

Failing to make sure that the size of the file falls within expected thresholds could also enable a form of denial-of-service (DoS) attack, whereby the attacker fills the available disk space.

`MIME Types, Extentions`

```php
<?php echo system($_GET['command']); ?> // Webshell via REQ command="exec"
```
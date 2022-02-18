---
title: TLSAssistant
subtitle: Standard Mitigations
layout: page
---

{% include toc.md %}

# Default Name

While creating a mitigation, you **must**:

* Set the name of the mitigation coherent to the module
* Set the file name in upper case
* Use `_` instead of `space`
* Use `json`
* Set the extension of the file in `.json` lowercase.

For example:

your module `rabbit hole` will have as a mitigation:

* ​	`RABBIT_HOLE.json` 

# Template

While writing a mitigation you should follow this template:

```json
{
  "Entry": {
    "Name": "The name of the vulnerability",
    "ExtendedName": "The extended name of the vulnerability (can be the same as the Name)",
    "Description": "Description of the vulnerability",
    "Mitigation": {
      "Textual": "Textual mitigation.",
      "Snippet": {
        "ServerWeType1": "Snippet",
        "ServerWebType2": "Snippet"
      }
    }
  },
  "#comment": "a",
  "#comment1": "b"
}
```

For example

```json
{
  "Entry": {
    "Name": "Missing Certificate Transparency",
    "ExtendedName": "Missing Certificate Transparency",
    "Description": "With the current configuration, if a CA misissues a valid certificate for this domain, it may take months before being detected. Certificate transparency helps by reducing the interval between detection and mitigation.",
    "Mitigation": {
      "Textual": "Use one of the following SCT delivery methods:\n1. use a CA that embeds the SCT within the certificate;\n2. embed it within a stapled OCSP response;\n3. delivery it using a TLS Extension.\n.",
      "Snippet": {
        "apache": "1. open your Apache configuration file (default: */etc/apache2/sites-available/default-ssl.conf*);\n2. add the following lines: \n    - SSLUseStapling on\n    - SSLStaplingResponderTimeout 5\n    - SSLStaplingReturnResponderErrors off\n    - SSLStaplingCache shmcb:/var/run/ocsp(128000)\n\nN.B. restart the server by typing: `sudo service apache2 restart` and make sure that your certificate has been logged (check at [crt.sh](https://crt.sh) ).",
        "nginx": "In a default situation, you can edit your website configuration */etc/nginx/sites-enabled/default*\n\t(if you changed your site conf name */etc/nginx/sites-enabled/YOURSITECONFIGURATION*);\n\t\n1. Add the following to your server configuration:\n\tssl_session_cache shared:SSL:5m;\n\n  \tssl_session_timeout 5m;\n\t\n\tssl_stapling on;\n  \t\n\tssl_stapling_verify on;\n\t\n2. Restart your server with `sudo service nginx restart` and make sure that your certificate has been logged (check at [crt.sh](https://crt.sh) ).\n"
      }
    }
  },
  "#comment": " https://www.digicert.com/certificate-transparency/enabling-ct.htm ",
  "#comment1": " http://www.certificate-transparency.org/resources-for-site-owners/apache "
}
```

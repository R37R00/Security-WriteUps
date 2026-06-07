# CIDSI presents: Robot 100pts.
## Description:
Un robot que te llevará al futuro....
### Web page:
http://200.9.165.32:8817/
### Steps to solve:
1. As the challenge says "robot" and mention it, the first thing that came up into my mind is the file **robots.txt**
**What is the robots.txt file?**
Is a standard text file used on websites to give instructions to web crawlers(bots).

It tells search engines like Google:
"Which parts of this website you are allowed or not to access."
2. Inside the file we get:

```
# $Id: robots.txt,v 1.97 2022/11/07 19:56:32 jliao Exp $
#
# This is a file retrieved by webwalkers a.k.a. spiders that 
# conform to a defacto standard.
# See <URL:http://www.robotstxt.org/wc/exclusion.html#robotstxt>
#
# Comments to the webmaster should be posted at <URL:http://www.ibm.com/contact>
#
# Format is:
#       User-agent: <name of spider>
#       Disallow: <nothing> | <path>
# ------------------------------------------------------------------------------

User-agent: *
Disallow: //
Disallow: /account/registration
Disallow: /account/mypro
Disallow: /account/myint
Disallow: /Admin
Disallow: /cgi-
Disallow: /cgii
Disallow: /cgii/run
Disallow: /cgii/rules
Disallow: /contact/employees/servlets
Disallow: /data/
Disallow: /db2s
Disallow: /developerworks/*-pdf.pdf$
Disallow: /developerworks/forums/servlet
Disallow: /developerworks/forums/abuse
Disallow: /developerworks/forums/post
Disallow: /docs/api
Disallow: /fcgi-
Disallow: /fscripts
Disallow: /secure/future
Disallow: /homepage
Disallow: /image
Disallow: /mashupm
Disallow: /*.ssi$
Disallow: /account/myibm/InterestsEdit.do
Disallow: /wcs
Disallow: /wcsstore
Disallow: /webapp
Disallow: /web/portal/software/websphere
Disallow: /common/austin-summit
Disallow: /link
Disallow: /links
Disallow: /blog/
Disallow: /web/portal/commerce 
Disallow: /industries/clients
Disallow: /standards

Allow:    /common/ssi
Allow:    /data-responsibility
Allow:    /docs/api/v1/content
sitemap: https://www.ibm.com/homepage_sitemap.xml
sitemap: https://www.ibm.com/internal/cms/sitemap-cms.xml
sitemap: https://www.ibm.com/marketplace/storefront-sitemap-index.xml
sitemap: https://www.ibm.com/downloads/cas/sitemap/sitemap.xml
sitemap: https://www.ibm.com/common/ssi/start/sitemap.xml
sitemap: https://www.ibm.com/content/dam/adobe-cms/IBM_Adobe_Sitemap.xml
sitemap: https://www.ibm.com/mysupport/s/sitemap.xml

```

3. There is a lot of "noise" inside it, so we extract just the suspectful directories:
```
/Admin
/cgii/rules
```
4. Inside /Admin there is just a fake flag, so lets check /cgii/rules
5. And there is the flag!!!
**Note:** Don't forget to hash the flag using MD5


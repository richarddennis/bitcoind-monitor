# bitcoind-monitor

## bitcoind-monitor.py

This is a script written in Python intended to run alongside a Bitcoin node and export statistics for monitoring purposes. It assumes the existence of bitcoin-cli in the PATH and access to the RPC interface over localhost.

It tracks stuff like: block height, difficulty, number of peers, network hash rate, errors, uptime in seconds, mempool size, size of recent blocks, number of transactions within blocks, chaintips, total bytes received and sent, and transaction inputs and outputs. These Bitcoin metrics are refreshed once every 5 minutes.

### How it works

[Prometheus](https://prometheus.io) is a monitoring system and time-series database.

It works by pulling or scraping numerical metrics from an HTTP endpoint (or "exporter"), and then ingesting and keeping track of them over time.

You can then build queries and alerting rules from this data. This code is targeted to users of both Prometheus and Bitcoin.

An exporter setup as a scrape target may be local or remote. Prometheus is a great backend for a visualization and analytics software such as [Grafana](https://grafana.com).

### Testing and requirements

To see it in action, run `bitcoind-monitor.py` and navigate to http://127.0.0.1:8334 in your browser.

Ensure that `prometheus_client` is installed via pip. If you're using Python 2, then the `whichcraft` module is another requirement.

### Running as a service

I'd also recommend running this persistently as a systemd service. For example:

```
[Unit]
Description=bitcoind-monitor
After=network.target bitcoind.service

[Service]
ExecStart=/usr/bin/python /usr/local/bin/bitcoind-monitor.py
KillMode=process
User=bitcoin
Group=bitcoin
Restart=on-failure

[Install]
WantedBy=multi-user.target
```

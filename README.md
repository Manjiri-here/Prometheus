# Prometheus

<img width="1427" height="714" alt="Screenshot 2025-09-10 at 1 17 24 PM" src="https://github.com/user-attachments/assets/0d88c1d4-1c59-4f7b-b8fb-5abf7608e50b" />
----


<img width="1792" height="911" alt="Screenshot 2025-10-07 at 21 43 58" src="https://github.com/user-attachments/assets/d704d349-2d95-49b4-bc0a-f5388bc5c32b" />

-----

<img width="1791" height="1035" alt="Screenshot 2025-10-07 at 21 45 15" src="https://github.com/user-attachments/assets/4a57df8c-8760-43a9-be7f-e4ef13f726cf" />
-----


<img width="1789" height="996" alt="Screenshot 2025-10-07 at 21 45 56" src="https://github.com/user-attachments/assets/f19ff3b4-7d0d-4185-918b-5bc2b23c5676" />
------

**prometheus.yaml file:**

```

# my global config
global:
  scrape_interval: 15s # Set the scrape interval to every 15 seconds. Default is every 1 minute.
  evaluation_interval: 15s # Evaluate rules every 15 seconds. The default is every 1 minute.
  # scrape_timeout is set to the global default (10s).

# Alertmanager configuration
alerting:
  alertmanagers:
    - static_configs:
        - targets:
          # - alertmanager:9093

# Load rules once and periodically evaluate them according to the global 'evaluation_interval'.
rule_files:
  # - "first_rules.yml"
  # - "second_rules.yml"

# A scrape configuration containing exactly one endpoint to scrape:
# Here it's Prometheus itself.
scrape_configs:
  # The job name is added as a label `job=<job_name>` to any timeseries scraped from this config.
  - job_name: "prometheus"

    # metrics_path defaults to '/metrics'
    # scheme defaults to 'http'.

    static_configs:
      - targets: ["localhost:9090"]
        # The label name is added as a label `label_name=<label_value>` to any timeseries scraped from this config.
        labels:
          app: "prometheus"

  - job_name: "node_exporter"

    static_configs:
      - targets : ["3.90.189.219:9100"]

  - job_name: 'blackbox'
    metrics_path: /probe
    params:
      module: [http_2xx]  # Look for a HTTP 200 response.
    static_configs:
      - targets:
        - [http://3.90.189.219:8080/](http://3.90.189.219:8080/)    # Target to probe with http.
    relabel_configs:
      - source_labels: [__address__]
        target_label: __param_target
      - source_labels: [__param_target]
        target_label: instance
      - target_label: __address__
        replacement: 54.221.93.1:9115  # The blackbox exporter's real hostname:port.

```

<img width="1792" height="758" alt="Screenshot 2025-10-07 at 21 46 30" src="https://github.com/user-attachments/assets/d7992594-d9de-45ed-8025-994eeda5a47b" />
----

<img width="1790" height="1053" alt="Screenshot 2025-10-07 at 21 47 55" src="https://github.com/user-attachments/assets/ab1f68ad-68eb-4151-a7d4-4cb1686e5d74" />


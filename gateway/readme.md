# Sumo Logic OpenTelemetry Gateway in a "secure" DMZ network

## The purpose of this readme is to configure a Sumo Logic Open Telemetry (otel) collector as a gateway (i.e., receive data from another otel collector). The other otel collector would be in a secure network that does NOT have access to the internet, requiring a multi-step process. This document is assuming you have created an agent config. If you are looking to configure the agent on a secure network please see [here](/agent-config.md). 

#### Step 1 - Install the collector

- Navigate to your Sumo Logic tenant -> Collection -> OpenTelemetry Collection

![otel-collection](/screenshots/gateway/otel-collection.png)

- Create a new installation token 
- Add any custom tags, in this example I did device = gateway
- Copy and run the command the UI provides you to install the collector. Once the installation is completed, you will see a green box indicating the collector registered successfully. 

![otel-config-example](/screenshots/gateway/otel-collection-config.png)

#### Step 2 - Create your config file 

- Navigate and create a new yaml file in ```
/etc/otelcol-sumo/conf.d ``` (the name can be anything - in this example I named it http.yaml) 

```yaml
receivers:
  otlp:
    protocols:
      grpc:
      http:

processors:
  resource/something:
    attributes:
      - key: _sourceCategory
        value: otel/linux/testlogs
        action: insert

service:
  pipelines:
    logs/something:
      receivers:
        - otlp
      processors:
        - resource/something
      exporters:
        - sumologic
```

- Restart the collector by running the following command: 
``` 
systemctl restart otelcol-sumo
```

- You can then validate the collector is running by running the following command: 
```
systemctl status otelcol-sumo 
```

#### Step 3 - Validate Logs Coming into Sumo
 - You can leverage [Live Tail](https://help.sumologic.com/docs/search/live-tail/about-live-tail/) to validate logs are coming into the platform. 

- From the otel collection page in your Sumo tenant you can open in a log search
![log-search](/screenshots/gateway/log-search.png)

- This will open you up into a log search, from there you can run the search or select live tail
![live-tail](/screenshots/gateway/live-tail.png)

- If everything is configured correctly, you should start to see logs flowing into your sumo instance
![data](/screenshots/gateway/live-tail-complete.png)


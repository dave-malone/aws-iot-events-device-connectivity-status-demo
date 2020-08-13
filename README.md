
# Description

The following AWS IoT Events detector model uses Lifecycle events from AWS IoT Core to then determine whether or not the device is in an offline state. Considering that IoT "Things" can experience intermittent connectivity issues, this model aims to reduce the issues experienced when devices are able to reconnect within a given amount of time before marking a device as "disconnected".

This model is intended to be used alongside AWS IoT Core, with a Rule configured to subscribing to `$aws/events/presence/#` and an Action configured to send these events to AWS IoT Events. However, that configuration is outside of the scope of this particular demo, since this demo can be run using the AWS CLI to emulate these events. This repo contains test payloads that model these events to help quickly iterate on the design and development of your IoT Events detector models.

# Pre-requisites

* SNS topic configured and ARN replaced in `ConnectivityStatusMonitorModel.json` (line 56)
* IAM service role configured for IoT Events and ARN replaced in `ConnectivityStatusMonitorModel.json` (line 257)


# Setup

Import the Input:

```bash
aws iotevents create-input \
  --cli-input-json file://lifecycle-event.json
```

Import the Detector Model:

```bash
aws iotevents create-detector-model \
  --cli-input-json file://ConnectivityStatusMonitorModel.json
```

# Testing

The following CLI commands can be run in any order to help evaluate the state transitions, variables, and timers as they are configured in the detector model contained in this application. 

```bash
aws iotevents-data batch-put-message \
    --cli-input-json file://iot-events-connected-messages.json


aws iotevents-data batch-put-message \
    --cli-input-json file://iot-events-disconnected-messages.json
```


## Resources

* IoT Core lifecycle events: https://docs.aws.amazon.com/iot/latest/developerguide/life-cycle-events.html
* IoT Events docs: https://docs.aws.amazon.com/iotevents/latest/developerguide/iotevents-getting-started.html
* AWS IoT Events re:Invent 2018: https://www.youtube.com/watch?v=3MQoTIbFCJ8
* IoT Events pricing: https://aws.amazon.com/iot-events/pricing/

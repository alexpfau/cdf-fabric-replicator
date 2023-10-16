# CDF Time Series replicator

Service which utilize the TimeSeries subscriptions API to replicate / stream time series datapoints to other destinations.

Currently it supports streaming data to:
* Other CDF projects
* Azure Event Hub

Before running the extractor you need to setup a data point subscriptions, see the [SDK documentation](https://cognite-sdk-python.readthedocs-hosted.com/en/latest/core_data_model.html#create-data-point-subscriptions)

Configuration file:
```
# Cognite project to stream your datapoints from
cognite:
    host: ${COGNITE_BASE_URL}
    project: ${COGNITE_PROJECT}

    idp-authentication:
        token-url: ${COGNITE_TOKEN_URL}
        client-id: ${COGNITE_CLIENT_ID}
        secret: ${COGNITE_CLIENT_SECRET}
        scopes:
            - ${COGNITE_BASE_URL}/.default
    extraction-pipeline:
        external-id: ts-sub

#Extractor config
extractor:
    state-store:
        local:
            path: state.json
    subscription-batch-size: 10000
    ingest-batch-size: 100000
    poll-time: 5

# destinations supported is event hub and CDF. Minimum one destination required.
destinations:
  - connection_string: ${EVENTHUB_CONN_STR}
    eventhub_name: ${EVENTHUB_NAME}
  - project: murad-lab
    host: ${DST_COGNITE_BASE_URL}
    idp-authentication:
        client_id: ${DST_COGNITE_CLIENT_ID}
        secret: ${DST_COGNITE_CLIENT_SECRET}
        token_url: ${DST_COGNITE_TOKEN_URL}
        scopes:
            - ${DST_COGNITE_BASE_URL}/.default
        audience: ${DST_COGNITE_AUDIENCE}
    external_id_prefix: subtest_

# subscriptions to stream
subscriptions:
  - externalId: ts-subscription
    partitions:
        - 0

```
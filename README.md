# Technical Task for a position of DWH Engineer @ Spark Networks <br> Onsite Coding Part

Dear candidate

We are very happy to meet you in our Berlin HQ!

As the technical part of the onsite set of interviews, we would like you to extend the data pipeline (see the diagram below) you started building at home while working on our technical task.

## Data pipeline diagram

```bash
io_source_raw_data
           |
          \|/
service_fetcher_batcher_pusher_raw_data
           |
          \|/
storage_incoming_raw_data    
           |
          \|/
service_loader_raw_data
           |
          \|/
[storage_hot,
  storage_cold,
  io_source_loaded_data]
           |
          \|/
service_processor_raw_data
           |
          \|/
[storage_hot,
  io_source_processed_data,
  service_output_processed_data]
```

The above diagram illustrates a data pipeline which can be built to load and process the data from <a href="https://www.immobilienscout24.de" target="_blank">immobilienscout24.de</a>. You developed the service <strong>service_loader_raw_data</strong> to load raw data batches from <strong>storage_incoming_raw_data</strong> into <strong>storage_hot</strong>. Today's mini-project is, to extend the pipeline and build the stateless service <strong>service_fetcher_batcher_pusher_raw_data</strong> to fetch raw data from the source <strong>io_source_raw_data</strong> into <strong>storage_incoming_raw_data</strong>.

## API end-points

The raw data can be fetched from <strong>io_source_raw_data</strong> using API end-points described <a href="https://documenter.getpostman.com/view/6808396/S11RKFo6" target="_blank">here</a>.

## service_fetcher_batcher_pusher_raw_data logic diagram

The following logic can be proposed for the <strong>service_fetcher_batcher_pusher_raw_data</strong> service:

```python
GET /pagination -> number of pages, n_page with ads

for iPage in range(1,n_page,1):

  GET /id for the page iPage -> list of ads, list_ads on the page iPage

    for iFlat in list_ads:

      GET /data for the flat iFlat -> append micro-batch to the final data batch batch_out

DUMP batch_out into storage_incoming_raw_data
```

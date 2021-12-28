# `S3 Select` — 

It enables applications to retrieve only a subset of data from an object by using `simple SQL expressions`.  
By using `S3 Select` to retrieve only the data needed by the application,  
we can achieve drastic performance increases – in many cases we can get as much as a `400%` improvement.  

![image](https://user-images.githubusercontent.com/26399543/147582517-87a02689-460f-4732-843e-e92b78ad4f8e.png)

As an example, let's assume, we need to analyze the weekly sales data from a single store,  
but the data for all 200 stores is saved in a new GZIP-ed CSV every day.  
Without S3 Select, we would need to download, decompress and process the entire CSV to get the needed data.  
With S3 Select, we can use a `simple SQL expression` to return only the data from the store we're interested in,  
instead of retrieving the entire object. 
This means we're dealing with an order of magnitude less data which improves the performance of the underlying applications.  

Let’s look at a quick Python example, which shows how to retrieve the first column from an object containing data in CSV format.  

```python
import boto3
s3 = boto3.client('s3')

r = s3.select_object_content(
        Bucket='jbarr-us-west-2',
        Key='sample-data/airportCodes.csv',
        ExpressionType='SQL',
        Expression="select * from s3object s where s.\"Country (Name)\" like '%United States%'",
        InputSerialization = {'CSV': {"FileHeaderInfo": "Use"}},
        OutputSerialization = {'CSV': {}},
)

for event in r['Payload']:
    if 'Records' in event:
        records = event['Records']['Payload'].decode('utf-8')
        print(records)
    elif 'Stats' in event:
        statsDetails = event['Stats']['Details']
        print("Stats details bytesScanned: ")
        print(statsDetails['BytesScanned'])
        print("Stats details bytesProcessed: ")
        print(statsDetails['BytesProcessed'])
```

It's `recommended` to to use `S3 Select` to accelerate all sorts of applications.  
For example, this partial data retrieval ability is especially useful for serverless applications built with `AWS Lambda`.  


# Considerations and limitations - 
1. Amazon S3 server-side encryption with customer-provided encryption keys (SSE-C) and client-side encryption are not supported.
2. The `AllowQuotedRecordDelimiters` property is not supported. If this property is specified, the query fails.
3. **`Only CSV and JSON files in UTF-8 format are supported`**.  
**Multi-line CSVs and JSON are not supported.**
4. Only uncompressed or gzip or bzip2 files are supported.
5. Comment characters in the last line are not supported.
6. Empty lines at the end of a file are not processed.


**Biggest limitation** — It only supports CSV and JSON file formats  

**Conclusion** —  
Query pushdown using `S3 Select` is now supported with `Spark`, `Hive` and `Presto in Amazon EMR`.  
We can use this feature to `push down` the computational work of filtering large data sets for processing from the EMR cluster to Amazon S3,  
which can improve performance and reduce the amount of data transferred between `Amazon EMR` and `Amazon S3`.  

**Reference:**  
1. https://aws.amazon.com/blogs/aws/s3-glacier-select/


<!--

    Licensed to the Apache Software Foundation (ASF) under one
    or more contributor license agreements.  See the NOTICE file
    distributed with this work for additional information
    regarding copyright ownership.  The ASF licenses this file
    to you under the Apache License, Version 2.0 (the
    "License"); you may not use this file except in compliance
    with the License.  You may obtain a copy of the License at
    
        http://www.apache.org/licenses/LICENSE-2.0
    
    Unless required by applicable law or agreed to in writing,
    software distributed under the License is distributed on an
    "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY
    KIND, either express or implied.  See the License for the
    specific language governing permissions and limitations
    under the License.

-->

# Native API Comparison

This chapter mainly compares the differences between Java Native API and python native API, mainly for the convenience of distinguishing the differences between Java Native API and python native API.



| Order | API name and function                 | Java API                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  | Python API                                                   | <div style="width: 200pt">API Comparison</div>                                               |
| ----- | ------------------------------------- |-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------| :----------------------------------------------------------- | ------------------------------------------------------------ |
| 1     | Initialize session                    | `Session.Builder.build();     Session.Builder().host(String host).port(int port).build();     Session.Builder().nodeUrls(List<String> nodeUrls).build();     Session.Builder().fetchSize(int fetchSize).username(String  username).password(String password).thriftDefaultBufferSize(int  thriftDefaultBufferSize).thriftMaxFrameSize(int  thriftMaxFrameSize).enableRedirection(boolean  enableCacheLeader).version(Version version).build();`                                                                                                                                                                                                           | `Session(ip, port_, username_,  password_,fetch_size=1024, zone_id="UTC+8")` | 1. The python native API lacks the default configuration to initialize the session<br/>2. The python native API is missing the initialization session of specifying multiple connectable nodes<br/>3. The python native API is missing. Use other configuration items to initialize the session |
| 2     | Open session                          | `void open()     void open(boolean enableRPCCompression)`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 | `session.open(enable_rpc_compression=False)`                 |                                                              |
| 3     | Close session                         | `void close()`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            | `session.close()`                                            |                                                              |
| 4     | Create Database                     | `void setStorageGroup(String  storageGroupId)`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            | `session.set_storage_group(group_name)`                      |                                                              |
| 5     | Delete database                  | `void deleteStorageGroup(String  storageGroup)     void deleteStorageGroups(List<String> storageGroups)`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  | `session.delete_storage_group(group_name)     session.delete_storage_groups(group_name_lst)` |                                                              |
| 6     | Create timeseries                     | `void createTimeseries(String  path, TSDataType dataType,TSEncoding encoding, CompressionType compressor,  Map<String, String> props,Map<String, String> tags,  Map<String, String> attributes, String measurementAlias)             void createMultiTimeseries(List<String> paths, List<TSDataType>  dataTypes,List<TSEncoding> encodings, List<CompressionType>  compressors,List<Map<String, String>> propsList,  List<Map<String, String>> tagsList,List<Map<String,  String>> attributesList, List<String> measurementAliasList)`                                                                                                                    | `session.create_time_series(ts_path,  data_type, encoding, compressor,props=None, tags=None, attributes=None,  alias=None)             session.create_multi_time_series(ts_path_lst, data_type_lst, encoding_lst, compressor_lst,props_lst=None,  tags_lst=None, attributes_lst=None, alias_lst=None)` |                                                              |
| 7     | Create aligned timeseries             | `void  createAlignedTimeseries(String prefixPath, List<String>  measurements,List<TSDataType> dataTypes, List<TSEncoding>  encodings,CompressionType compressor, List<String>  measurementAliasList);`                                                                                                                                                                                                                                                                                                                                                                                                                                                    | `session.create_aligned_time_series(device_id,  measurements_lst, data_type_lst, encoding_lst, compressor_lst)` |                                                              |
| 8     | Delete timeseries                     | `void deleteTimeseries(String  path)     void deleteTimeseries(List<String> paths)`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       | `session.delete_time_series(paths_list)`                     | Python native API is missing an API to delete a time series  |
| 9     | Detect whether the timeseries exists  | `boolean  checkTimeseriesExists(String path)`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             | `session.check_time_series_exists(path)`                     |                                                              |
| 10    | Metadata template                     | `public void  createSchemaTemplate(Template template);`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |                                                              |                                                              |
| 11    | Insert tablet                         | `void insertTablet(Tablet  tablet)     void insertTablets(Map<String, Tablet> tablets)`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   | `session.insert_tablet(tablet_)     session.insert_tablets(tablet_lst)` |                                                              |
| 12    | Insert record                         | `void insertRecord(String  prefixPath, long time, List<String> measurements,List<TSDataType>  types, List<Object> values)     void insertRecords(List<String> deviceIds,List<Long>  times,List<List<String>> measurementsList,List<List<TSDataType>>  typesList,List<List<Object>> valuesList)     void insertRecordsOfOneDevice(String deviceId, List<Long>  times,List<List<Object>> valuesList)`                                                                                                                                                                                                                                                       | `session.insert_record(device_id,  timestamp, measurements_, data_types_, values_)     session.insert_records(device_ids_, time_list_, measurements_list_,  data_type_list_, values_list_)     session.insert_records_of_one_device(device_id, time_list, measurements_list,  data_types_list, values_list)` |                                                              |
| 13    | Write with type inference             | `void insertRecord(String  prefixPath, long time, List<String> measurements, List<String>  values)     void insertRecords(List<String> deviceIds, List<Long>  times,List<List<String>> measurementsList, List<List<String>>  valuesList)     void insertStringRecordsOfOneDevice(String deviceId, List<Long>  times,List<List<String>> measurementsList,  List<List<String>> valuesList)`                                                                                                                                                                                                                                                                 | `session.insert_str_record(device_id,  timestamp, measurements, string_values)` | 1. The python native API lacks an API for inserting multiple records<br/>2. The python native API lacks the ability to insert multiple records belonging to the same device |
| 14    | Write of aligned time series          | `insertAlignedRecord     insertAlignedRecords     insertAlignedRecordsOfOneDevice     insertAlignedStringRecordsOfOneDevice     insertAlignedTablet     insertAlignedTablets`                                                                                                                                                                                                                                                                                                                                                                                                                                                                             | `insert_aligned_record     insert_aligned_records     insert_aligned_records_of_one_device     insert_aligned_tablet     insert_aligned_tablets` | Python native API is missing the writing of aligned time series with judgment type |
| 15    | Data deletion                         | `void deleteData(String path,  long endTime)     void deleteData(List<String> paths, long endTime)`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |                                                              | 1. The python native API lacks an API to delete a piece of data<br/>2. The python native API lacks an API to delete multiple pieces of data |
| 16    | Data query                            | `SessionDataSet  executeRawDataQuery(List<String> paths, long startTime, long  endTime)     SessionDataSet executeLastDataQuery(List<String> paths, long  LastTime)`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |                                                              | 1. The python native API lacks an API for querying the original data<br/>2. The python native API lacks an API to query the data whose last timestamp is greater than or equal to a certain time point |
| 17    | Iotdb SQL API - query statement       | `SessionDataSet  executeQueryStatement(String sql)`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       | `session.execute_query_statement(sql)`                       |                                                              |
| 18    | Iotdb SQL API - non query statement   | `void  executeNonQueryStatement(String sql)`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              | `session.execute_non_query_statement(sql)`                   |                                                              |
| 19    | Test API                              | `void testInsertRecord(String  deviceId, long time, List<String> measurements, List<String>  values)     void testInsertRecord(String deviceId, long time, List<String>  measurements,List<TSDataType> types, List<Object> values)     void testInsertRecords(List<String> deviceIds, List<Long>  times,List<List<String>> measurementsList,  List<List<String>> valuesList)     void testInsertRecords(List<String> deviceIds, List<Long> times,List<List<String>>  measurementsList, List<List<TSDataType>>  typesList,List<List<Object>> valuesList)     void testInsertTablet(Tablet tablet)     void testInsertTablets(Map<String, Tablet> tablets)` | Python client support for testing is based on the testcontainers library | Python API has no native test API                            |
| 20    | Connection pool for native interfaces | `SessionPool`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |                                                              | Python API has no connection pool for native API             |
| 21    | API related to cluster information    | `iotdb-thrift-cluster`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |                                                              | Python API does not support interfaces related to cluster information |
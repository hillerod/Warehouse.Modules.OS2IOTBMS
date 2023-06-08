# Warehouse module: OS2IOTBMS

## Introduction

With this Azure module, you can setup an environment in Azure, to make communication between OS2IOT and an BMS-system.
 
### Getting payloads

Data from the sensors gets send to the server and then transferred to the OS2IOT server as a payload. These packages are compressed to be as tiny as possible, so In OS2IOT, technical users can setup their own payload decoders, or use the library of decoders, to decompress the payloads.

OS2IOT then has a HTTP Push service that pushes the decoded payloads out to a receiver. To set OS2IOT up to send data to the Azure module, you need to set up a data-target in OS2IOT with a datatarget-URL and an Authorization header.
The URL is a path to the `PostPayloads` function, that's like: `https://os2iot-v3xshh3xyxllq.azurewebsites.net/api/PostPayloads`.
You defined The Authorization header when installing this Azure module. It can be changed At the Azure Functions under Configuration where it has the name: `PostPayloadsAuthorizationKey`.

Payloads are received and saved directly down to the data lake as queue massages.

Each 5 minute another function called `IngestQueuedPayloads`, asks for all messages in the data lake and converts them to a CSV-stream (a comma separated file-format). The CSV gets saved into the data lake for history purpose, and the csv gets send to the database in one bulk.
If data contains column names that are not defined in the database, new columns will be added. And if the datatype has been changed on an already created column, the datatype will be updated.

All the ingested queue messages from the data lake gets deleted.
If the function `IngestQueuedPayloads` or the database is not working properly, messages will be saved for up to 7 days before the oldest will expire. So if the database has been down for 2 days, no data will be lost.

## Contact

For information or consultant hours, please write to bygdrift@gmail.com.

## Installation

All modules can be installed and facilitated with ARM templates (Azure Resource Management): [Use ARM templates to setup and maintain this module](https://github.com/hillerod/Warehouse.Modules.OS2IOTBMS/tree/master/Deploy).

# License

[MIT License](https://github.com/hillerod/Warehouse.Modules.OS2IOTBMS/blob/master/License.md)

# Data Processing Service Compose

## Summary

This project contains a docker compose file and other required files to start the data processing stack alongside applications that 
1) monitor a folder for input documents to send for processing.
2) output the result of processing those documents to a folder on disk.

Documentation for Data Processing can be found [here](https://cafdataprocessing.github.io/data-processing-service).

## Containers

The containers started by this compose file are;

### rabbitmq

A rabbitmq message queue that is used for communication with the workers.

### processingDB

A postgres database instance with the workflow, classification and boilerplate databases installed, along with types required for the default workflow. Further details 
on this container can be found [here](https://github.com/CAFDataProcessing/data-processing-service/tree/v1.0.0/utils/data-processing-databases-container).

### workflowAdmin

A core policy API web service that is used by the processing API web service.

### processingAPI

A data processing API web service that can be used to create workflows. The task submitter application also uses this to initialize the workflow for messages it sends when no workflow ID is provided. The API documentation can be found [here](https://cafdataprocessing.github.io/data-processing-service/pages/en-us/Data_Processing/API)).

### boilerplateAPI

A web service used for the creation of boileplate expressions and tags used as part of the boilerplate action type in a workflow. Details on the boilerplate service can be found [here](https://github.com/CAFDataProcessing/boilerplate-service).

### taskSubmitter

A container that will monitor a provided folder for documents and submit task messages for each document it finds to the workflow worker. It can be set to either pass a pre-existing workflow ID on these messages or to create a workflow for use in the submitted messages. By default this compose file will have the submitter create its default workflow and a set of default boilerplate expressions. The default workflow performs the following processing;

####	Generate Binary Hash of document

* Handled by the [Binary Hash Worker](https://github.com/CAFDataProcessing/worker-binaryhash).

####	Renaming of metadata fields

* Handled by the [Field Mapping Handler](https://github.com/CAFDataProcessing/worker-policy/tree/v1.0.0/handlers-converters/handlers/field-mapping).

#### Identification of elements within emails and documents

* Handled by the [Markup Worker](https://github.com/CAFDataProcessing/worker-markup).

#### Language detection

* Handled by the [Language Detection Worker](https://github.com/CAFDataProcessing/worker-languagedetection).

#### Detection of expressions defined via the Boilerplate API

* Handled by the [Boilerplate Worker](https://github.com/CAFDataProcessing/boilerplate-service).

#### Classification of document against defined classifications

* By default the task submitter will create a classification workflow for the document to be compared against. These classifications are Travel Documents, Receipts, Social Networking Notifications, Newsletters, Tracking Notifications and Meeting Notifications.

#### Send result to designated Output Queue

* Handled by the generic queue output action type.

Further details on the task submitter application can be found [here](https://github.com/CAFDataProcessing/data-processing-service/tree/v1.0.0/utils/data-processing-task-submitter).

### taskReceiver

Application to receive messages that are output after a document has been fully processed by the workflow worker. Details about the output message will be written to folder on disk. More details about the task receiver application can be found [here](https://github.com/CAFDataProcessing/data-processing-service/tree/v1.0.0/utils/data-processing-task-receiver).

### workflowWorker

The workflow worker co-ordinates what processing should be performed on a document based on a workflow that is defined beforehand. More details on it can be found on the data processing documentation site [here](https://cafdataprocessing.github.io/data-processing-service/pages/en-us/Overview) and the GitHub repository for the worker [here](https://github.com/CAFDataProcessing/worker-policy). The version of the worker container used here includes the handlers and converters required for the default workflow defined in the task submitter. Further detail on the container can be found [here](https://github.com/CAFDataProcessing/worker-policy/tree/v1.0.0/worker-data-processing-container).

### binaryHashWorker

Worker to generate a binary hash for a provided document. Its GitHub repository is located [here](https://github.com/CAFDataProcessing/worker-binaryhash).

### markupWorker

Worker to identify elements within emails and documents such as separate message threads. Its GitHub repository is located [here](https://github.com/CAFDataProcessing/worker-markup).

### langDetectWorker

Worker to detect the most likely languages that the text in a document is written in. Its GitHub repository is located [here](https://github.com/CAFDataProcessing/worker-languagedetection).

### boilerplateWorker

Worker to compare the text of a document against a set of defined expressions (through the boilerplate API) and return which are considered matches. It can also remove or redact the content of matched expressions. Its GitHub repository is located [here](https://github.com/CAFDataProcessing/boilerplate-service).

### classificationWorker

Worker to classify the document metadata against the classifications that have been defined. This is the Policy Worker running in Elasticsearch mode. Its GitHub repository is located [here](https://github.com/CAFDataProcessing/worker-policy).

## Configuration

All the deployed services are configured through environment variables in the compose file.

### Input Folder

This is the path that will be monitored by the task submitter application. Each document in the folder on startup will have a task message sent to the workflow worker so that the document is processed. Any new files added to the folder will also have task messages sent to the workflow worker. The default location of this folder is a directory 'input' in the same location as the compose file. If the folder does not exist it will be created on docker-compose up by the task submitter.

The location of this folder may also be set via the environment variable 'INPUT_DOCUMENTS_LOCAL_FOLDER', which will be resolved by the compose file.

### Output Folder

This is that path that messages output at the end of processing will be output to on disk so their results may be examined. The default location of this folder is a directory 'output' in the same location as the compose file. If the folder does not exist it will be created on docker-compose up by the task submitter.

The location of this folder may also be set via the environment variable 'OUTPUT_DOCUMENTS_LOCAL_FOLDER', which will be resolved by the compose file.

## Running

Navigate to a directory with the compose files and run a docker-compose up command to launch the containers.

```
docker-compose up -d
```

On startup the workflow database and a default workflow are installed to the database container. This can take around 5 minutes and messages will not be submitted until after this installation. The state of the setup can be determined by checking the logs of the task-submitter container as it will report when the Processing API becomes able to handle requests.

```
docker logs a0b
```

* Replace 'a0b' with the container ID of the running data-processing-task-submitter image.

To stop the containers run the following command;

```
docker-compose down -v
```

### Debug Deployment

A compose file that adds additional debugging capability to the data processing services. This exposes additional ports on the deployed services and increases log levels. It should be used by passing the debug file as an override file, as shown below;

```
docker-compose -f docker-compose.yml -f docker-compose.debug.yml up -d
```
# Creating an Audit Trail of Device Twin Changes

## Introduction

IoT Hub provides a great feature called Device Twin. Device Twin is a digital representation of your physical device attributes (tags), configuration (desired properties), and status (reported properties) — the Device Twin for each device represented by a JSON document. IoT Hub provides a timestamp of the last update for each object in the device twin as well as a version number which increments every time the object has been change. For many scenarios, this information is enough to provide an edit trail of the changes to the device twin document. In others, a more granular view of the changes is required. In this quick start, I plan to show you how you can maintain an audit trail of the changes made to the device twin.

## Approach

There are two ways you could create an audit trail of changes to the device twin. You could instrument your code so that every time a call to the device twin API is made, you would log the new value and perhaps if your application provides it, the user who made the changes. This approach requires changes to your existing code base and has the limitation of only being able to capture changes made via your code. In other words, if changes are made via the portal or a different application, your audit trail will be out of sync.

The other approach and the one we will focus on is to take advantage of IoT Hub's message routing feature. Message routing allows you to route telemetry messages, device twin changes, and device life cycle changes to an endpoint (Event Hubs, Service Bus Queues or Topics and Blob Storage). This approach does not require any changes to your existing application and could be accomplished without writing one line of code.

## Configuration

For simplicity, we will use Blob Storage as the endpoint as it will provide us with the data persistence needed. If you need active reporting on the changes, you might want to consider routing to Event Hubs and then using Azure Functions to persist the changes to CosmoDB.

### Steps

1. From the Azure Portal, click on your instance of IoT Hub and then click on Message Routing (Left hand menu).
2. Click on the +Add button to add a new route.
3. On the Add a route screen
   1. Provide a name for your route
   2. If you don't have an endpoint define you can click on Add endpoint, select Blob Storage and point to a container
   3. Under Data source, select Device Twin Change Events.
   4. Click Save

That is all. You should be able to see device twin changes in the blob storage container you selected. 

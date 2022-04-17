# CS_ProductPricingTest

Two components are included in this project:
1. PriceSend: this is the service that sends prices over a RabbitMQ queue
2. PriceReceive: this service can have multiple instances and subscribes to messages from PriceSend
via the RabbitMQ queue.

As mentioned PriceSend uses RabbitMQ to publish messages and a 
RabbitMQ server must be running for it to work

I have used routing keys to allow for multiple PriceReceive instances to run and the PriceSend will
randomly select a routing key and publish to that instance.

Assumption: No more than 3 PriceReceive instances will be run. Under normal circumstances this
would be handled by config files.

PriceSend will randomly select a product and a price and publish it on the queue.
Press q and enter to terminate PriceSend

PriceReceive will clear the queue when it starts up to remove any stale prices
It takes one command line argument which is a numeric value representing the routing key 
that the service will be listening on.

For example: dotnet run 1
This will then listen to messages being sent to routing key 1 only.

Once an price that is below or at the option price for a product is received all instances
of PriceReceive will terminate from processing any more messages.
The PriceSend will continue to publish prices though.

Press enter to terminate PriceReceive

A PurchaseOrder object is populated with the details of the trade, but nothing further 
is done with it.

Unit tests have been provided where appropriate.

# Notes on Stuff
## REMOTE PROCEDURE CALL (RPC)
- Cause a procedure to execute in a different address space

- Client-server interaction, inter-process communication

## Request-response protocol:
- Initiated by client -> send to a known remote server -> execute procedure with supplied parameters
	- Remote server sends response to client -> app continues
- while server processing the call, client is blocked unless sends an asynchronous request to server
- sequence of events:
	+ client calls client stub: local procedure call, para in stack
	+ client stub packs para (marshalling) -> make system call -> send the message
	+ client's local OS sends mess from client machine -> server machine
	+ local OS on server machine passes packets -> server stub
	+ server stub unpacks para (unmarshalling)
	+ server stub calls server procedure


## MESSAGE QUEUE:
- used for inter-process communication, or inter-thread communication within the same process, or inter-application communication
- decouples the sender from the receiver
- provide asynchronous communications protocol:
	+ a message in the message queue don't require an immediate response
	+ messages placed onto the queue stored until the recipient retrieves them


## RABBITMQ:
- RPC in RabbitMQ:
	+ a client/publisher sends a mesasge and a server/consumer responses
	+ the client must know which queue it's supposed to listen to for the the response from the server
	+ the server also needs to send the response to the matched queue for the client be able to receive it


### WORD DEFINITIONS:
- synchronous vs asynchronous:
	+ synchronous: tasks/processes are aware of each other and must wait for another to finish before moving on
	+ asynchronous: they are not aware of others, totally "independent"

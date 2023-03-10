## Sample project with microservice architecture.
---

This is a sample application built with microservice architecture with 4 standalone services:
- API
- Simple Parser 
- Message Queue
- Worker


_Note: The microservices are intentionally made as simple as possible to showcase how the entirety of an application works as a system._ 

---
### Application workflow:

--> Parser microservice each 5 seconds parses the image from given url, saves it to the filesystem and sends its path to specific RabbitMQ Exchange \ Queue. 

--> From there, Worker microservice consumer listens specified Exchange / Queue and redirects messages further to celery tasks, thereby consuming a message from queue.

--> Lastly, workers having received messages make an HTTP request to the API microservice and the flow is now complete.


Overall workflow  | `Parser` --> `RabbitMQ` --> `Celery` --> `API` 

---
Start the project with:
```sh
make up
```

It is easy to send custom message to queue with:
```
# Update env variables if needed
export $(cat .env.compose)
make msg
```

_**Note:** There is also an unused service `worker_pika` which is a Worker microservice with custom Pika consumer implementation with running daemonized Celery workers. Left there as a reference for daemonized Celery application._
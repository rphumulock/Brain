#backend #postgres

---
## Request - Response

#### Pattern
1. Client prepares the Request.
2. Client sends a Request.
	- Sent over TCP/UDP etc
3. Server parses the Request.
	- Where does this Request get handled? 
4. Server processes the Request.
	- This can include deserialization/parsing the Request.
	- There is an expense to parsing/deserializing the Request: XML > JSON > protobufs.
5. Server sends a Response.
6. Client parses the Response and consumes.
	- More deserialization/parsing occurs.
![[Screen Shot 2023-03-20 at 11.25.57 PM.png]]
#### Where is it used
- Web, HTTP, DNS, SSH
- RPC (remove procedure call)
- SQL
- APIs (REST/SOAP/GraphQL)

#### Anatomy
- A request structure is defined by both client and server. Libraries will parse the request packets and put them back together (ex. HTTP) - you don't have to worry about it. Every library you utilize is doing work as well as your own code.
- Request/Response has a boundary defined by a protocol and a message format.
![[Screen Shot 2023-03-20 at 11.32.08 PM.png]]

#### Different techniques can be used
Building an upload image service with Request/Response
- Send a large request with the image (simple).
- Chunk image and send request per chunk (resumable).

#### Request/Response will not work everywhere
- Notification Service/ Chatting Application?
	- You would have to use polling to continously check if you had a notification.
	- Doesn't scale well.
- Very long Requests?
	- Asynchronous is probably better.
- What if a disconnect occurs?

#### cURL
Client library that can be used to send Requests.

----
## Synchronous - Asynchronous

#### Synchronous I/O
- Caller sends a request and blocks.
- Caller cannot execute any code in the mean time. 
- Receiver responds, Caller unblocks.

#### Pattern
1. Program asks OS to read from disk.
2. Program main thread is taken off the CPU.
3. Read completes, program can resume execution.

#### Asynchronous I/O
- Caller sends a request
- Caller can work until it gets a reponse
- Caller either:
	- Checks if response is ready ([epoll](https://en.wikipedia.org/wiki/Epoll#:~:text=epoll%20is%20a%20Linux%20kernel,possible%20on%20any%20of%20them.)).
	- Receiver calls back when it's done ([io_uring](https://en.wikipedia.org/wiki/Io_uring)).
	- Spins up a new thread that blocks - but main thread continues.
- Caller and Receiver are not in sync.

#### Pattern
1. Program spins up a secondary thread.
2. Secondary thread reads from disk - OS blocks it.
3. Main program still running and executing code.
4. Thread finishes reading and calls back main thread.

#### Synchronicity in Request/Response
- More of the Client trait but most of the time we're usually working Asynchronously.
- Client sends Request and then continues to do the work - why block?

#### Asynchronous patterns are everywhere
- Asynchronous Programming (Promises/Futures).
- Asynchronous Backend Processing.
- [[Reliability and WAL#Asynchronous Commit|Asynchronous Commits]] in Postgres.
- Asynchronous I/O in Linux ([epoll](https://en.wikipedia.org/wiki/Epoll#:~:text=epoll%20is%20a%20Linux%20kernel,possible%20on%20any%20of%20them.)) / ([io_uring](https://en.wikipedia.org/wiki/Io_uring)).
- [[Asynchronous Replication]].
- Asynchronous OS  fsync. (fs cache)
	- Writes are put in a buffer and then flushed to the disc - this is faster and also easier on the HDD/SSD - because they have shelf lives with more wites.

#### Asynchronous Backend Processing
1. Request isn't immediately handled - it is put into a queue.
	- Processing isn't guaranteed to happen right away -backend could be processing other Requests.
2. Client is handed essentially a job ID (Promise) the backend will get back to it when it's Request is processed. Client can go do other work or even disconnect.

---
## Push

#### Used By
- RabbitMQ

#### Request/Response isn't always ideal
- Client wants real time notification from backend.

#### Pattern
1. Client connects to server with a socket.
2. Server sends data to client.
	- Client does not have to request anything.
3. Protocol should be bidirectional.

![[push1.png]]

#### Pros
- Real time.

#### Cons
- Client must be online.
- Client might not be able to handle. (Polling is preferred for light clients)
- Requires a bidirectional protocol.

**Example:**
```
const http = require("http");
const WebSocketServer = require("websocket").server
let connections = [];

//create a raw http server
const httpserver = http.createServer()

 //pass the httpserver object to the WebSocketServer library to do all the job, this class will override the req/res 
const websocket = new WebSocketServer({"httpServer": httpserver })
//listen on the TCP socket
httpserver.listen(8080, () => console.log("My server is listening on port 8080"))

//when a legit websocket request comes listen to it and get the connection
websocket.on("request", request=> {
    const connection = request.accept(null, request.origin)
    connection.on("message", message => {
        //someone just sent a message tell everybody
        connections.forEach (c=> c.send(`User${connection.socket.remotePort} says: ${message.utf8Data}`))
    }) 
    
    connections.push(connection)
    //someone just connected, tell everybody
    connections.forEach (c=> c.send(`User${connection.socket.remotePort} just connected.`))
  
})
 
//client code 
let ws = new WebSocket("ws://localhost:8080");
ws.onmessage = message => console.log(`Received: ${message.data}`);
ws.send("Hello! I'm client")
```

---
## Short Polling

#### Request/Response isn't always ideal
- Request that takes a log time to process.
	- Upload a YouTube video.
- The backend wants to send a notification.

#### Pattern
1. Client sends Request.
2. Server responds immediately with a handle.
3. Server continues to process the Request.
4. Client uses that handle to check the status of the Request.
	- Multiple short Requests/Responses sent for polling.

![[polling-short.png]]

#### Pros
- Simple.
- Good for long running Requests.
- Client can disconect.

#### Cons
- Very chatty.
- Network bandwidth waste.
- Wastes processes on the Backend when it answers "no" repeatedly.

**Example:**
```
const app = require("express")();
const jobs = {}

app.post("/submit", (req, res) =>  {
    const jobId = `job:${Date.now()}`
    jobs[jobId] = 0;
    updateJob(jobId,0); 
    res.end("\n\n" + jobId + "\n\n");
})

app.get("/checkstatus", (req, res) => {
    console.log(jobs[req.query.jobId])
    res.end("\n\nJobStatus: " + jobs[req.query.jobId] + "%\n\n")

} )

app.listen(8080, () => console.log("listening on 8080"));

function updateJob(jobId, prg) {
    jobs[jobId] = prg;
    console.log(`updated ${jobId} to ${prg}`)
    if (prg == 100) return;
    this.setTimeout(()=> updateJob(jobId, prg + 10), 3000)
}
```

---
## Long Polling

#### Used By
- Kafka

#### Request/Response and Short Polling isn't ideal
- Request that takes a log time to process.
	- Upload a YouTube video.
- The backend wants to send a notification.
- Short Polling is too chatty.

#### Pattern
1. Client sends Request.
2. Server responds immediately with a handle.
3. Server continues to process the Request.
	- Server does not respond until it finishes processing.
1. Client uses that handle to check the status of the Request.
	- Client has handle - can disconnect and are less chatty.
	- Client Polls when it wants to.

![[polling-long.png]]

#### Pros
- Less chatty than Short Polling.
- Client can disconnect.

#### Cons
- Not real time.

**Example:**
```
const app = require("express")();
const jobs = {}

app.post("/submit", (req, res) =>  {
    const jobId = `job:${Date.now()}`
    jobs[jobId] = 0;
    updateJob(jobId,0); 
    res.end("\n\n" + jobId + "\n\n");
})

app.get("/checkstatus", async (req, res) => {
    console.log(jobs[req.query.jobId])
    //long polling, don't respond until done
    while(await checkJobComplete(req.query.jobId) == false);
    res.end("\n\nJobStatus: Complete " + jobs[req.query.jobId] + "%\n\n")

} )

app.listen(8080, () => console.log("listening on 8080"));

async function checkJobComplete(jobId) {
    return new Promise( (resolve, reject) => {
        if (jobs[jobId] < 100)
            this.setTimeout(()=> resolve(false),  1000);
        else
            resolve(true);
    })
   
}

function updateJob(jobId, prg) {
    jobs[jobId] = prg;
    console.log(`updated ${jobId} to ${prg}`)
    if (prg == 100) return;
    this.setTimeout(()=> updateJob(jobId, prg + 10), 10000)
}
```

---
## Server Sent Events

#### Used By

#### Request/Response not ideal for notifications backend
- Client wants something in real time.
- Push works but is restrictive.
- Srver Sent Events works with Request/Response
- Designed for HTTPs and isn't WebSockets.

#### Pattern
1. Response has a start and and end.
2. Client sends a Request.
3. Server sends logical events as part of the Response.
	- Server never write the end of the Response.
	- It is still a Request but an unending Response.
1. Client parses the stream data looking for events.

![[sse.png]]

#### Pros
- Real time
- Compatible with Request/response 

#### Cons
- Clients must be online
- Clients might not be able to handle 
- Polling is preferred for light clients
- HTTP/1.1 problem (6 connections max)

**Example:**
```
// Client Code
let sse = new EventSource("http://localhost:8080/stream");
sse.onmessage = console.log

const app = require("express")();
app.get("/", (req, res) => res.send("hello!"));
app.get("/stream", (req,res) => {
    res.setHeader("Content-Type", "text/event-stream");
    send(res);
})

const port = process.env.PORT || 8888;
let i = 0;
function send (res) {
    res.write("data: " + `hello from server ---- [${i++}]\n\n`);
    setTimeout(() => send(res), 1000);
}
app.listen(port)
console.log(`Listening on ${port}`)
```

---
## Publish - Subscribe

#### Request/Response not ideal - can attempt but will break down

#### Pattern
1. Request is sent out to first service - it processes and sends out Response to next service.
2. Continues until it gets back to the original server and then it Responds.
	- What happens if a Response somewhere in the middle of the chain breaks?

![[Pasted image 20230326143108.png]]

#### Pros
- Elegant and Simple
- Scalable

#### Cons
- Bad for multiple receivers
- High Coupling
- Client/Server have to be running
- Chaining, circuit breaking

#### Pattern
1. Client Requests (uploads)
2. Server receives Request and publishes event to broker.
3. Broker recieves and sends out event to all listening services.
4. If the event is relevant to the service - the service will process and return back to broker.
5. Broker will publish new event based on that service and chain continues until it finishes.

![[pub-sub.png]]

#### Pros
- Scales w/ multiple receivers.
- Great for microservices.
- Loose Coupling.
- Works while clients not running.
- Many different implementations.

#### Cons
- Message delivery issues (Two generals problem). DId receiver get the message?
- Complexity.
- Network saturation.

---
## Multiplexing - De-Multiplexing

[[Multiplexing - Demultiplexing]]
Multiplexing, or _muxing_, is a way of sending multiple signals or streams of information over a communications link at the same time in the form of a single, complex signal. When the signal reaches its destination, a process called _demultiplexing_, or _demuxing_, recovers the separate signals and outputs them to individual lines.

#### Pattern
- HTTP1.1 has a max of 6 connections currently in Browsers.
- Will send Request on one of those 6.
- HTTP2 uses one connection and multiplexes multiple Requests using that one connection.
- Has a max ~200 some connections.

![[Pasted image 20230326150809.png]]
![[http2-multip1.png]]

- Can decouple Client from architecture and have Proxy server use Client HTTP1.1 front end but HTTP2 - for backend.
- Will put some load on the backend server to parse the Request.
![[http2-multip2.png]]
**Inverse:**
![[http1.1-multip.png]]

- PostGres works this way.
- Maxmimum number of connections configured that wil lbe used for Requests.
- New Requests will be blocked until a connection frees.
- Can multiplex because order isn't guaranteed on query executions - so what query response data goes to what Request isn't known.
	- Could set this up if you wanted.
![[connection-pooling.png]]

---
## Stateful vs Stateless
>[[Stateful vs Stateless]]

#### Stateful vs Stateless backend
**Stateful**
- Stores state about clients in its memory
- Depends on the information being there 

**Stateless**
- Client is responsible to “transfer the state” with every request
- May store but can safely lose it

#### What makes a backend stateless?
- Stateless backends can store state somewhere else (database)
- The backend remain stateless but the system is stateful
- Can you restart the backend during idle time and the client workflow continue to work?

![[Pasted image 20230326152749.png]]
![[Pasted image 20230326152759.png]]
![[Pasted image 20230326152813.png]]

#### Stateless vs Stateful protocols
- The Protocols can be designed to store state
- TCP is stateful
	- Sequences, Connection file descriptor
- UDP is stateless 
	- DNS send queryID in UDP to identify queries
	- QUIC sends connectionID to identify connectio

#### Stateless vs Stateful protocols
- You can build a stateless protocol on top of a stateful one and vise versa
- HTTP on top of TCP
- If TCP breaks, HTTP blindly create another one
- QUIC on top UDP

#### Complete Stateless System
- Stateless Systems are rare
- State is carried with every request
- A backend service that relies completely on the input

---
## Sidecar Pattern

![[Pasted image 20230326153458.png]]


#### Changing the library is hard
- Once you use the library your app is entrenched
- App & Library “should” be same language
- Changing the library require retesting 
- Breaking changes Backward compatibility 
- Adding features to the library is hard 
- Microservices suffer

#### What if we delegate communication?
- Proxy communicate instead
- Proxy has the rich library
- Client has thin library (e.g. h1)
- Meet Sidecar pattern
- Each client must have a sidecar proxy

![[Pasted image 20230326153650.png]]

**Examples:**
- Service Mesh Proxies.
	- Linkerd, Istio, Envoy.
- Sidecar Proxy Container.
- Must be Layer 7 Proxy.

#### Pros
- Language agnostic (polyglot).
- Protocol upgrade.
- Security.
- Tracing and Monitoring.
- Service Discovery .
- Caching .

#### Cons 
- Complexity.
- Latency (adding two more endpoints).
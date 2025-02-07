## Exercise 12757

### Write a server program and two client programs so that the server can communicate privately to each client individually via a single message queue.

**Exercise 12757** involves writing a server and two client programs using message queues for communication in a way that the server can communicate privately with each client. Each client should receive its own messages privately from the server via a single message queue.

We'll use **POSIX message queues** for inter-process communication (IPC) in this example. POSIX message queues allow processes to exchange messages efficiently. The server will manage the communication and send messages to clients using the same message queue, but each client will be identified by a unique message type or identifier to ensure private communication.

### Key Concepts:
1. **Server Program**: The server will receive messages from clients, process them, and send a response back to each client individually.
2. **Client Programs**: Each client will send a request to the server and then wait for a private response.
3. **Message Queue**: A POSIX message queue is used for inter-process communication. The server will send messages to the clients using this queue.

### Setup:
1. **Server** will create and open a message queue.
2. **Clients** will connect to this message queue, send a request, and wait for a response.
3. The server will send responses to the specific client based on a unique message type.

---

### **Server Program (`server.c`)**:

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <mqueue.h>
#include <errno.h>

#define QUEUE_NAME "/my_message_queue"  // Message queue name
#define MAX_MSG_SIZE 256
#define MAX_MSG_COUNT 10

// Structure for message
typedef struct {
    long mtype;         // Message type (for identifying clients)
    char mtext[MAX_MSG_SIZE]; // Message text
} message_t;

int main() {
    mqd_t mq;  // Message queue descriptor
    message_t msg;

    // Open message queue
    mq = mq_open(QUEUE_NAME, O_CREAT | O_RDWR, 0666, NULL);
    if (mq == -1) {
        perror("Server: mq_open failed");
        exit(1);
    }

    printf("Server started, waiting for messages...\n");

    while (1) {
        // Receive messages from clients
        if (mq_receive(mq, (char *)&msg, MAX_MSG_SIZE, NULL) == -1) {
            perror("Server: mq_receive failed");
            continue;
        }

        // Display received message
        printf("Server received message from client (type %ld): %s\n", msg.mtype, msg.mtext);

        // Respond to the client based on the message type (client ID)
        snprintf(msg.mtext, MAX_MSG_SIZE, "Hello client %ld, your message was received.", msg.mtype);

        if (mq_send(mq, (const char *)&msg, sizeof(msg), msg.mtype) == -1) {
            perror("Server: mq_send failed");
            continue;
        }

        printf("Server sent response to client %ld\n", msg.mtype);
    }

    // Clean up message queue
    mq_close(mq);
    mq_unlink(QUEUE_NAME);
    return 0;
}
```

### **Client Program (`client.c`)**:

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <mqueue.h>
#include <errno.h>

#define QUEUE_NAME "/my_message_queue"  // Message queue name
#define MAX_MSG_SIZE 256
#define MAX_MSG_COUNT 10

// Structure for message
typedef struct {
    long mtype;         // Message type (for identifying the client)
    char mtext[MAX_MSG_SIZE]; // Message text
} message_t;

int main() {
    mqd_t mq;  // Message queue descriptor
    message_t msg;
    char buffer[MAX_MSG_SIZE];
    int client_id;

    // Get the client ID
    printf("Enter your client ID (1 or 2): ");
    scanf("%d", &client_id);

    // Open the message queue
    mq = mq_open(QUEUE_NAME, O_RDWR);
    if (mq == -1) {
        perror("Client: mq_open failed");
        exit(1);
    }

    // Send a message to the server
    msg.mtype = client_id;  // Set the client ID as message type
    snprintf(msg.mtext, MAX_MSG_SIZE, "Message from client %d", client_id);

    if (mq_send(mq, (const char *)&msg, sizeof(msg), msg.mtype) == -1) {
        perror("Client: mq_send failed");
        exit(1);
    }

    printf("Client %d sent message: %s\n", client_id, msg.mtext);

    // Receive response from the server
    if (mq_receive(mq, (char *)&msg, MAX_MSG_SIZE, NULL) == -1) {
        perror("Client: mq_receive failed");
        exit(1);
    }

    printf("Client %d received response: %s\n", client_id, msg.mtext);

    // Close the message queue
    mq_close(mq);
    return 0;
}
```

---

### **Explanation of the Code**:

#### **Server Program (`server.c`)**:
- **Message Queue Creation**: The server creates a message queue using `mq_open()`. It uses the `O_CREAT` flag to create the queue if it doesn't exist.
- **Receiving Messages**: The server continuously waits for messages from clients using `mq_receive()`. It processes the message and then responds back to the correct client based on the message type (`mtype`).
- **Responding to Clients**: After processing the message, the server constructs a response and sends it to the correct client using `mq_send()`, where the `mtype` corresponds to the client ID.

#### **Client Program (`client.c`)**:
- **Message Queue Connection**: The client connects to the same message queue using `mq_open()`.
- **Sending Message**: The client sends a message to the server with a specific `mtype` (client ID). This ensures the server can reply to the correct client.
- **Receiving Response**: After sending the message, the client waits for a response using `mq_receive()`.
- **Client Interaction**: Each client is identified by a unique `mtype`, which allows the server to send responses to the correct client.

---

### **Compilation**:
To compile the server and client programs:

```bash
gcc -o server server.c -lrt
gcc -o client client.c -lrt
```

The `-lrt` option links the real-time library, which is necessary for POSIX message queues.

---

### **Running the Programs**:
1. **Start the server**: In one terminal, run the server program:
   ```bash
   ./server
   ```

2. **Start the clients**: In two separate terminals, run the client programs:
   - **Client 1**:
     ```bash
     ./client
     ```
   - **Client 2**:
     ```bash
     ./client
     ```

Each client will send a message to the server, and the server will reply to each client individually based on their `mtype` (client ID).

---

### **Example Output**:

#### **Server Output**:
```
Server started, waiting for messages...
Server received message from client (type 1): Message from client 1
Server sent response to client 1
Server received message from client (type 2): Message from client 2
Server sent response to client 2
```

#### **Client 1 Output**:
```
Enter your client ID (1 or 2): 1
Client 1 sent message: Message from client 1
Client 1 received response: Hello client 1, your message was received.
```

#### **Client 2 Output**:
```
Enter your client ID (1 or 2): 2
Client 2 sent message: Message from client 2
Client 2 received response: Hello client 2, your message was received.
```

---

### **Summary**:
This program uses **POSIX message queues** to facilitate private communication between a server and multiple clients. 
The server can distinguish between clients using a message type (`mtype`), and each client sends and receives messages privately.
The server uses a single message queue to manage communication with all clients.

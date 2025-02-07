## Exercise 12758

### Implement a blocked or synchronous method of message passing using signal interrupts.

---

**Exercise 12758** involves implementing a blocked or synchronous method of message passing using signal interrupts. 
This requires using signal handling mechanisms in C to synchronize communication between processes. 
The goal is to simulate synchronous message passing where the sender is blocked (waits) until the receiver acknowledges the message or performs a certain action.

### **Concept**:
1. **Blocked or Synchronous Message Passing**: 
   - The sender sends a message and then waits until the receiver processes it.
   - Signal handling mechanisms can be used to notify the sender when the message has been received and processed.
   
2. **Signals and Interrupts**:
   - Signals are asynchronous notifications sent to processes or threads, allowing inter-process communication or event notification.
   - The sender will be blocked until it receives an acknowledgment signal from the receiver that the message has been processed.
   - The receiver will trigger a signal back to the sender to notify it that the message has been processed.

### **Steps**:
- **Sender**: 
  - Sends a message to the receiver.
  - Blocks (waits) for an acknowledgment signal (for example, `SIGUSR1`) to indicate the message has been processed.
  
- **Receiver**:
  - Receives the message from the sender (using a suitable inter-process communication method).
  - Processes the message.
  - Sends a signal (e.g., `SIGUSR1`) back to the sender to unblock it.

### **Code Example**: Using Signals for Synchronous Message Passing

#### **Sender Program (`sender.c`)**:
```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <signal.h>
#include <string.h>
#include <errno.h>

volatile sig_atomic_t ack_received = 0;  // Flag to track if ACK is received

// Signal handler for receiving acknowledgment
void sig_handler(int signum) {
    if (signum == SIGUSR1) {
        ack_received = 1;  // Set flag when ACK signal is received
    }
}

// Function to send a message and wait for acknowledgment
void send_message(pid_t receiver_pid, const char *message) {
    printf("Sender: Sending message: %s\n", message);
    // Send the message (simplified, actual IPC not implemented here)
    // In a real scenario, we would use a method like a shared memory or message queue

    // Wait for acknowledgment signal from receiver
    printf("Sender: Waiting for acknowledgment...\n");
    while (!ack_received) {
        sleep(1);  // Block until we get the acknowledgment signal
    }
    printf("Sender: Acknowledgment received. Message processed.\n");
}

int main() {
    pid_t receiver_pid;
    
    // Register the signal handler for SIGUSR1
    signal(SIGUSR1, sig_handler);

    // Get the receiver's process ID
    printf("Enter receiver process ID: ");
    scanf("%d", &receiver_pid);

    // Send a message to the receiver and wait for acknowledgment
    send_message(receiver_pid, "Hello, Receiver!");

    return 0;
}
```

#### **Receiver Program (`receiver.c`)**:
```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <signal.h>

// Function to process the received message (simulated)
void process_message(const char *message) {
    printf("Receiver: Processing message: %s\n", message);
    sleep(2);  // Simulate some processing time
}

// Function to send acknowledgment signal to the sender
void send_acknowledgment(pid_t sender_pid) {
    printf("Receiver: Sending acknowledgment to sender.\n");
    kill(sender_pid, SIGUSR1);  // Send SIGUSR1 to sender to acknowledge message
}

int main() {
    pid_t sender_pid;
    char message[256];

    // Get the sender's process ID
    printf("Enter sender process ID: ");
    scanf("%d", &sender_pid);

    // Simulate receiving a message (in a real system, we'd use IPC mechanisms)
    printf("Receiver: Waiting for message...\n");

    // For demonstration purposes, we'll just simulate receiving a message
    printf("Receiver: Received message from sender: Hello, Receiver!\n");

    // Process the message
    process_message("Hello, Receiver!");

    // Send acknowledgment signal to sender
    send_acknowledgment(sender_pid);

    return 0;
}
```

### **Explanation**:

#### **Sender Program (`sender.c`)**:
1. **Signal Handler**: The sender registers a signal handler (`sig_handler`) for `SIGUSR1` using `signal(SIGUSR1, sig_handler)`. When the sender receives this signal, it sets the `ack_received` flag to `1`.
   
2. **Send Message**: The `send_message` function simulates sending a message to the receiver and then waits for an acknowledgment signal (`SIGUSR1`). The sender is blocked in a `while` loop and keeps checking if it has received the acknowledgment signal by checking the `ack_received` flag.

3. **Blocking on Signal**: The sender is blocked until it receives the acknowledgment signal from the receiver, at which point the sender prints a message confirming that the message was processed.

#### **Receiver Program (`receiver.c`)**:
1. **Simulate Message Processing**: The receiver simulates receiving and processing a message by printing the received message and sleeping for 2 seconds to simulate message processing time.

2. **Send Acknowledgment**: After processing the message, the receiver sends an acknowledgment back to the sender using the `kill(sender_pid, SIGUSR1)` function. This sends the `SIGUSR1` signal to the sender, which unblocks it.

3. **Simulated Message**: For simplicity, the receiver program doesn't actually read a message via IPC (e.g., message queues or shared memory), but instead directly prints a predefined message to simulate message reception.

### **Signal Handling**:
- **Sender**: Sends a message, waits (blocks) for an acknowledgment signal (`SIGUSR1`), and then continues once the acknowledgment is received.
- **Receiver**: Receives a message, processes it, and sends an acknowledgment signal back to the sender.

### **Compilation**:
To compile both programs:

```bash
gcc -o sender sender.c
gcc -o receiver receiver.c
```

### **Running the Programs**:

1. **Start the Receiver**:
   ```bash
   ./receiver
   ```
   The receiver will prompt for the sender's process ID and will wait for a message.

2. **Start the Sender**:
   ```bash
   ./sender
   ```
   The sender will prompt for the receiver's process ID, send a message, and wait for the acknowledgment.

3. **Interaction**:
   - The sender sends a message and then waits for the receiver to acknowledge it by sending the `SIGUSR1` signal.
   - The receiver processes the message and sends the acknowledgment back to the sender.

### **Example Output**:

#### **Sender Output**:
```
Enter receiver process ID: 12345
Sender: Sending message: Hello, Receiver!
Sender: Waiting for acknowledgment...
Sender: Acknowledgment received. Message processed.
```

#### **Receiver Output**:
```
Enter sender process ID: 12344
Receiver: Waiting for message...
Receiver: Received message from sender: Hello, Receiver!
Receiver: Processing message: Hello, Receiver!
Receiver: Sending acknowledgment to sender.
```

---

### **Summary**:
This implementation uses signal handling (`SIGUSR1`) to create a synchronous communication mechanism between the sender and receiver. 
The sender sends a message and blocks until it receives an acknowledgment signal from the receiver, which processes the message and sends the acknowledgment back.
This approach ensures that the sender and receiver are synchronized in their message passing using signal interrupts.

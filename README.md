# 🔐 Encrypted File Transfer Application in Java

A secure client-server file transfer application built using **Java Socket Programming** and **AES-GCM authenticated encryption**.

The application encrypts a file on the sender side, transfers the encrypted data over a TCP socket, and decrypts it on the receiver side before saving the original file.

## 🚀 Features

* 🔐 AES-GCM authenticated encryption
* 🎲 Random 12-byte IV generated for every encryption
* 🌐 TCP client-server communication using Java Sockets
* 📁 File transfer between sender and receiver
* 🔓 Automatic decryption on the receiver side
* 🛡️ Authentication tag helps detect modified encrypted data
* 📏 Maximum file-size validation on the server
* ⚠️ Error handling for missing files, connection failures, invalid data, and authentication failures
* 💻 Simple command-line interface
## 🏗️ Architecture photo

![System Architecture](docs/architecture.png)
## 🏗️ Architecture 

```text
                    SECURE FILE TRANSFER

 ┌─────────────────┐
 │     SENDER      │
 │   Client.java   │
 └────────┬────────┘
          │
          │ 1. Read file
          ▼
 ┌─────────────────┐
 │    AES-GCM      │
 │   Encryption    │
 └────────┬────────┘
          │
          │ 2. Encrypted data + IV
          ▼
═════════════════════════════════════
          TCP Socket / Port 5000
═════════════════════════════════════
          │
          ▼
 ┌─────────────────┐
 │    RECEIVER     │
 │   Server.java   │
 └────────┬────────┘
          │
          │ 3. Receive encrypted data
          ▼
 ┌─────────────────┐
 │    AES-GCM      │
 │   Decryption    │
 └────────┬────────┘
          │
          │ 4. Original file
          ▼
 ┌─────────────────────┐
 │ received_file.txt   │
 └─────────────────────┘
```

## 🔄 How It Works

### Sender

1. `Client.java` locates `send.txt`.
2. The file is read into memory.
3. `AESUtil.java` encrypts the file using AES-GCM.
4. A fresh random IV is generated for the encryption operation.
5. The encrypted data and IV are sent to the server through a TCP socket.

### Receiver

1. `Server.java` listens on port `5000`.
2. The server accepts the client connection.
3. It receives the encrypted data.
4. `AESUtil.java` extracts the IV and decrypts the data.
5. AES-GCM authentication verifies the encrypted data.
6. The decrypted content is saved as `received_file.txt`.

## 🔐 Security

This project uses:

**AES-GCM (Advanced Encryption Standard - Galois/Counter Mode)**

AES-GCM provides:

* **Confidentiality** — the file contents are encrypted.
* **Integrity and authentication** — unauthorized modifications to encrypted data can cause decryption to fail.

A fresh random **12-byte IV** is generated for every encryption operation.

### ⚠️ Key Management

For demonstration purposes, the project currently uses a shared AES key defined in `AESUtil.java`.

This demonstrates the encryption workflow, but a production application should **not hard-code encryption keys**.

Possible production improvements include:

* Secure environment variables or secret management
* Password-based key derivation
* RSA/ECDH-based key exchange
* Secure key storage
* User authentication

## 🛠️ Technologies Used

* **Java**
* **Java Socket Programming**
* **TCP/IP**
* **AES-GCM**
* **Java Cryptography Architecture (JCA)**
* **File I/O**
* **Command Line**

## 📂 Project Structure

```text
Encrypted-File-Transfer-Application-in-Java/
│
├── src/
│   ├── AESUtil.java
│   ├── Client.java
│   └── Server.java
│
├── docs/
│   └── Secure_File_Transfer_Report.pdf
│
├── send.txt
├── README.md
└── .gitignore
```

## ▶️ How to Run

### 1. Open the Project Directory

Open Command Prompt in the project root directory.

### 2. Compile the Java Files

Run:

```cmd
javac src\AESUtil.java src\Client.java src\Server.java
```

### 3. Start the Server

Open the first terminal and run:

```cmd
java -cp src Server
```

Expected output:

```text
========================================
     SECURE FILE TRANSFER - SERVER
========================================
[+] Server started.
[+] Listening on port 5000
[+] Waiting for client...
```

Keep the server terminal running.

### 4. Start the Client

Open a second terminal in the project root directory and run:

```cmd
java -cp src Client
```

Expected output:

```text
========================================
     SECURE FILE TRANSFER - CLIENT
========================================
[+] File: send.txt
[+] File read successfully.
[+] Encrypting file...
[+] Encryption completed.
[+] Connected to server.
[+] Encrypted file sent successfully.
========================================
       TRANSFER COMPLETED ✓
========================================
```

### 5. Verify the Transfer

After successful transfer, the server creates:

```text
received_file.txt
```

Compare the original and received files using Windows CMD:

```cmd
fc send.txt received_file.txt
```

Successful verification:

```text
FC: no differences encountered
```

## 🧪 Test Result

The application was successfully tested using a sample `send.txt` file.

The test demonstrated the complete secure transfer pipeline:

```text
Original File
     ↓
AES-GCM Encryption
     ↓
Encrypted Data + IV
     ↓
TCP Socket Transfer
     ↓
AES-GCM Decryption
     ↓
Received File
```

The original and decrypted files were successfully compared using:

```text
FC: no differences encountered
```

The test file was **39 bytes**, while the encrypted payload was **67 bytes**, demonstrating the additional IV and authentication-tag overhead introduced by AES-GCM.

## 📌 Current Limitations

This is an educational/demo implementation.

* The AES key is currently shared between the client and server.
* The encryption key is stored in the source code for demonstration.
* Files are currently loaded into memory rather than streamed in chunks.
* The application currently supports one transfer per server process.
* There is no user authentication.
* Communication is currently implemented over a standard TCP socket without TLS.

## 🔮 Future Enhancements

* 🔑 Secure key exchange using RSA or ECDH
* 👤 User authentication
* 📦 Large-file streaming
* 📊 Transfer progress indicator
* 🖥️ JavaFX graphical interface
* 👥 Multiple simultaneous clients
* 📜 Transfer history and logging
* 🔒 Secure key storage
* 🛡️ TLS-secured communication
* ☁️ Cloud-based deployment

## 🎓 Learning Outcomes

This project demonstrates practical knowledge of:

* Network programming
* Client-server architecture
* TCP sockets
* Symmetric encryption
* Authenticated encryption
* Java Cryptography Architecture
* File handling
* Exception handling
* Secure data transmission

## 👩‍💻 Author

**Raheena**

B.Tech — Computer Science & Engineering

GitHub: **P188-raheena**

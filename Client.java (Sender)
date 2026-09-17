import java.io.*;
import java.net.*;

public class Client {

    private static final String HOST = "localhost";
    private static final int PORT = 5000;
    private static final String FILE_NAME = "send.txt";

    public static void main(String[] args) {

        File file = new File(FILE_NAME);

        if (!file.exists()) {
            System.out.println("[ERROR] File not found: " + FILE_NAME);
            return;
        }

        if (!file.isFile()) {
            System.out.println("[ERROR] The selected path is not a file.");
            return;
        }

        try {
            System.out.println("========================================");
            System.out.println("     SECURE FILE TRANSFER - CLIENT");
            System.out.println("========================================");

            System.out.println("[+] File: " + file.getName());
            System.out.println("[+] File size: " + file.length() + " bytes");

            // Read file
            byte[] fileData;

            try (FileInputStream fis = new FileInputStream(file);
                 ByteArrayOutputStream buffer = new ByteArrayOutputStream()) {

                byte[] temp = new byte[8192];
                int bytesRead;

                while ((bytesRead = fis.read(temp)) != -1) {
                    buffer.write(temp, 0, bytesRead);
                }

                fileData = buffer.toByteArray();
            }

            System.out.println("[+] File read successfully.");

            // Encrypt
            System.out.println("[+] Encrypting file...");

            byte[] encryptedData = AESUtil.encrypt(fileData);

            System.out.println("[+] Encryption completed.");
            System.out.println("[+] Encrypted size: "
                    + encryptedData.length + " bytes");

            // Connect to server
            System.out.println("[+] Connecting to server...");

            try (Socket socket = new Socket(HOST, PORT);
                 DataOutputStream dos =
                         new DataOutputStream(socket.getOutputStream())) {

                System.out.println("[+] Connected to server.");

                // Send encrypted data size
                dos.writeInt(encryptedData.length);

                // Send encrypted data
                dos.write(encryptedData);
                dos.flush();

                System.out.println("[+] Encrypted file sent successfully.");
            }

            System.out.println("========================================");
            System.out.println("       TRANSFER COMPLETED ✓");
            System.out.println("========================================");

        } catch (FileNotFoundException e) {

            System.out.println("[ERROR] File could not be opened.");

        } catch (ConnectException e) {

            System.out.println("[ERROR] Could not connect to server.");
            System.out.println("[INFO] Start Server.java first.");

        } catch (IOException e) {

            System.out.println("[ERROR] Network/File error: "
                    + e.getMessage());

        } catch (Exception e) {

            System.out.println("[ERROR] Encryption error: "
                    + e.getMessage());
        }
    }
}

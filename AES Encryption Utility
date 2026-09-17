import javax.crypto.Cipher;
import javax.crypto.spec.GCMParameterSpec;
import javax.crypto.spec.SecretKeySpec;
import java.security.SecureRandom;
import java.util.Arrays;

public class AESUtil {

    private static final String ALGORITHM = "AES/GCM/NoPadding";

    // Demo key for this project.
    // In a real application, this must NOT be hard-coded.
    private static final byte[] KEY_BYTES = {
            0x01, 0x23, 0x45, 0x67,
            0x11, 0x33, 0x55, 0x77,
            0x21, 0x43, 0x65, 0x07,
            0x31, 0x53, 0x75, 0x17
    };

    private static final int IV_LENGTH = 12;
    private static final int TAG_LENGTH = 128;

    private static final SecretKeySpec SECRET_KEY =
            new SecretKeySpec(KEY_BYTES, "AES");

    public static byte[] encrypt(byte[] data) throws Exception {

        // Generate a fresh random IV for every encryption
        byte[] iv = new byte[IV_LENGTH];
        SecureRandom secureRandom = new SecureRandom();
        secureRandom.nextBytes(iv);

        Cipher cipher = Cipher.getInstance(ALGORITHM);

        GCMParameterSpec gcmSpec =
                new GCMParameterSpec(TAG_LENGTH, iv);

        cipher.init(Cipher.ENCRYPT_MODE, SECRET_KEY, gcmSpec);

        byte[] encryptedData = cipher.doFinal(data);

        // Send IV together with encrypted data
        byte[] result = new byte[IV_LENGTH + encryptedData.length];

        System.arraycopy(iv, 0, result, 0, IV_LENGTH);
        System.arraycopy(
                encryptedData,
                0,
                result,
                IV_LENGTH,
                encryptedData.length
        );

        return result;
    }

    public static byte[] decrypt(byte[] encryptedData) throws Exception {

        if (encryptedData.length <= IV_LENGTH) {
            throw new IllegalArgumentException(
                    "Invalid encrypted data."
            );
        }

        // Extract IV
        byte[] iv = Arrays.copyOfRange(
                encryptedData,
                0,
                IV_LENGTH
        );

        // Extract encrypted content + authentication tag
        byte[] cipherText = Arrays.copyOfRange(
                encryptedData,
                IV_LENGTH,
                encryptedData.length
        );

        Cipher cipher = Cipher.getInstance(ALGORITHM);

        GCMParameterSpec gcmSpec =
                new GCMParameterSpec(TAG_LENGTH, iv);

        cipher.init(
                Cipher.DECRYPT_MODE,
                SECRET_KEY,
                gcmSpec
        );

        return cipher.doFinal(cipherText);
    }
}

# Aster DEX Automated Trading Bot

This repository contains an automated trading bot designed for perpetual futures trading on [Aster DEX](https://aster.pro/). The bot is written in Java and is designed for 24/7 operation on a Virtual Private Server (VPS).

Happy earning and farming!

## VPS Server Requirements

To run the bot continuously and efficiently, a reliable VPS is recommended. The following specifications outline the minimum and recommended hardware configurations.

| Feature          | Minimum (Sufficient)        | Recommended (For Future Growth) |
| ---------------- | --------------------------- | ------------------------------- |
| **CPU**          | 1-2 vCores                  | 2-4 vCores                      |
| **RAM**          | 2 GB                        | 4 GB                            |
| **Storage**      | 20-30 GB SSD                | 40-50 GB NVMe SSD               |
| **Operating System** | Linux (Ubuntu or Debian)    | Linux (Ubuntu or Debian)        |
| **Uptime**       | 99.9% Guarantee             | 99.99% Guarantee                |
| **Connection**   | 1 Gbps Port                 | 1 Gbps Port                     |

## Setup and Configuration

Follow these steps to set up and deploy your trading bot.

### 1. Generate API Keys

First, you need to obtain API credentials from Aster DEX.

1.  Navigate to the [Aster DEX website](https://aster.pro/).
2.  Click on **More** in the navigation menu, then select **API Management**.
3.  Click the **Create API** button.
4.  Securely save the generated **API Key** and **API Secret**. You will need them for the bot's configuration file.
<img width="956" height="328" alt="image" src="https://github.com/user-attachments/assets/7a2c5c3a-20fd-4b01-91bd-c19003f2befa" />

1.  copy and past i give code ai and tell your stargy ang say ive me full code
```bash
import javax.crypto.Mac;
import javax.crypto.spec.SecretKeySpec;
import java.net.*;
import java.io.*;
import java.nio.charset.StandardCharsets;

/**
 * A universal, empty template for a crypto trading bot.
 * All strategy-specific and personal values are replaced with placeholders ("XX").
 * The user must fill in all placeholders before the bot can function.
 */
public class UniversalTradingBotTemplate {

    // --- 1. User Must Define All Trading Parameters ---
    private static final String SYMBOL = "XX_TRADING_PAIR_XX"; // e.g., "BTCUSDT"
    private static final double TRADE_SIZE = 0.0;             // e.g., 0.01
    private static final int LEVERAGE = 0;                     // e.g., 20

    // --- 2. User Must Provide API Credentials ---
    private static final String API_KEY = "XX_YOUR_API_KEY_XX";
    private static final String API_SECRET = "XX_YOUR_API_SECRET_XX";
    
    // --- 3. User Must Provide Exchange API URL ---
    private static final String BASE_URL = "XX_EXCHANGE_API_URL_XX"; // e.g., "https://fapi.binance.com"

    /**
     * Places a market order on the exchange.
     * This function will not work until all placeholder values are filled in.
     * @param side The side of the order ("BUY" for long, "SELL" for short).
     */
    public static void placeOrder(String side) throws Exception {
        if (!side.equals("BUY") && !side.equals("SELL")) {
            throw new IllegalArgumentException("Order side must be 'BUY' or 'SELL'");
        }

        String endpoint = "/fapi/v1/order"; // This might need to be adjusted based on the exchange
        String params = "symbol=" + SYMBOL +
                        "&side=" + side +
                        "&type=MARKET" +
                        "&quantity=" + TRADE_SIZE +
                        "&leverage=" + LEVERAGE +
                        "&timestamp=" + System.currentTimeMillis();

        String signature = hmacSHA256(params, API_SECRET);
        String fullQuery = params + "&signature=" + signature;

        URL url = new URL(BASE_URL + endpoint);
        HttpURLConnection conn = (HttpURLConnection) url.openConnection();
        conn.setRequestMethod("POST");
        conn.setRequestProperty("X-MBX-APIKEY", API_KEY);
        conn.setRequestProperty("Content-Type", "application/x-www-form-urlencoded");
        conn.setDoOutput(true);

        try (OutputStream os = conn.getOutputStream()) {
            os.write(fullQuery.getBytes(StandardCharsets.UTF_8));
        }

        int responseCode = conn.getResponseCode();
        if (responseCode == 200) {
            System.out.println("SUCCESS: Order placed -> " + side);
        } else {
            System.out.println("ERROR: Order failed with code " + responseCode);
            try (BufferedReader br = new BufferedReader(new InputStreamReader(conn.getErrorStream()))) {
                System.out.println("Error Response: " + br.readLine());
            }
        }
    }

    /**
     * Generates an HMAC-SHA256 signature for authenticating API requests.
     */
    public static String hmacSHA256(String data, String key) throws Exception {
        Mac sha256_HMAC = Mac.getInstance("HmacSHA256");
        SecretKeySpec secret_key = new SecretKeySpec(key.getBytes(StandardCharsets.UTF_8), "HmacSHA256");
        sha256_HMAC.init(secret_key);
        
        byte[] raw = sha256_HMAC.doFinal(data.getBytes(StandardCharsets.UTF_8));
        StringBuilder hex = new StringBuilder(2 * raw.length);
        for (byte b : raw) {
            hex.append(String.format("%02x", b));
        }
        return hex.toString();
    }

    /**
     * The main entry point for the bot.
     * This area is intentionally left blank for the user to implement their strategy.
     */
    public static void main(String[] args) {
        System.out.println("--- Universal Trading Bot Template ---");
        System.out.println("This bot is not functional until you fill in all 'XX' placeholder values.");
        System.out.println("You must also add your trading logic inside the main() method.");
        
        // --- YOUR STRATEGY LOGIC GOES HERE ---
        //
        // Example:
        //
        // try {
        //     // placeOrder("BUY");
        // } catch (Exception e) {
        //     e.printStackTrace();
        // }
        //
        // --- END OF STRATEGY AREA ---
        
        System.out.println("Bot execution finished.");
    }
}
```
3.  Create a Java file (e.g., `TradingBot.java`) inside the folder.   file name accoding to you i give just examle 
4.  Paste your trading bot's Java code into this file. Ensure your trading strategy, API keys, and other configurations are correctly set within the code.
5.  Compile the code to ensure there are no errors. You can ask an AI assistant for the correct compilation and run commands if needed. 

```bash
javac YourBotFileName.java
```

5.  Test the bot locally to confirm that it functions as expected.

### 3. VPS Deployment

These steps will guide you through setting up a Linux VPS to run the bot 24/7.

1.  **Connect to Your VPS**
    Use an SSH client like Termius or the built-in terminal to connect to your server.

2.  **Update Server Packages**
    Keep your system up-to-date.
    ```bash
    sudo apt update
    ```

3.  **Install Java**
    The bot requires a Java Development Kit (JDK) to run.
    ```bash
    sudo apt install default-jdk -y
    ```

4.  **Create a Project Folder**
    Create a dedicated directory for your bot in your home directory.
    ```bash
    mkdir ~/trading_bot
    ```

5.  **Navigate to the Folder**
    Change into the newly created directory.
    ```bash
    cd ~/trading_bot
    ```

6.  **Upload Your Bot File**
    Transfer your compiled `.java` or `.class` file(s) from your local machine to the `~/trading_bot` directory on your VPS. You can use SFTP tools (available in clients like Termius, FileZilla, or Cyberduck) for this.

### 4. Running the Bot 24/7

To ensure the bot runs continuously even after you disconnect from the VPS, we will use the `screen` utility.

1.  **Install Screen**
    If `screen` is not already installed, use this command:
    ```bash
    sudo apt install screen -y
    ```

2.  **Compile the Java Code on the VPS**
    Navigate to your project directory and compile the Java file. Replace `YourBotFileName.java` with the actual name of your file.
    ```bash
    cd ~/trading_bot
    javac YourBotFileName.java
    ```

3.  **Create a New Screen Session**
    Start a named `screen` session. You can name it anything you like.
    ```bash
    screen -S tradingbot
    ```

4.  **Run the Bot**
    Inside the `screen` session, execute your compiled bot. Replace `YourBotFileName` with the name of your main class file (without the `.java` or `.class` extension).
    ```bash
    java YourBotFileName
    ```
    Your bot is now running inside the `screen` session.

5.  **Detach from the Session**
    To let the bot run in the background, detach from the session by pressing the following key combination:
    **`Ctrl+A`**, followed by **`D`**.

### How to Check on Your Bot

You can reconnect to the `screen` session at any time to check the bot's logs or status.

1.  Connect to your VPS via SSH.
2.  List all running screen sessions:
   
    screen -ls
   
3.  Re-attach to your named session:
  
    screen -r tradingbot
   
<img width="960" height="510" alt="image" src="https://github.com/user-attachments/assets/126be215-7811-44bf-beea-6b1b56384a0f" />


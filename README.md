# Port-forwarding
Forward network traffic on locally running web server make accessible on current network

### What is socat?

[Socat](https://formulae.brew.sh/formula/socat) (Socket CAT) is a versatile networking tool for data transfer between two points, allowing you to forward traffic between ports, devices, or addresses.

1. Install socat on macOS
   ```
   brew install socat
   ```

2. Identify Your Local Server Port
   ```
   curl http://127.0.0.1:8000
   ```

3. Set Up Port Forwarding with socat
   ```
   socat TCP-LISTEN:8000,reuseaddr,fork TCP:127.0.0.1:8000
   ```
   You can use socat to forward traffic from your Mac's IP (e.g., 192.168.1.100) to the local server on 127.0.0.1:8000.

   * Explanation:
      1. TCP-LISTEN:8000: Listens on all interfaces (0.0.0.0) for incoming connections on port 8000.
      2. reuseaddr: Allows the port to be reused.
      3. fork: Handles multiple simultaneous connections.
      4. TCP:127.0.0.1:8000: Forwards traffic to the Axum server running locally.
      5. Now, your Axum server should be accessible from other devices at (e.g. ,http://192.168.1.100:8000).

4. Verify Access
From another device on the same Wi-Fi network:
   ```
   curl http://192.168.1.100:8000
   ```
* Persistent Setup (Optional):
   1. Create a script
      ```
      #!/bin/bash
      socat TCP-LISTEN:8000,reuseaddr,fork TCP:127.0.0.1:8000
      ```
   2. Make it executable:
      ```
      chmod +x socat_forward.sh
      ```
   3. Run it in the background:
      ```
      ./socat_forward.sh &
      ```
* Use socat for External Access
   If you want to expose the server to the internet:
   ```
   ngrok http 8000
   ```
References

1. [Websocat](https://formulae.brew.sh/formula/websocat) is a command-line utility similar to socat, but it is specifically designed for working with WebSockets.
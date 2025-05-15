
# Dockerized SOCKS5 Proxy

A lightweight and secure SOCKS5 proxy server using Docker. This setup leverages environment variables to configure the username and password.

---

## Features
- Secure SOCKS5 proxy with user authentication.
- Easy configuration through environment variables.
- Minimal footprint using the `serjs/go-socks5-proxy` image.

---

## Getting Started

### Prerequisites
- Docker and Docker Compose installed on your system.

### Installation

1. **Clone the Repository:**
   ```bash
   git clone https://github.com/blackrosezy/go-socks5.git
   cd go-socks5
   ```

2. **Create a `.env` file:**

   Create a file named `.env` in the project root and add the following lines:
   ```
   SOCKS5_USER=yourusername
   SOCKS5_PASSWORD=yourpassword
   ```

---

## Docker Compose Configuration

The `docker-compose.yml` file is configured as follows:

```yaml
version: '3.8'
services:
  socks5:
    image: serjs/go-socks5-proxy
    container_name: socks5
    ports:
      - "1090:9090"
    environment:
      - PROXY_PORT=9090
      - PROXY_USER=${SOCKS5_USER}
      - PROXY_PASSWORD=${SOCKS5_PASSWORD}
```

---

## Running the Proxy Server

### Start the Proxy
To start the SOCKS5 proxy server, run:
```bash
docker-compose up -d
```

### Check the Running Container
To verify the server is running:
```bash
docker ps
```

### Stop the Proxy
To stop the SOCKS5 proxy server:
```bash
docker-compose down
```

---

## Usage

### Connecting to the Proxy
To connect to the SOCKS5 proxy, use the following credentials:
- **Server:** `localhost` or your server IP
- **Port:** `1090`
- **Username:** The value set in the `.env` file (`SOCKS5_USER`)
- **Password:** The value set in the `.env` file (`SOCKS5_PASSWORD`)

### Example: Using cURL with SOCKS5
You can test the proxy using `cURL`:
```bash
curl -x socks5://yourusername:yourpassword@localhost:1090 http://example.com
```

### Example: Using Proxychains
If you use Proxychains, add the following line to your `proxychains.conf`:
```
socks5 127.0.0.1 1090 yourusername yourpassword
```
Then run:
```bash
proxychains curl http://example.com
```

---

## Environment Variables

| Variable       | Description                   | Default |
|---------------|-------------------------------|--------|
| SOCKS5_USER    | Username for SOCKS5 authentication | -      |
| SOCKS5_PASSWORD| Password for SOCKS5 authentication | -      |
| PROXY_PORT     | Port on which the proxy runs   | 9090   |

---

## Troubleshooting

### Log Output
To check the logs of the running container:
```bash
docker logs socks5
```

### Restarting the Proxy
If you update the environment variables, restart the container:
```bash
docker-compose down && docker-compose up -d
```

### Common Issues
- **Proxy Not Working:** Make sure the container is running and the correct port is exposed.
- **Authentication Failure:** Double-check your `.env` file and ensure the credentials are correctly set.

---

## Security Considerations
- Avoid hardcoding credentials directly in `docker-compose.yml`.
- Use a secure password and change it periodically.
- Restrict access to the exposed port (`1090`) using firewall rules.

---

## License
This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

---

## Contributions
Contributions are welcome! Please open an issue or submit a pull request.

---

## Author
Maintained by [Mohd Rozi](https://github.com/blackrosezy)

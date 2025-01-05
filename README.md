<h1 align="center">
Bitping ☄️📈
</h1>

<div align="center">

[![Static Badge](https://img.shields.io/badge/GitHub-blue?style=flat&logo=github)](https://github.com/AnikBeris/bitping)
[![Static Badge](https://img.shields.io/badge/License-purple?style=flat&logo=github)](https://github.com/AnikBeris/bitping/blob/main/LICENSE)
[![Docker Stars](https://img.shields.io/docker/stars/xterna/honeygain-pot?logo=docker&label=Docker%20Stars)](https://hub.docker.com/r/bitping/bitpingd)
[![GitHub Repo stars](https://img.shields.io/github/stars/XternA/honeygain-reward?style=flat&logo=github&label=Stars&color=orange)](https://github.com/AnikBeris/bitping)


If you like this project, don't forget to leave a star. ⭐

### Containerized Docker image for [Bitping](bit.ly/4jiu0UT) lucky pot 


>**Note:** This generated image does not come with any warranty. By using this image, you agree to this License Agreement in addition to the Bitping Terms and Conditions.

</div>




## Docker Deployment 🐋
### Compose
File: `compose.yml`
```yaml
version: '3.8'

services:
  bitpingd-interactive:
    image: bitping/bitpingd:latest
    container_name: bitpingd-interactive
    restart: unless-stopped
    volumes:
      - ./bitpingd-volume:/root/.bitpingd
    tty: true # For interactive mode

  bitpingd-cli:
    image: bitping/bitpingd:latest
    container_name: bitpingd-cli
    restart: "no" # CLI used for authentication only
    entrypoint: /app/bitpingd
    command: "login --email *********** --password ***********" # Provide MFA if required
    volumes:
      - ./bitpingd-volume:/root/.bitpingd
    stdin_open: true
    tty: true

  bitpingd-node:
    image: bitping/bitpingd:latest
    container_name: bitpingd-node
    restart: unless-stopped
    depends_on:
      - bitpingd-cli
    volumes:
      - ./bitpingd-volume:/root/.bitpingd

  bitpingd-env:
    image: bitping/bitpingd:latest
    container_name: bitpingd-env
    restart: unless-stopped
    environment:
      BITPING_EMAIL: ***********
      BITPING_PASSWORD: ***********
      BITPING_MFA: "" # Provide MFA if required
    volumes:
      - ./bitpingd-volume:/root/.bitpingd

volumes:
  bitpingd-volume:
    driver: local
```

Execute where compose file is located.
```yaml
docker compose up -d
```

### CLI
Using environment variable or Dotenv `.env` defined e.g.
```sh
docker run -it --mount type=volume,source="bitpingd-volume",target=/root/.bitpingd --entrypoint /app/bitpingd bitping/bitpingd:latest login --email "YOUR_BITPING_EMAIL" --password "YOUR_BITPING_PASSWORD"
```
Option 1. To run the container in interactive mode:
```sh
bash docker run -it --mount type=volume,source="bitpingd-volume",target=/root/.bitpingd bitping/bitpingd:latest
```

Option 2. To run the container and pass email and password via CLI instead of an interactive session run:
Log in to your account with the following command:
```sh
docker run -it --mount type=volume,source="bitpingd-volume",target=/root/.bitpingd --entrypoint /app/bitpingd bitping/bitpingd:latest login --email "YOUR_BITPING_EMAIL" --password "YOUR_BITPING_PASSWORD"
```
Now start the bitping node!
```sh
docker run -it --mount type=volume,source="bitpingd-volume",target=/root/.bitpingd bitping/bitpingd:latest
```

Option 3. Login with environment variables:
```sh
docker run -it \
  -e BITPING_EMAIL='YOUR_BITPING_EMAIL' \
  -e BITPING_PASSWORD='YOUR_BITPING_PASSWORD' \
  -e BITPING_MFA='YOUR_BITPING_2FA_CODE' \
  --mount type=volume,source="bitpingd-volume",target=/root/.bitpingd bitping/bitpingd:latest
```




## Like My Work? 👍
Donations are warmly welcomed no matter how small and thank you very much. 😌
- **Bitcoin (BTC)** - `1Dbwq9EP8YpF3SrLgag2EQwGASMSGLADbh`
- **Ethereum (ERC20)** - `0x22258ea591966e830199d27dea7c542f31ed5dc5`
- **Binance Smart Chain (BEP20)** - `0x22258ea591966e830199d27dea7c542f31ed5dc5`
- **Solana (SOL)** - `yYYXsiVTzsvfvsMnBxfxSZEWTGytjAViE2ojf3hbLeF`

## Disclaimer ⚠️
Use this image at your own risk and responsibility. By using this image, you agree to be automatically bound by the License Agreement associated with it.

The author does not provide any assurances, whether explicit or implicit, regarding the accuracy, completeness, or appropriateness of this image for specific purposes. The author shall not be held accountable for any damages, including but not limited to direct, indirect, incidental, consequential, or special damages, arising from the use or inability to use this image or its accompanying documentation, even if the possibility of such damages has been communicated.

By choosing to use this image, you acknowledge and assume all risks associated with its use. Additionally, you agree that the author cannot be held liable for any issues or consequences that may arise as a result of its usage.

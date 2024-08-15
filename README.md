# Setting Up Oracle Database 19c Enterprise Edition for ARM (M1/M2 Macs)

Source: <https://itnext.io/oracle-on-arm-mac-m1-m2-docker-images-99ed67ed6ba6>

Tested with [OrbStack](https://orbstack.dev/) on an M1 Pro MacBook Pro.

1. Clone the official Oracle Docker images repository:

   ```bash
   git clone https://github.com/oracle/docker-images
   ```

1. Download the Oracle Database 19c aarch64 installer

   Download the installer from [Oracle Database 19c](https://www.oracle.com/database/technologies/oracle19c-linux-arm64-downloads.html) and put them into the dockerfiles/19.3.0 folder. The needed file is named _LINUX.ARM64_1919000_db_home.zip_.

1. Build the 19c Enterprise Edition container

   ```bash
   cd OracleDatabase/SingleInstance/dockerfiles/
   ./buildContainerImage.sh -v 19.3.0 -e
   ```

   At the end of this script, you should see the image in your Docker images: `oracle/database:19.3.0-ee`

1. Check out [this fork](https://github.com/marcelo-ochoa/oci-oracle-free/tree/19c-arm-slim) for the slim fast container

   ```bash
   git clone -b 19c-arm-slim https://github.com/marcelo-ochoa/oci-oracle-free
   cd oci-oracle-free
   ```

   *__Note:__ Make sure to checkout the `19c-arm-slim` branch.*

1. Build the slim version

   ```bash
   docker buildx build --build-arg BUILD_MODE=SLIM -t oracle/database:19.3.0-ee-slim -f Dockerfile.193 .
   ```

   At the end, you should see this image: `oracle/database:19.3.0-ee-slim`

1. Finally build slim-faststart version

   ```bash
   docker buildx build --build-arg BASE_IMAGE=oracle/database:19.3.0-ee-slim -t oracle/database:19.3.0-ee-slim-faststart -f Dockerfile.faststart .
   ```

   At the end, you should see this image: `oracle/database:19.3.0-ee-slim-faststart`

1. Use the `oracle/database:19.3.0-ee-slim-faststart` image in your Docker Compose
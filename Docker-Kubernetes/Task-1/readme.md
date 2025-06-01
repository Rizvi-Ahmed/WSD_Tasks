The docker compose file contains the necessary configuration to persist logs and configurations between restarts. It is also connected to a custom bridge network.

To spin up up nginx server container the following command is 

```bash
docker compose up -d 
```
docker version - 28.0.1
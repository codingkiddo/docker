
version: '3'
services:
  database:
    image: postgres:latest
    ports:
      - "5432:5432"
    environment:
      POSTGRES_USER: "postgres"
      POSTGRES_PASSWORD: "postgres"
      POSTGRES_DB:       "ostock_dev"
    volumes:
        - ./init.sql:/docker-entrypoint-initdb.d/1-init.sql
        - ./data.sql:/docker-entrypoint-initdb.d/2-data.sql
    networks:
      backend:
        aliases:
          - "database"
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U postgres"]
      interval: 10s
      timeout: 5s
      retries: 5

networks:
  backend:
    driver: bridge
	
	
	
docker run -d -p 8200:8200 --name vault -e 'VAULT_DEV_ROOT_TOKEN_ID=myroot' -e 'VAULT_DEV_LISTEN_ADDRESS=0.0.0.0:8200' vault
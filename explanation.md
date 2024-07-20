# Docker Setup and Deployment Documentation
## 1. Choice of Base Images:
### Backend and Client Containers:
Base Image: node:16-alpine
Reasoning: 
Lightweight: Alpine Linux is known for its small size and simplicity, which helps reduce the overall size of the Docker image.
Security: Alpine Linux has a smaller attack surface due to its minimalistic nature, which enhances security.
Performance: Using a smaller base image can lead to faster download times and improved performance when deploying the image.
Node.js Version: Node.js 16 was chosen because it is a Long-Term Support (LTS) version, providing stability and support for production environments.

## 2. Dockerfile Directives:
### Backend Container:
Directives Used: FROM, WORKDIR, COPY, RUN, EXPOSE, CMD

Use Node.js 16 on Alpine Linux for the build stage.<br />
 ```FROM node:16-alpine AS builder```

Set the working directory to /app.<br />
 ```WORKDIR /app```

Copy package files to the container.<br />
 ```COPY package*.json ./```

Install production dependencies.<br />
 ```RUN npm install --production```

Copy the rest of the application code.<br />
 ```COPY . .```

Start a new stage with Node.js 16 on Alpine Linux.<br />
 ```FROM node:16-alpine```

Set the working directory for the final image.<br />
 ```WORKDIR /app```

Copy built files from the build stage.<br />
 ```COPY --from=builder /app ./```

Document that the container uses port 5000.<br />
 ```EXPOSE 5000```

Define the command to run the application.<br />
 ```CMD ["npm", "start"]```

### Client Container:
Directives Used: FROM, WORKDIR, COPY, RUN, EXPOSE, CMD


PUse Node.js 16 on Alpine Linux for the build stage.<br />
 ```FROM node:16-alpine AS builder```

Set the working directory to /app.<br />
 ```WORKDIR /app```

Copy package files to the container.<br />
 ```COPY package*.json ./```

Install production dependencies.<br />
 ```RUN npm install --production```

Copy the rest of the application code.<br />
 ```COPY . .```

Start a new stage with Node.js 16 on Alpine Linux.<br />
 ```FROM node:16-alpine```

Set the working directory for the final image.<br />
 ```WORKDIR /app```

Copy built files from the build stage.<br />
 ```COPY --from=builder /app ./```

Document that the container uses port 5000.<br />
 ```EXPOSE 5000```

Define the command to run the application.<br />
 ```CMD ["npm", "start"]```
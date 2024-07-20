## 1. Choice of the base image on which to build each container
Base Image: **node:16-alpine** used to build both client and backend images
Reasoning: 
**Lightweight**: Alpine Linux is known for its small size and simplicity, which helps reduce the overall size of the docker image.
**Security**: Alpine Linux has a smaller attack surface due to its minimalistic nature, which enhances security.
**Performance**: Using a smaller base image can lead to faster download times and improved performance when deploying the image.
**Node.js Version**: Node.js 16 was chosen because it is a Long-Term Support (LTS) version, providing stability and support for production environments.

## 2. Dockerfile directives used in the creation and running of each container:
### Backend Dockerfile:
Directives used: FROM, WORKDIR, COPY, RUN, EXPOSE, CMD


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

 Multi-stage build process helps keep the final docker image lean by separating the build environment from the runtime environment.

### Client Dockerfile:
Directives used: FROM, WORKDIR, COPY, RUN, EXPOSE, CMD


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

Document that the container uses port 3000.<br />
 ```EXPOSE 3000```

Define the command to run the application.<br />
 ```CMD ["npm", "start"]```

 Multi-stage build process helps keep the final docker image lean by separating the build environment from the runtime environment.
![Docker Logo](/images/docker/docker-logo.png)

# How to Build a Dockerfile

## Premade base image from DockerHub vs Building your own image from scratch&colon;

### Pros&colon;

1. **Time Efficiency**: Building an image from scratch can be time-consuming, especially if it requires installing and configuring multiple dependencies. Using a premade base image saves time by providing a preconfigured environment.

2. **Consistency**: Base images are typically maintained by trusted sources, ensuring consistency across different environments. This reduces the risk of configuration errors and compatibility issues.

3. **Community Support**: Popular base images often have a large community of users who contribute to their maintenance and provide support. This can be valuable for troubleshooting and finding solutions to common problems.

4. **Security Updates**: Maintainers of base images regularly release updates to patch security vulnerabilities and improve performance. By using a premade base image, you benefit from these updates without having to manually apply them.

### Cons&colon;

1. **Lack of Customization**: Premade base images may not include all the dependencies or configurations required for your specific application. This can lead to additional work to customize the image to meet your needs.

2. **Image Size**: Base images can be relatively large, especially those based on popular operating systems like Ubuntu or Alpine. This can increase the size of your final Docker image, leading to longer download and deployment times.

3. **Dependency Risk**: Relying on third-party base images introduces a dependency on the maintainers to keep the image up-to-date and secure. If a base image becomes deprecated or is no longer maintained, it may require migration to a different base image.

4. **Potential for Bloat**: Base images often include tools and packages that may not be necessary for your specific application. This can result in unnecessary bloat and resource usage in your Docker containers.

Ultimately, the decision to use a premade base image versus building from scratch depends on factors such as your specific requirements, the availability of suitable base images, and the level of control and customization you require.

### Advantages of base image instead of from scratch&colon;

Base images typically provide a complete and configured environment for running applications within a Docker container. These environments often include essential components such as the operating system, runtime libraries, and pre-installed tools. Here are some features commonly found in base images that are not included in a `scratch` image:

1. **Operating System**: Base images are typically built on top of a lightweight Linux distribution, such as Alpine Linux or Ubuntu. This provides a familiar and stable environment for running applications. In contrast, a `scratch` image is completely empty and lacks any operating system components.

2. **Runtime Libraries**: Base images include essential runtime libraries required by most applications, such as libc (C library) and other system libraries. These libraries provide the necessary functionality for executing binaries and interacting with the operating system.

3. **Package Managers**: Base images often come with package managers like apt (Advanced Package Tool) for Debian-based images or apk (Alpine Package Keeper) for Alpine-based images. These package managers allow users to install additional software packages and dependencies as needed.

4. **Networking Configuration**: Base images typically include networking configurations that enable communication between containers and with the host system. This may include networking tools and configurations for managing network interfaces, IP addresses, and DNS resolution.

5. **Security Features**: Base images may include security features such as user isolation, file system permissions, and security patches. These features help protect the containerized environment from security vulnerabilities and attacks.

6. **Pre-installed Tools**: Base images often come with pre-installed tools and utilities commonly used in development and operations, such as shell interpreters (e.g., bash), text editors (e.g., vim), and debugging tools. These tools can be helpful for troubleshooting and debugging applications running inside containers.

7. **Default User**: Base images typically define a default user for running processes inside the container. This helps improve security by running processes with limited privileges. In contrast, a `scratch` image has no predefined user, and processes run as the root user by default.

Overall, base images provide a convenient starting point for building Docker containers by offering a preconfigured environment with essential components and tools. While a `scratch` image offers the ultimate minimalism, it requires users to manually provide all necessary components, making it less practical for most use cases.

## Building a Dockerfile&colon;

- Docker runs instructions in a Dockerfile in order. A Dockerfile must begin with a `FROM` instruction.

- On a project that does not already have a `Dockerfile`, `compose.yaml`(or .yml) file, and/or a `.dockerignore` file, you can run the command `docker init` to auto-build those files. This command will ask you if what template you want to build with (Node, Python, Java, etc.).

- If you run `docker init` on a project that already has a `Dockerfile`, `compose.yaml`(or .yml) file, and/or a `.dockerignore` file, your command terminal will ask if you want to overwrite these existing files. If you say yes, the old files will be deleted & replaced with the template files. **NOTE**: overwritten files cannot be recovered.

- Lines that start with `#` are treated as comments and anything in that line will not run.
Instruction Keywords:

- These are the main keywords used when building a Dockerfile. The only keywords that are truly required are `FROM`, `WORKDIR`, `COPY`, `RUN`, & `CMD`. Others are technically "optional", but you will need to use other keywords to follow best practices There are other optional keywords for certain use cases.

- `FROM <image>` - this specifies the base image that the build will use.

- `LABEL <key>=<value> <key>=<value> ...` - this adds metadata to the image. You can specify any kind of metadata, like the app author, description, title, etc.

- `WORKDIR <PATH` - this specifies the "working directory" or the path in the image where the files will be copied and commands will be executed.

- `COPY <host-path> <image-path>` - this tells the builder to copy files from the host and put them into the container image.

- `RUN <command>` - this tells the builder to run the specified command.

- `ENV <name> <values>` - this sets an environment variable that a running container will use.

- `EXPOSE <port-number>` - this sets configuration on the image that indicates a port the image would like to expose.

- `USER <user-or-uid>` - this sets the default user for all subsequent instructions.

- `CMD ["<command>", "<arg1>"]` - this sets the default command a container using this image will run.

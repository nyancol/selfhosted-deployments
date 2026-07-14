FROM nousresearch/hermes-agent:latest

USER root

# 1. Install system dependencies
RUN apt-get update && apt-get install -y --no-install-recommends \
    build-essential curl git procps file ca-certificates \
    && rm -rf /var/lib/apt/lists/*

# 2. Setup non-root user with specific UID/GID (1000 is standard for primary user)
# We use 'usermod' or 'useradd' only once. 
RUN groupadd -g 1000 hermes_user && \
    useradd -u 1000 -g hermes_user -m -s /bin/bash hermes_user

# Create necessary directories
RUN mkdir -p /home/linuxbrew/.linuxbrew && \
    chown -R hermes_user:hermes_user /opt/data /home/hermes_user /home/linuxbrew

# 3. Install Homebrew as the non-root user
USER hermes_user

RUN /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
ENV PATH="/home/linuxbrew/.linuxbrew/bin:${PATH}"

WORKDIR /home/hermes_user

# 4. Install Brew taps/tools
RUN brew install spotify_player
RUN brew tap openhue/cli && brew trust openhue/cli
RUN brew install openhue-cli

USER root

# Set working directory to the data path or user home
ENV HERMES_DATA_DIR=/opt/data


# ENTRYPOINT ["hermes", "gateway", "run"]

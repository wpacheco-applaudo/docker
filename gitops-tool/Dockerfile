# --- Stage 1: Download kubectl safely ---
FROM alpine:3.19 AS fetcher
RUN apk add --no-cache curl

# Fetch the latest stable kubectl binary
RUN curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl" \
    && chmod +x kubectl

# --- Stage 2: Final Light GitOps Container ---
FROM alpine:3.19

# Install git and openssh (needed to clone repos over SSH/HTTPS)
# --no-cache keeps the image size as small as possible
RUN apk add --no-cache git openssh-client bash

# Copy the pre-compiled kubectl binary from the first stage
COPY --from=fetcher /kubectl /usr/local/bin/kubectl

# Verify tools are working when the container starts
RUN git --version && kubectl version --client

# Set default working directory
WORKDIR /workspace

CMD ["/bin/bash"]

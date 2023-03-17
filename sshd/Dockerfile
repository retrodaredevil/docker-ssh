FROM alpine:latest

ARG USERNAME=ssh

RUN apk --no-cache update && apk --no-cache add openssh-server

# Add the user and unlock the account by giving it a password that is invalid, but does not start with "!".
#   (thanks https://unix.stackexchange.com/a/193131)
RUN adduser --shell /bin/false -D ${USERNAME} && \
    echo "${USERNAME}:*" | chpasswd -e
RUN mkdir -m 700 /home/${USERNAME}/.ssh/ && \
    chown ${USERNAME}:${USERNAME} /home/${USERNAME}/.ssh/

ENTRYPOINT ["/usr/sbin/sshd", "-D", "-e"]

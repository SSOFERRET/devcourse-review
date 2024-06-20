FROM jenkins/jnlp-agent-docker

USER root

COPY entrypoint.sh /entrypoint.sh
RUN chown jenkins:jenkins /entrypoint.sh
RUN chmod +x /entrypoint.sh

RUN wget https://releases.hashicorp.com/terraform/1.6.6/terraform_1.6.6_linux_amd64.zip
RUN unzip terraform_1.6.6_linux_amd64.zip
RUN mv terraform /bin && chmod +x /bin/terraform

USER jenkins

ENTRYPOINT ["/entrypoint.sh"]

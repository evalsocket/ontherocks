FROM alpine:latest

ARG VCS_REF
ARG BUILD_DATE


LABEL org.label-schema.vcs-ref=$VCS_REF \
      org.label-schema.name="dev-tool" \
      org.label-schema.url="https://hub.docker.com/r/evalsocket/ontherocks" \
      org.label-schema.vcs-url="https://github.com/evalsocket/ontherocks" \
      org.label-schema.build-date=$BUILD_DATE

RUN apk --no-cache add \
	ca-certificates \
	groff \
	less \
	python \
	py2-pip \
	&& pip install awscli \
	&& mkdir -p /root/.aws 


RUN apk update && \
    apk add curl jq python bash ca-certificates git openssl unzip wget 

ENV TERRAFORM_VERSION="0.12.6"

RUN cd /tmp && \
    wget https://releases.hashicorp.com/terraform/${TERRAFORM_VERSION}/terraform_${TERRAFORM_VERSION}_linux_amd64.zip && \
    unzip terraform_${TERRAFORM_VERSION}_linux_amd64.zip -d /usr/bin && \
    rm -rf /tmp/* && \
    rm -rf /var/cache/apk/* && \
    rm -rf /var/tmp/*

ENV KUBE_LATEST_VERSION="1.13.7"

RUN wget -q https://storage.googleapis.com/kubernetes-release/release/v${KUBE_LATEST_VERSION}/bin/linux/amd64/kubectl -O /usr/local/bin/kubectl \
    && chmod +x /usr/local/bin/kubectl 

RUN wget -q https://amazon-eks.s3-us-west-2.amazonaws.com/${KUBE_LATEST_VERSION}/2019-06-11/bin/linux/amd64/aws-iam-authenticator -O /usr/local/bin/aws-iam-authenticator \
    && chmod +x /usr/local/bin/aws-iam-authenticator
    

ENV HELM_VERSION="v2.13.1"

RUN wget -q https://storage.googleapis.com/kubernetes-helm/helm-${HELM_VERSION}-linux-amd64.tar.gz -O - | tar -xzO linux-amd64/helm > /usr/local/bin/helm \
    && chmod +x /usr/local/bin/helm

RUN curl -o kustomize --location https://github.com/kubernetes-sigs/kustomize/releases/download/v3.1.0/kustomize_3.1.0_linux_amd64 && mv ./kustomize /usr/local/bin/kustomize && chmod u+x /usr/local/bin/kustomize

CMD ["/bin/bash"]
# Desc: Standard kubernetes container, with everything needed to communicate
#       with the STC cluster
#
# Args:
#   AWS_TOKEN_ID: AWS authorization token ID\Access Key Id
#   AWS_TOKEN: AWS authorization token\Secret Access Key
#
# Notes:
#   The token and token id buuld args can be set with the --build-arg flag,
#       such as: docker build . --build-arg AWS_TOKEN=123abc --build-arg AWS_TOKEN_ID=456def
#
# Authors:
#   David Newman <email_address.com>

FROM alpine

ARG AWS_TOKEN_ID
ARG AWS_TOKEN

# Install dependencies
RUN apk add curl python3 py-pip

# eksctl
RUN wget https://github.com/weaveworks/eksctl/releases/download/latest_release/eksctl_$(uname -s)_amd64.tar.gz
RUN tar zxf eksctl*.tar.gz
RUN chmod +x eksctl
RUN mv eksctl /bin

# kubectl
RUN wget https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl
RUN chmod +x kubectl
RUN mv kubectl /bin

# AWS IAM Authenticator
RUN wget https://amazon-eks.s3-us-west-2.amazonaws.com/1.13.7/2019-06-11/bin/linux/amd64/aws-iam-authenticator
RUN chmod +x aws*
RUN mv aws* /bin 

# awscli
RUN pip install awscli

# Inject AWS credentials
RUN (echo ${AWS_TOKEN_ID} && echo ${AWS_TOKEN} && echo "us-east-2" && echo "json") | aws configure --profile stc-one-platform-dev

#SET CONFIG VALUES/TOKENS
RUN aws eks --profile stc-one-platform-dev update-kubeconfig --name stc

ENTRYPOINT ["kubectl"]

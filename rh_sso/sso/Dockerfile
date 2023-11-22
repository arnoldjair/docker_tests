FROM registry.redhat.io/rh-sso-7/sso76-openshift-rhel8:7.6-35
ARG GLB_GROUP_PJT_ID
ARG GLB_PRIVATE_TOKEN
ARG GLB_GROUP_PJT_ID
ARG MAVEN_GROUP_ID
ARG MAVEN_ARTIFACT_ID
ARG MAVEN_ARTIFACT_VERSION
ARG GLB_ARTIFACT_FILE_NAME
ARG FILE_TYPE
COPY extensions/postgresql-42.6.0.jar /opt/eap/extensions/
COPY extensions/postconfigure.sh /opt/eap/extensions/
COPY extensions/sso-extensions.cli /opt/eap/extensions/
USER root
RUN chmod 774 /opt/eap/extensions/*.sh
USER 185
RUN curl --header "Private-Token: $GLB_PRIVATE_TOKEN" "https://gitlab.com/api/v4/projects/$GLB_GROUP_PJT_ID/packages/maven/$MAVEN_GROUP_ID/$MAVEN_ARTIFACT_ID/$MAVEN_ARTIFACT_VERSION/$GLB_ARTIFACT_FILE_NAME$FILE_TYPE" -o /opt/eap/standalone/deployments/$GLB_ARTIFACT_FILE_NAME$FILE_TYPE
CMD ["/opt/eap/bin/openshift-launch.sh"]
{{- if .Values.configMap.data -}}
apiVersion: v1
kind: ConfigMap
metadata:
  name: {{ .Release.Name }}-{{ .Values.configMap.nameSuffix }}
  namespace: {{ .Release.Namespace }}
  labels:
    {{- include "transaction.labels" . | nindent 4 }}
{{- if .Values.configMap.additionalLabels }}
    {{- toYaml .Values.configMap.additionalLabels | nindent 4 }}
{{- end }}
{{- if .Values.configMap.annotations }}
  annotations:
    {{- toYaml .Values.configMap.annotations | nindent 4 }}
{{- end }}
data:
  application.properties: |
    spring.application.name=epay_transaction_service
    server.port=9093
    server.servlet.context-path=/api/transaction/v1

    # Db connectivity
    spring.jpa.show-sql=true
    spring.jpa.properties.hibernate.show_sql=true 
    #spring.datasource.url=jdbc:oracle:thin:@10.177.135.4:1590:epaysit2
    #spring.datasource.url=jdbc:oracle:thin:@10.177.135.1:1590:newuat1
    spring.datasource.url=jdbc:oracle:thin:@(DESCRIPTION=(ADDRESS=(PROTOCOL=TCP)(HOST=10.177.135.13)(PORT=1524))(CONNECT_DATA= (SERVER=DEDICATED) (SERVICE_NAME=newuat)))
    spring.datasource.username=PAYAGGTRANS
    spring.datasource.password=SIA#2025
    spring.datasource.driver-class-name=oracle.jdbc.OracleDriver

    # Liquibase Properties
    spring.liquibase.change-log=classpath:db/changelog/db.changelog-master.xml
    spring.liquibase.enabled=true
    spring.liquibase.drop-first=false
    logging.level.liquibase=DEBUG

    #External API
    external.api.admin.services.base.path=http://admin-adminservice.uat-admin.svc.cluster.local:9094/api/admin/v1
    external.api.kms.services.base.path=http://kms-kmsservice.uat-kms.svc.cluster.local:9093/api/kms/v1
    external.api.merchant.services.base.path=http://merchant-merchantservice.uat-merchant.svc.cluster.local:9093/api/merchant/v1
    external.api.payment.services.base.path=http://payment-paymentservice.uat-payment.svc.cluster.local:9093/api/payments/v1
    external.api.reports.services.base.path=http://reports-reportsservice.uat-reports.svc.cluster.local:9093/api/reports/v1

    #GemFire settings
    spring.data.gemfire.pool.locators=gemfire-uat-system-locator.uat-cache-gemfire[10334]
    spring.data.gemfire.use-bean-factory-locator=false
    spring.data.gemfire.cache.server.port=40404
    spring.data.gemfire.logging.level=info

    spring.data.gemfire.security.ssl.keystore=/certs/keystore.p12
    spring.data.gemfire.security.ssl.keystore.password=VAmyM-dtMTtAXGA1J8UkYoMc0WYLXA7wOxLfCNl1yu0=
    spring.data.gemfire.security.ssl.keystore.type=PKCS12
    spring.data.gemfire.security.ssl.truststore=/certs/truststore.p12
    spring.data.gemfire.security.ssl.truststore.password=VAmyM-dtMTtAXGA1J8UkYoMc0WYLXA7wOxLfCNl1yu0=
    spring.data.gemfire.security.ssl.truststore.type=PKCS12

    #spring.data.gemfire.security.ssl.components=all
    spring.data.gemfire.security.ssl.enabled-components=all
    spring.data.gemfire.security.ssl.enable-endpoint-identification=false
    spring.data.gemfire.security.ssl.require-authentication=false
    spring.data.gemfire.security.ssl.web-require-authentication=false
    spring.data.gemfire.security.ssl.protocols=any
    spring.data.gemfire.security.ssl.ciphers=any

    spring.data.gemfire.pdx.read-serialized=false
    spring.data.gemfire.pdx.auto-serializable-classes=com.epay.admin.repository.cache.*
    logging.level.org.apache.geode=debug
    logging.level.org.springframework.data.gemfire=debug

    #Kafka settings
    spring.kafka.bootstrapServers=uat-cluster-kafka-bootstrap.uat-kafka.svc.cluster.local:9092
    spring.kafka.consumer.groupId=transaction-consumers
    spring.kafka.consumer.keyDeserializer=org.apache.kafka.common.serialization.StringDeserializer
    spring.kafka.consumer.valueDeserializer=org.apache.kafka.common.serialization.StringDeserializer
    spring.kafka.consumer.autoOffsetReset=latest
    spring.kafka.consumer.autoCommitInterval=100
    spring.kafka.consumer.enableAutoCommit=true
    spring.kafka.consumer.sessionTimeoutMS=300000
    spring.kafka.consumer.requestTimeoutMS=420000
    spring.kafka.consumer.fetchMaxWaitMS=200
    spring.kafka.consumer.maxPollRecords=5
    spring.kafka.consumer.retryMaxAttempts=3
    spring.kafka.consumer.retryBackOffInitialIntervalMS=10000
    spring.kafka.consumer.retryBackOffMaxIntervalMS=30000
    spring.kafka.consumer.retryBackOffMultiplier=2
    spring.kafka.consumer.spring.json.trusted.packages=com.epay.transaction
    spring.kafka.consumer.numberOfConsumers=1

    spring.kafka.producer.keyDeserializer=org.apache.kafka.common.serialization.StringSerializer
    spring.kafka.producer.valueDeserializer=org.apache.kafka.common.serialization.StringSerializer
    spring.kafka.producer.acks=all
    spring.kafka.producer.retries=3
    spring.kafka.producer.batchSize=1000
    spring.kafka.producer.lingerMs=1
    spring.kafka.producer.bufferMemory=33554432

    spring.kafka.topic.transaction.notification.sms=transaction_sms_notification_topic
    spring.kafka.topic.transaction.notification.email=transaction_email_notification_topic
    spring.kafka.topic.alert=transaction_alert_topic

    spring.kafka.topic.partitions=4
    spring.kafka.topic.replicationFactor=1

    #Scheduler settings
    scheduler.cron.expression.transaction=0 */5 * * * ?
    scheduler.cron.expression.alert=0 */5 * * * ?
    scheduler.cron.expression.notification=0 */5 * * * ?
{{- end }}








# Default values for epaytransactionservice.
# This is a YAML-formatted file.
# Declare variables to be passed into your templates.

replicaCount: 3

image:
  repository: "registry.dev.sbiepay.sbi:8443/library/{{ .Chart.Name }}"
  pullPolicy: IfNotPresent
  # Overrides the image tag whose default is the chart appVersion.
  #tag: "v-release-20250506135014"
  tag: "v-release-20250506135014"

imagePullSecrets:
  - name: regcred
nameOverride: ""
fullnameOverride: ""

serviceAccount: 
  create: true
  annotations:
    app.kubernetes.io/part-of: epay-platform
  name: txn-service-account

podAnnotations:
  prometheus.io/scrape: "true"
  prometheus.io/port: "9092"
  prometheus.io/path: "/actuator/prometheus"
podLabels:
  app.kubernetes.io/part-of: epay-platform
  environment: pre-prod

podSecurityContext:
  runAsNonRoot: true
  runAsUser: 1000
  fsGroup: 1000
  seccompProfile:
    type: RuntimeDefault

securityContext:
  allowPrivilegeEscalation: false
  runAsNonRoot: true
  runAsUser: 1000
  capabilities:
    drop:
      - ALL
  readOnlyRootFilesystem: true

service:
  type: ClusterIP
  port: 9092
  annotations:
    prometheus.io/scrape: "true"
    prometheus.io/port: "9092"

namespace: pre-prod-transaction
gatewayName: pre-prod-istio-system/epay-pre-prod-gateway
virtualServiceName: txn-transactionservice
serviceName: txn-transactionservice
prefixName: /api/transaction/v1
servicePort: 9092
gatewayPort: 80
gatewayHost: "pre-prod.epay.sbi"

configMap: 
  additionalLabels:
    app.kubernetes.io/part-of: epay-platform
    environment: pre-prod
  annotations:
    app.kubernetes.io/managed-by: helm
  nameSuffix: "config"

resources:
  limits:
    cpu: 2
    memory: 4Gi
  requests:
    cpu: 1
    memory: 2Gi

ingress:
  enabled: false

readinessProbe:
  httpGet:
    path: /actuator/health/readiness
    port: 9092
  initialDelaySeconds: 30
  periodSeconds: 10
  timeoutSeconds: 5
  failureThreshold: 3
  successThreshold: 1

livenessProbe:
  httpGet:
    path: /actuator/health/liveness
    port: 9092
  initialDelaySeconds: 60
  periodSeconds: 20
  timeoutSeconds: 10
  failureThreshold: 6
  successThreshold: 1

autoscaling:
  enabled: true
  minReplicas: 3
  maxReplicas: 10
  targetCPUUtilizationPercentage: 80
  targetMemoryUtilizationPercentage: 80

# Additional volumes for configuration and secrets
volumes:
  - name: config-volume
    configMap:
      name: txn-transactionservice-config
  - name: secrets-volume
    secret:
      secretName: txn-transactionservice-secrets
  - name: tmp-volume
    emptyDir: {}

# Mount points for volumes
volumeMounts:
  - name: config-volume
    mountPath: "/opt/app/config"
    readOnly: true
  - name: secrets-volume
    mountPath: "/opt/app/secrets"
    readOnly: true
  - name: tmp-volume
    mountPath: "/tmp"

nodeSelector:
  node-type: app

# Ensure pods are scheduled on different nodes
affinity:
  podAntiAffinity:
    preferredDuringSchedulingIgnoredDuringExecution:
      - weight: 100
        podAffinityTerm:
          labelSelector:
            matchExpressions:
              - key: app.kubernetes.io/name
                operator: In
                values:
                  - txn-transactionservice
          topologyKey: kubernetes.io/hostname

# Allow scheduling on nodes with taints
tolerations:
  - key: "app"
    operator: "Equal"
    value: "transaction"
    effect: "NoSchedule"

# Pod disruption budget for high availability
podDisruptionBudget:
  enabled: true
  minAvailable: 2

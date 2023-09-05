# Read by plugin docker-maven-plugin/docker:build "...A relative path is looked up in ${project.basedir}/src/main/docker by default"
FROM openjdk:11-jre-slim as builder

WORKDIR application

# Use maven/... when using docker-maven-plugin
ADD maven/${project.build.finalName}.jar ./

RUN java -Djarmode=layertools -jar ${project.build.finalName}.jar extract

FROM openjdk:11-jre-slim

WORKDIR application

COPY --from=builder application/dependencies/ ./
COPY --from=builder application/spring-boot-loader/ ./
COPY --from=builder application/snapshot-dependencies/ ./
COPY --from=builder application/application/ ./

ENTRYPOINT ["java", "-Djava.security.egd=file:/dev/./urandom", "org.springframework.boot.loader.JarLauncher"]

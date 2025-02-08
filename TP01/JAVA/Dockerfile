FROM eclipse-temurin:21-jdk-alpine as builder

WORKDIR /JAVA

COPY Main.java .
RUN javac Main.java

FROM eclipse-temurin:21-jdk-alpine

WORKDIR /JAVA
COPY --from=builder /JAVA/Main.class .
CMD ["java", "Main"]
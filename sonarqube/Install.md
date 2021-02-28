# Sonarqube 安裝流程

![PostgreSQL](https://img.shields.io/badge/PostgreSQL-13.2-yellowgreen.svg)


## 環境需求

- [Docker](https://www.docker.com/)


## Docker Network 設定
1. **建立 Network**
```
docker network create <network-name>
```

## 建立資料庫

1. **下載 PostgreSQL 映像檔(Image)**  
```
docker pull postgres
```
2. **建立容器(Container)**
``` 
docker run --name <container-name> -d --network <network-name> -p 5432:5432 -e POSTGRES_USER=<user> POSTGRES_PASSWORD=<password> postgres
```

## 建立 SonarQube

1. **下載 SonarQube 映像檔(Image)**  
```
docker pull sonarqube:8.3.1-community
```

2. **建立容器(Container)**
```
docker run -d --name <container-name> --network <network-name> -p 9000:9000 -p 9092:9092 -e sonar.jdbc.username=<username> -e sonar.jdbc.password=<password> -e sonar.jdbc.url=jdbc:postgresql://<container>:5432/sonar sonarqube:8.3.1-community
```


## SonarQube Web 使用

- 連線至 SonarQube
> http://<docker-ip>:9000/

- 帳號密碼
> admin  admin

## Example

#### 1.設定 (Maven Plugin)
>>pom.xml 
>````
><build>
>   <plugins>
>      <plugin>
>         <groupId>org.sonarsource.scanner.maven</groupId>
>         <artifactId>sonar-maven-plugin</artifactId>
>         <version>3.5.0.1254</version>
>      </plugin>
>   </plugins>
></build>
>````

#### 2. maven profile 設定
>> pom.xml 
> ```
><profiles>
>   <profile>
>      <id>sonar</id>
>      <activation>
>          <activeByDefault>true</activeByDefault>
>      </activation>
>      <properties>
>          <!-- 配置 Sonar Host地址，默认：http://localhost:9000 -->
>          <sonar.host.url>
>              http://localhost:9000
>          </sonar.host.url>
>      </properties>
>   </profile>
></profiles>
> ```
>
>> 或是 %MAVEN_PATH%/conf/settings.xml 設定
> ```
>  
><profiles>
>   <profile>
>      <id>sonar</id>
>      <activation>
>          <activeByDefault>true</activeByDefault>
>      </activation>
>      <properties>
>          <!-- 配置 Sonar Host地址，默认：http://localhost:9000 -->
>          <sonar.host.url>
>              http://localhost:9000
>          </sonar.host.url>
>      </properties>
>   </profile>
></profiles>
> ```

#### 3.執行 
```
    mvn clean verify sonar:sonar
```


## 參考資料

- [Docker 建立 SonarQube](https://juejin.cn/post/6844904163374006286)
- [Docker 建立 SonarQube](https://juejin.cn/post/6844904001637449735)

## 聯絡作者

- [信箱](mailto:poshen0322@gmail.com)


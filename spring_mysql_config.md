# create mysql user
create user 'blockchain'@'%' identified by '111111';

# grant privilege
grant select, insert, delete, update on blockchian_training.* to 'blockchain'@'%';

# config application.properties
spring.jpa.hibernate.ddl-auto=update
spring.jpa.properties.hibernate.dialect = org.hibernate.dialect.MySQL5InnoDBDialect
spring.datasource.url=jdbc:mysql://127.0.0.1:3306/blockchian_training?Unicode=true&autoReconnect=true&useSSL=false&useLegacyDatetimeCode=false&serverTimezone=UTC
spring.datasource.username=blockchain
spring.datasource.password=111111


# reference 
https://spring.io/guides/gs/accessing-data-mysql/


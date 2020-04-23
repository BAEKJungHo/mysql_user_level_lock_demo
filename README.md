# MySQL을 이용한 분산락으로 여러 서버에 걸친 동시성 관리 예제코드

> https://woowabros.github.io/experience/2019/05/30/mysql-user-level-lock.html

```
// 블로그 댓글 내용 중
JPA가 아닌 mysql query 로 동시성 제어를 한다면,

SELECT ~ FOR UPDATE 로 구현하는게 더 심플할것 같습니다.

select count(*) from card where user_id=:user_id FOR UPDATE
```

### MySQL 설치
[Docker](https://www.docker.com/) 설치 후
```
docker run --name mysql57 \
    -p 3306:3306 \
    -e MYSQL_ROOT_PASSWORD=root \
    -e MYSQL_ROOT_HOST='%' \
    --restart=unless-stopped \
    -d \
    mysql/mysql-server:5.7
```
MySQL 콘솔접속
```
docker exec -it mysql57 bash
```
데이터베이스 생성
```
create database user_lock
```

### 예제코드 결과 확인
- `DemoApplication.java` 를 실행하여 api 서버를 실행합니다. api 서버가 실행될때 flyway 설정에 의해 데이터베이스의 내용이 초기화 됩니다.
- woowa.demo.test 패키지에 있는 클래스들을 실행하여 실행 결과를 확인합니다.
- 테이블의 내용을 초기화 하려면 직접 쿼리를 수행하거나 api 서버를 재시작 해주면 됩니다.

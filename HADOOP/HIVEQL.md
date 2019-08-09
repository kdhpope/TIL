### HIVEQL

- sql문과 거의 유사하다
- update 와 delete 를 사용할수 없다.
- subquery는 from절 외에서는 사용 불가
- view 사용불가
- seletct에서 having 사용 불가

partitioned by : 쿼리문의 수행 속도를 향상시키기 위해 테이블의 파티션을 설정하는 부분

#### Join

eq 조인만 가능하다.

from절에 하나만 가능하고, on키워드를 사용해야 함



#### JAVA와의 연동

##### hive

hive --service 서비스 이름(이름은 별로 의미 없다)

ex: hive --service hiveserver2



##### java

```java
		Class.forName("org.apache.hive.jdbc.HiveDriver");
		Connection conn=DriverManager.getConnection("jdbc:hive2://IP ADDRESS:PROT NUMBER/default","LINUX USER ID","PASSWORD");
        Statement stmt=conn.createStatement();
        ResultSet rs= stmt.executeQuery("SQL query");
        while(rs.next()) {
            System.out.println(rs.getString(2));
        }
        conn.close();
```


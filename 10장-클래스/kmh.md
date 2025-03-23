### 클래스

- 클래스는 작아야 한다

클래스의 크기는 해당 클래스의 책임은 전부 수행하되, 최대한 작아야 한다. 이에 대한 설명은 if, and, or, but을 사용하지 않고 25단어 내외로 가능해야 한다.

- 단일 책임 원칙(SRP, Single Responsibility Principle)

클래스나 모듈을 변경할 이유가 하나뿐이어야 한다는 원칙이다. 이는 클래스의 책임에 따라 분리하여, 비슷한 기능을 하는 메소드만을 묶은 클래스로 작성하는 것을 의미한다.

- 응집도

클래스는 인스턴스 변수 수가 작아야 한다. 각 클래스 메서드는 클래스 인스턴스 변수를 하나 이상 사용해야 하며, 일반적으로 메서드가 변수를 더 많이 사용할수록 메서드와 클래스의 응집도가 더 높다.


- 변경하기 쉬운 클래스

OCP와 관련된 내용으로 파생 클래스를 통해 기존 코드 수정없이 기능을 확장하는 예시를 보여준다. 즉, 결합도를 최소화하여 DIP(Dependency Inversion Principle)를 따르는 클래스를 작성해야 한다.

```java
// 기존 변경에 불리한 클래스
public class Sql {
public Sql(String table, Column[] columns)
public String create()
public String insert(Object[] fields)
public String selectAll()
public String findByKey(String keyColumn, String keyValue)
public String select(Column column, String pattern)
public String select(Criteria criteria)
public String preparedInsert()
private String columnList(Column[] columns)
private String valuesList(Objct[] feilds, final Column[] columns)
private String selectWithCriteria(String criteria)
private String placeholderList(Column[] columns)
}

// 결합도를 낮춰 분리한 클래스
abstract public class Sql {
public Sql(String table, Column[] columns)
abstract public String generate();
}

public class CreateSql extends Sql {
public CreateSql(String table, Column[] columns)
@Overrid public String generate()
}

public class Select Sql extens Sql {
public SelectSql(String table, Column[] columns)
@Override publc String generate()
}

public class InsertSql extends Sql {
public InsetSql(String table, Column[] columns, Object[] fields)
@Override public String generate()
private String valuesList(Object[] fields, final Column[] columns)
}
.
.
.
}
```

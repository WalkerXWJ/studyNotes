本文介绍代码安全为java语言
# 基本概念
代码审计（Code Audit）是指对软件源代码进行系统性的安全检查和漏洞分析，以发现潜在的安全漏洞、逻辑缺陷、编码错误或不符合安全规范的问题。它是白盒测试的一种形式，通常由安全工程师、开发人员或第三方安全团队执行，目的是在软件发布或部署前发现并修复安全问题，降低被攻击的风险。

# 常见漏洞
## 空指针解引用
### 概念
java空指针解引用（Null Pointer Dereference）​是指程序尝试访问或操作一个 `null`引用的对象成员（如调用方法、访问属性等），导致抛出 `NullPointerException`（NPE）。这是 Java 开发中最常见的运行时错误之一。
### 危害
java空指针解引用可能导致程序异常终止，或出现拒绝服务的安全影响。
### 典型漏洞场景
#### 场景1:代码中调用null对象的方法
漏洞代码：
```java
String str = null;
int length = str.length(); // 调用null对象的方法将抛出异常 NullPointerException
```
已修复代码：
```java
String str = null;
int length = (str != null) ? str.length() : 0; //增加显式null判断 如果 str 为 null，返回默认值 0
System.out.println(length); //str为null 输出: 0
```
#### 场景2:代码中访问null对象的属性
漏洞代码：
```java
​class User {
    String name;
}

User user = null;
System.out.println(user.name); //代码中user.name访问null对象的属性将抛出异常 NullPointerException
```
已修复代码：
```java
class User {
    String name;
}

User user = null;
if (user != null) { //增加显式是否为null的判断
    System.out.println(user.name); // 安全访问
} else {
    System.out.println("用户不存在"); // 处理 null 情况
}
```
#### 场景3:数组未初始化访问数组元素
漏洞代码：
```java
int[] arr = null;
System.out.println(arr[0]); // 数组未初始化访问数组元素将抛出异常 NullPointerException
```
已修复代码：
```java
int[] arr = null;
if (arr != null && arr.length > 0) { //增加显式是否为null的判断
    System.out.println(arr[0]); // 安全访问
} else {
    System.out.println("数组未初始化或为空");
}
```
#### 场景4:自动拆箱（Unboxing）导致的 NPE
漏洞代码：
```java
​Integer num = null;
int value = num; //自动拆箱过程会自动调用num.intValue()出现访问null对象方法的情况 将抛出异常 NullPointerException
```
已修复代码：
```java
Integer num = null;
int value = (num != null) ? num : 0; //增加显式是否为null的判断 如果 num 为 null，返回默认值 0
System.out.println(value); 
```
#### 场景5:方法可能返回null，但调用方未检查
漏洞代码：
```java
public String getName() {
    return null; // 可能返回 null
}

String name = getName();
name.toUpperCase(); // 当调用方法返回为null，对返回值未做检查将抛出异常 NullPointerException
```
已修复代码：
```java
public String getName() {
    return null; // 可能返回 null
}

String name = getName();
if (name != null) { // 增加显式是否为null的判断
    System.out.println(name.toUpperCase());
} else {
    System.out.println("姓名为空"); // 处理 null 情况
}
```
### 修复建议描述
1. 编程中在调用对象的属性和方法前，针对对象可能为null的情况，增加显示的非null判断，针对为null和非null进行不同的业务处理。
2. 编程中针对访问的数组可能未初始化，在访问数组元素前，增加显示的非null判断，针对为null和非null进行不同的业务处理。
3. 编程中涉及自动拆箱和调用方法返回值可能为null的情况，增加显示的非null判断，针对为null和非null进行不同的业务处理。
4. 在条件允许的情况下，可以考虑封装统一的安全检查方法。
### 人工审计建议
1. 检查可能为null的对象，在调用对象的属性和方法前是否进行了非null判断，未进行合理检查则有问题。
2. 检查可能未初始化的数组，在访问数组元素前是否进行非null判断，未进行合理检查则有问题。
3. 涉及自动拆箱操作的代码，检查值是否可能为null，是否在自动拆箱操作前进行了非null判断，未进行合理检查则有问题。
4. 检查调用的方法是否会返回null值，针对可能为null的值，是否在调用其他方法时进行了非null判断，未进行合理检查则有问题。
5. 如果程序中自定义了安全检查函数已进行非null处理，且处理逻辑严谨合理，则没有问题。
## sql注入
### 概念
SQL 注入是一种将恶意的 SQL 代码插入到应用程序的数据库查询字符串中的攻击技术。如果应用程序使用用户输入的数据来动态构建 SQL 语句，并且没有对输入进行适当的验证或转义，攻击者就可以操纵这些查询，从而执行非预期的数据库操作。
### 危害
恶意攻击人员可以通过sql注入漏洞获取数据库中的数据，例如登陆的账户和密码，个人敏感信息等，在数据库权限足够的情况下可以通过sql注入漏洞像服务器写入shell，从而进一步获取服务器的权限。sql注入漏洞会导致企业信息被泄漏，任意用户登陆，数据被修改、删除等，同时可能导致服务器被攻击者完全控制。
### 典型漏洞场景
#### 场景1:字符串拼接 SQL（最经典）
漏洞代码：
```java
// 示例用户登录功能 - 用户名和密码字符拼接导致严重SQL注入
public boolean loginVulnerable(String username, String password) {
    Connection conn = null;
    Statement stmt = null;
    ResultSet rs = null;
    
    try {
        conn = dataSource.getConnection();
        stmt = conn.createStatement();
        
        // 直接拼接用户输入 将导致sql注入
        String sql = "SELECT * FROM users WHERE username = '" + username 
                    + "' AND password = '" + password + "'";
        
        System.out.println("执行的SQL: " + sql); // 日志中可以看到完整SQL
        
        rs = stmt.executeQuery(sql);
        return rs.next(); // 如果查询到结果返回true
        
    } catch (SQLException e) {
        e.printStackTrace();
        return false;
    } finally {
        // 关闭资源...
    }
}

// 攻击示例：username = "admin' -- ", password = "anything"
// 生成SQL: SELECT * FROM users WHERE username = 'admin' -- ' AND password = 'anything'
// 结果：绕过密码验证，直接登录admin账户
```
已修复代码：
```java
// 使用 PreparedStatement 修复
public boolean loginSafe(String username, String password) {
    String sql = "SELECT * FROM users WHERE username = ? AND password = ?";
    
    try (Connection conn = dataSource.getConnection();
         PreparedStatement pstmt = conn.prepareStatement(sql)) {
        
        // 参数化查询，防止SQL注入
        pstmt.setString(1, username);
        pstmt.setString(2, password);
        
        try (ResultSet rs = pstmt.executeQuery()) {
            return rs.next();
        }
        
    } catch (SQLException e) {
        e.printStackTrace();
        return false;
    }
}
```
#### 场景2:使用 StringBuilder/StringBuffer 拼接
漏洞代码：
```java
// 示例用户搜索功能 使用 StringBuilder/StringBuffer 拼接keyword和sortBy参数将导致严重SQL注入
public List<User> searchUsersVulnerable(String keyword, String sortBy) {
    List<User> users = new ArrayList<>();
    
    StringBuilder sql = new StringBuilder("SELECT * FROM users WHERE 1=1");
    
    // 动态字符串拼接查询条件keyword 触发sql注入
    if (keyword != null && !keyword.trim().isEmpty()) {
        sql.append(" AND (username LIKE '%").append(keyword).append("%'")
           .append(" OR email LIKE '%").append(keyword).append("%')");
    }
    
    // 动态字符串拼接查询条件sortBy参数触发ORDER BY 注入
    if (sortBy != null) {
        sql.append(" ORDER BY ").append(sortBy);
    }
    
    try (Connection conn = dataSource.getConnection();
         Statement stmt = conn.createStatement();
         ResultSet rs = stmt.executeQuery(sql.toString())) {
         
        while (rs.next()) {
            users.add(mapResultSetToUser(rs));
        }
        
    } catch (SQLException e) {
        e.printStackTrace();
    }
    return users;
}

// 攻击示例：
// keyword = "test'; DROP TABLE users; -- "
// sortBy = "id; DROP TABLE products -- "
```
已修复代码：
```java
// 示例用户搜索功能 修复后的安全版本
public List<User> searchUsersSafe(String keyword, String sortBy) {
    List<User> users = new ArrayList<>();
    
    StringBuilder sql = new StringBuilder("SELECT * FROM users WHERE 1=1");
    List<Object> params = new ArrayList<>();
    
    // 安全处理：参数化查询条件
    if (keyword != null && !keyword.trim().isEmpty()) {
        sql.append(" AND (username LIKE ? OR email LIKE ?)");
        params.add("%" + keyword + "%");
        params.add("%" + keyword + "%");
    }
    
    // 安全处理：ORDER BY 使用白名单
    if (sortBy != null && isValidSortColumn(sortBy)) {
        sql.append(" ORDER BY ").append(sortBy);
    } else {
        sql.append(" ORDER BY id"); // 默认排序
    }
    
    try (Connection conn = dataSource.getConnection();
         PreparedStatement pstmt = conn.prepareStatement(sql.toString())) {
         
        // 设置参数
        for (int i = 0; i < params.size(); i++) {
            pstmt.setObject(i + 1, params.get(i));
        }
        
        try (ResultSet rs = pstmt.executeQuery()) {
            while (rs.next()) {
                users.add(mapResultSetToUser(rs));
            }
        }
        
    } catch (SQLException e) {
        e.printStackTrace();
    }
    return users;
}

// 白名单验证排序字段
private boolean isValidSortColumn(String column) {
    Set<String> allowedColumns = Set.of("id", "username", "email", "create_time");
    return allowedColumns.contains(column);
}
```
#### 场景3:IN 查询的 SQL 注入

```java
// 示例根据ID列表查询用户  IN 查询拼接idList参数导致注入
public List<User> findUsersByIdsVulnerable(List<String> ids) {
    List<User> users = new ArrayList<>();
    
    if (ids == null || ids.isEmpty()) {
        return users;
    }
    
    // sql语句直接拼接idList触发sql注入
    String idList = String.join(",", ids);
    String sql = "SELECT * FROM users WHERE id IN (" + idList + ")";
    
    try (Connection conn = dataSource.getConnection();
         Statement stmt = conn.createStatement();
         ResultSet rs = stmt.executeQuery(sql)) {
         
        while (rs.next()) {
            users.add(mapResultSetToUser(rs));
        }
        
    } catch (SQLException e) {
        e.printStackTrace();
    }
    return users;
}

// 攻击示例：ids = ["1", "2) OR 1=1 -- "]
// 生成SQL: SELECT * FROM users WHERE id IN (1,2) OR 1=1 -- )
// 结果：查询所有用户数据
```
已修复代码：
```java
// 示例根据ID列表查询用户 安全的IN查询实现
public List<User> findUsersByIdsSafe(List<Integer> ids) {
    List<User> users = new ArrayList<>();
    
    if (ids == null || ids.isEmpty()) {
        return users;
    }
    
    // 生成占位符：?,?,?
    String placeholders = ids.stream()
                            .map(id -> "?")
                            .collect(Collectors.joining(","));
                            
    String sql = String.format("SELECT * FROM users WHERE id IN (%s)", placeholders);
    
    try (Connection conn = dataSource.getConnection();
         PreparedStatement pstmt = conn.prepareStatement(sql)) {
         
        // 设置每个参数
        for (int i = 0; i < ids.size(); i++) {
            pstmt.setInt(i + 1, ids.get(i));
        }
        
        try (ResultSet rs = pstmt.executeQuery()) {
            while (rs.next()) {
                users.add(mapResultSetToUser(rs));
            }
        }
        
    } catch (SQLException e) {
        e.printStackTrace();
    }
    return users;
}
```
#### 场景4:MyBatis 中的 SQL 注入​
漏洞代码：不安全的 Mapper XML​
```xml
<!-- UserMapper.xml - 存在SQL注入 -->
<select id="findByUsername" parameterType="String" resultType="User">
    SELECT * FROM users 
    WHERE username LIKE '%${username}%'  <!-- 使用 ${} 拼接 将触发sql注入 -->
</select>

<select id="dynamicOrder" parameterType="Map" resultType="User">
    SELECT * FROM users 
    ORDER BY ${sortField} ${sortOrder}  <!-- 使用 ${} 拼接动态排序 将触发sql注入 -->
</select>
```
已修复代码：不安全的 Mapper XML​
```xml
<!-- 使用 #{} 参数占位符 -->
<select id="findByUsername" parameterType="String" resultType="User">
    SELECT * FROM users 
    WHERE username LIKE CONCAT('%', #{username}, '%')  <!-- #{}动态取值是安全的 -->
</select>

<!-- 排序字段使用白名单 可防止sql注入 -->
<select id="dynamicOrder" parameterType="Map" resultType="User">
    SELECT * FROM users 
    ORDER BY 
    <choose>
        <when test="sortField == 'username'">username</when>
        <when test="sortField == 'email'">email</when>
        <otherwise>id</otherwise>
    </choose>
    <choose>
        <when test="sortOrder == 'DESC'">DESC</when>
        <otherwise>ASC</otherwise>
    </choose>
</select>
```
漏洞代码：不安全的注解方式​
```java
// Mapper 接口 存在注入风险
public interface UserMapper {
    
    @Select("SELECT * FROM users WHERE username = '${username}'")  // ${}触发sql注入
    User findByUsername(@Param("username") String username);
    
    @Select("SELECT * FROM users ORDER BY ${sortBy}")  // ${}触发sql注入
    List<User> findAllWithSort(@Param("sortBy") String sortBy);
}
```
已修复代码：不安全的注解方式​
```java
// Mapper 接口 安全实现方式
public interface UserMapper {
    
    @Select("SELECT * FROM users WHERE username = #{username}")  // 安全
    User findByUsername(@Param("username") String username);
    
    // 动态排序需要在代码中处理
    default List<User> findAllWithSortSafe(String sortBy) {
        String safeSortBy = validateSortColumn(sortBy); //排序字段白名单检查
        String finalSql = "SELECT * FROM users ORDER BY " + safeSortBy;
        
        // 使用 @SelectProvider 实现动态SQL
        return findAllWithSortDynamic(finalSql);
    }
    
    @SelectProvider(type = UserSqlProvider.class, method = "getFindAllSql")
    List<User> findAllWithSortDynamic(String sql);
    
    private String validateSortColumn(String column) {
        Set<String> allowed = Set.of("id", "username", "email");
        return allowed.contains(column) ? column : "id";
    }
}
```
#### 场景5:JPA/Hibernate 中的 SQL 注入
漏洞代码：
```java
@Repository
public class UserRepository {
    
    @PersistenceContext
    private EntityManager entityManager;
    
    // 拼接原生SQL 导致sql漏洞
    public List<User> findUsersByNameVulnerable(String name) {
        String sql = "SELECT * FROM users WHERE name = '" + name + "'"; // sql拼接触发sql注入
        Query query = entityManager.createNativeQuery(sql, User.class);
        return query.getResultList();
    }
    
    // 不安全的JPQL拼接 导致sql注入
    public List<User> findUsersByEmailVulnerable(String email) {
        String jpql = "SELECT u FROM User u WHERE u.email = '" + email + "'"; // sql拼接触发sql注入
        TypedQuery<User> query = entityManager.createQuery(jpql, User.class);
        return query.getResultList();
    }
}
```
已修复代码：
```java
@Repository
public class UserRepository {
    
    @PersistenceContext
    private EntityManager entityManager;
    
    // 安全：使用参数化原生SQL
    public List<User> findUsersByNameSafe(String name) {
        String sql = "SELECT * FROM users WHERE name = ?1";
        Query query = entityManager.createNativeQuery(sql, User.class)
                                  .setParameter(1, name);
        return query.getResultList();
    }
    
    // 安全：使用参数化JPQL
    public List<User> findUsersByEmailSafe(String email) {
        String jpql = "SELECT u FROM User u WHERE u.email = :email";
        TypedQuery<User> query = entityManager.createQuery(jpql, User.class)
                                             .setParameter("email", email);
        return query.getResultList();
    }
    
    // 最佳实践：使用Spring Data JPA
    public interface UserJpaRepository extends JpaRepository<User, Long> {
        
        // 方法名查询 自动防注入
        List<User> findByName(String name);
        
        // @Query 注解安全使用
        @Query("SELECT u FROM User u WHERE u.email = :email")
        List<User> findByEmail(@Param("email") String email);
        
        // 原生查询也要参数化
        @Query(value = "SELECT * FROM users WHERE name = ?1", nativeQuery = true)
        List<User> findByNameNative(String name);
    }
}

```
#### 场景6:动态表名/列名场景​
漏洞代码：
```java
// 动态表名查询 导致sql注入风险
public int getCountVulnerable(String tableName, String condition) {
    // 动态表名和条件拼接 导致sql注入
    String sql = "SELECT COUNT(*) FROM " + tableName + " WHERE " + condition; //  tableName 和 condition 字段拼接触发sql注入
    
    try (Connection conn = dataSource.getConnection();
         Statement stmt = conn.createStatement();
         ResultSet rs = stmt.executeQuery(sql)) {
         
        return rs.next() ? rs.getInt(1) : 0;
        
    } catch (SQLException e) {
        e.printStackTrace();
        return 0;
    }
}
```
已修复代码：
```java
// 安全的动态表名/列名处理
public int getCountSafe(String tableName, String columnName, String value) {
    // 白名单验证表名和列名
    if (!isValidTableName(tableName) || !isValidColumnName(columnName)) {
        throw new IllegalArgumentException("无效的表名或列名");
    }
    
    // 使用参数化查询值
    String sql = String.format("SELECT COUNT(*) FROM %s WHERE %s = ?", 
                              tableName, columnName);
    
    try (Connection conn = dataSource.getConnection();
         PreparedStatement pstmt = conn.prepareStatement(sql)) {
         
        pstmt.setString(1, value);
        
        try (ResultSet rs = pstmt.executeQuery()) {
            return rs.next() ? rs.getInt(1) : 0;
        }
        
    } catch (SQLException e) {
        e.printStackTrace();
        return 0;
    }
}

// 白名单验证
private boolean isValidTableName(String tableName) {
    Set<String> allowedTables = Set.of("users", "products", "orders");
    return allowedTables.contains(tableName.toLowerCase());
}

private boolean isValidColumnName(String columnName) {
    Set<String> allowedColumns = Set.of("id", "name", "email", "status");
    return allowedColumns.contains(columnName.toLowerCase());
}

```
#### 场景7:String.format() 拼接 SQL
漏洞代码：
```java
// 使用 String.format() 拼接 SQL 导致sql注入
public List<User> findUsersByNameVulnerable(String name) {
    // 看起来比字符串拼接"优雅"，但同样危险 ，和sql注入场景1效果一样
    // MessageFormat 同样存在风险 如果 String sql = MessageFormat.format("SELECT * FROM users WHERE name = '{0}'", input);也存在sql注入
    String sql = String.format("SELECT * FROM users WHERE name = '%s'", name);
    
    try (Connection conn = dataSource.getConnection();
         Statement stmt = conn.createStatement();
         ResultSet rs = stmt.executeQuery(sql)) {
         
        return mapResultSetToList(rs);
    } catch (SQLException e) {
        e.printStackTrace();
        return new ArrayList<>();
    }
}

// 攻击示例：
// name = "admin' OR '1'='1"
// 生成SQL: SELECT * FROM users WHERE name = 'admin' OR '1'='1'
// 结果：返回所有用户数据
```
已修复代码：
```java
// 安全：String.format() 只用于静态部分，动态值用参数化查询
public List<User> findUsersByNameSafe(String name) {
    // 只格式化静态部分
    String sql = String.format("SELECT * FROM users WHERE name = ?");
    
    try (Connection conn = dataSource.getConnection();
         PreparedStatement pstmt = conn.prepareStatement(sql)) {
         
        pstmt.setString(1, name);  // 参数化设置值
        
        try (ResultSet rs = pstmt.executeQuery()) {
            return mapResultSetToList(rs);
        }
    } catch (SQLException e) {
        e.printStackTrace();
        return new ArrayList<>();
    }
}
```
#### 场景8:复杂的动态查询构建
漏洞代码：
```java
// 多层格式化嵌套
public String buildDynamicQueryVulnerable(Map<String, String> filters) {
    StringBuilder whereClause = new StringBuilder();
    
    for (Map.Entry<String, String> entry : filters.entrySet()) {
        if (whereClause.length() > 0) {
            whereClause.append(" AND ");
        }
        // 直接格式化用户输入
        whereClause.append(String.format("%s = '%s'", entry.getKey(), entry.getValue()));
    }
    
    String sql = String.format("SELECT * FROM products WHERE %s", whereClause.toString()); //sql注入触发
    return sql;
}

// 攻击示例：
// filters = { "name": "test", "category": "1' OR category <> '1" }
// 生成SQL: SELECT * FROM products WHERE name = 'test' AND category = '1' OR category <> '1'
// 结果：绕过条件限制
```
已修复代码：
```java
//预编译实现
public PreparedStatement buildDynamicQuerySafe(Connection connection, Map<String, String> filters) throws SQLException {
    StringBuilder whereClause = new StringBuilder();
    List<Object> parameters = new ArrayList<>();
    
    for (Map.Entry<String, String> entry : filters.entrySet()) {
        if (whereClause.length() > 0) {
            whereClause.append(" AND ");
        }
        // 验证列名合法性 防止SQL注入
        String columnName = validateColumnName(entry.getKey());
        whereClause.append(columnName).append(" = ?");
        parameters.add(entry.getValue());
    }
    
    String sql = "SELECT * FROM products";
    if (whereClause.length() > 0) {
        sql += " WHERE " + whereClause.toString();
    }
    
    PreparedStatement stmt = connection.prepareStatement(sql);
    for (int i = 0; i < parameters.size(); i++) {
        stmt.setObject(i + 1, parameters.get(i));
    }
    
    return stmt;
}

// 验证列名合法性，防止SQL注入
private String validateColumnName(String columnName) {
    // 只允许字母、数字和下划线
    if (!columnName.matches("[a-zA-Z_][a-zA-Z0-9_]*")) {
        throw new IllegalArgumentException("Invalid column name: " + columnName);
    }
    return columnName;
}
```
#### 场景9:日志记录中的二次注入​
漏洞代码：
```java
// 日志记录可能被用于后续SQL执行 导致的sql注入
public void auditUserActionVulnerable(String userId, String action) {
    // 记录审计日志
    String auditLog = String.format(
        "User '%s' performed action: '%s' at %s", 
        userId, action, new Date()
    );
    
    // 日志内容可能被用于后续SQL查询
    String sql = String.format("INSERT INTO audit_logs (message) VALUES ('%s')", auditLog); //sql注入触发
    
    try (Connection conn = dataSource.getConnection();
         Statement stmt = conn.createStatement()) {
         
        stmt.executeUpdate(sql);
    } catch (SQLException e) {
        e.printStackTrace();
    }
}

// 攻击示例：
// userId = "admin'; DROP TABLE users; -- "
// 生成的审计日志内容包含恶意SQL
// 如果日志系统后续执行这些内容，会导致SQL注入
```
已修复代码：
```java
// 使用严格校验和预编译修复漏洞
public void auditUserActionSecure(String userId, String action) {
    // 输入验证
    if (!isValidUserId(userId) || !isValidAction(action)) {
        throw new IllegalArgumentException("Invalid input parameters");
    }
    
    String auditLog = String.format(
        "User '%s' performed action: '%s' at %s", 
        userId, action, new Date()
    );
    
    // 使用预编译语句
    String sql = "INSERT INTO audit_logs (message) VALUES (?)";
    
    try (Connection conn = dataSource.getConnection();
         PreparedStatement pstmt = conn.prepareStatement(sql)) {
         
        pstmt.setString(1, auditLog);
        pstmt.executeUpdate();
    } catch (SQLException e) {
        e.printStackTrace();
    }
}

// 严格的输入验证
private boolean isValidUserId(String userId) {
    // 只允许字母、数字、下划线，长度限制
    return userId != null && userId.matches("^[a-zA-Z0-9_]{1,50}$");
}

private boolean isValidAction(String action) {
    // 定义允许的操作列表
    Set<String> allowedActions = Set.of("login", "logout", "update", "create", "delete");
    return action != null && allowedActions.contains(action.toLowerCase());
}
```

### 修复建议描述
1. sql注入漏洞的防护核心思想是阻止来自用户输入的恶意sql拼接到sql中执行
2. 编程中可以采用预编译形式实现动态sql的参数优先采用预编译形式
3. 编程中不可采用预编译形式实现动态sql的参数，则应该建立被名单检查，如严格限制字符集、长度、正则格式，建立被名单列表检查规则。
4. 建立黑名单对参数进行安全检查则是下下之选，黑名单规则极容易被绕过。
### 人工审计建议
1. sql注入漏洞在人工审计时需要注意漏洞的可达性，如果从接口到业务、数据库整个调用链中存在有效的安全校验逻辑，或会异常阻断注入数据传递的数据类型转换代码，sql语句即使采用拼接构建，也通常认为是安全的。

## XML外部实体注入（XXE）
### 概念
XML外部实体注入（XXE）是一种安全漏洞，许多过时的或配置不当的 XML 处理器都会对外部实体进行引用，允许攻击者通过XML文档中的外部实体声明来读取文件、执行SSRF攻击、端口扫描或造成拒绝服务攻击。
XML允许定义自定义实体，包括外部实体，这些实体可以引用外部资源：
```xml
<!DOCTYPE root [
    <!ENTITY xxe SYSTEM "file:///etc/passwd">
]>
<root>&xxe;</root>
```
### 危害

### 典型漏洞场景
#### 场景1:使用易受攻击的XML解析器
漏洞代码：
```java
import javax.xml.parsers.DocumentBuilder;
import javax.xml.parsers.DocumentBuilderFactory;
import org.w3c.dom.Document;
import java.io.ByteArrayInputStream;

public class XXEVulnerableParser {
    
    // 易受攻击的XML解析
    public static Document parseVulnerableXML(String xmlData) throws Exception {
        DocumentBuilderFactory factory = DocumentBuilderFactory.newInstance();
        DocumentBuilder builder = factory.newDocumentBuilder();
        
        // 默认配置允许外部实体
        return builder.parse(new ByteArrayInputStream(xmlData.getBytes()));
    }
    
    // 攻击示例
    public static void main(String[] args) {
        String maliciousXML = "<?xml version=\"1.0\"?>"
            + "<!DOCTYPE root ["
            + "  <!ENTITY xxe SYSTEM \"file:///etc/passwd\">"
            + "]>"
            + "<root>&xxe;</root>";
        
        try {
            Document doc = parseVulnerableXML(maliciousXML);
            System.out.println(doc.getDocumentElement().getTextContent());
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```
已修复代码：
```java
import javax.xml.parsers.DocumentBuilderFactory;
import javax.xml.parsers.DocumentBuilder;
import org.w3c.dom.Document;
import java.io.ByteArrayInputStream;

public class XXESafeParser {
    
    public static Document parseSafeXML(String xmlData) throws Exception {
        DocumentBuilderFactory factory = DocumentBuilderFactory.newInstance();
        
        // 关键安全配置
        // 1. 禁用DOCTYPE声明 最根本的防护措施
        // 设置此特性为true将完全禁止DTD声明，从根本上防止XXE攻击
        // 如果XML中包含<!DOCTYPE>声明，解析器将抛出异常
        factory.setFeature("http://apache.org/xml/features/disallow-doctype-decl", true);
        
        // 2. 禁用外部通用实体
        // 防止引用外部实体的攻击，如：<!ENTITY xxe SYSTEM "file:///etc/passwd">
        // 设置为false表示不解析外部通用实体
        factory.setFeature("http://xml.org/sax/features/external-general-entities", false);
        
        // 3. 禁用外部参数实体
        // 防止参数实体攻击，这类攻击常用于绕过一些防护措施
        // 参数实体以%开头，如：<!ENTITY % xxe SYSTEM "http://attacker.com/malicious.dtd">
        factory.setFeature("http://xml.org/sax/features/external-parameter-entities", false);
        
        // 4. 禁用外部DTD加载
        // 防止从外部加载DTD文件，避免DTD文件中的恶意内容被执行
        // 这个特性控制是否加载外部的DTD子集
        factory.setFeature("http://apache.org/xml/features/nonvalidating/load-external-dtd", false);
        
        // 5. 禁用XInclude处理
        // XInclude是另一种包含外部内容的方式，也可能被利用进行攻击
        factory.setXIncludeAware(false);
        
        // 6. 禁用实体引用扩展
        // 确保实体引用不会被实际内容替换，进一步减少风险
        factory.setExpandEntityReferences(false);
        
        DocumentBuilder builder = factory.newDocumentBuilder();
        return builder.parse(new ByteArrayInputStream(xmlData.getBytes()));
    }
}
```
#### 场景2:SAX解析器漏洞
漏洞代码：
```java
import org.xml.sax.InputSource;
import org.xml.sax.helpers.DefaultHandler;
import javax.xml.parsers.SAXParser;
import javax.xml.parsers.SAXParserFactory;
import java.io.StringReader;

public class XXEVulnerableSAX {
    
    public static void parseWithSAX(String xmlData) throws Exception {
        SAXParserFactory factory = SAXParserFactory.newInstance();
        SAXParser parser = factory.newSAXParser();
        
        // 易受攻击的SAX解析
        parser.parse(new InputSource(new StringReader(xmlData)), new DefaultHandler());
    }
}
```
已修复代码：
```java
//使用安全的SAXParser
import org.xml.sax.InputSource;
import org.xml.sax.helpers.DefaultHandler;
import javax.xml.parsers.SAXParser;
import javax.xml.parsers.SAXParserFactory;
import javax.xml.parsers.ParserConfigurationException;
import org.xml.sax.SAXException;
import java.io.StringReader;
import java.io.IOException;

/**
 * XXE安全防护SAX解析器 - 修复漏洞版本
 * 修复了原始代码中的XML外部实体注入(XXE)漏洞
 */
public class XXESafeSAXFixed {
    
    // 修复XXE漏洞后的安全SAX解析方法
    // 通过配置安全特性防止XML外部实体注入攻击
    public static void parseWithSAXSecure(String xmlData) throws ParserConfigurationException, SAXException, IOException {
        SAXParserFactory factory = SAXParserFactory.newInstance();
        
        // 1. 核心防护 禁用DOCTYPE声明 从根本上阻止XXE攻击
        factory.setFeature("http://apache.org/xml/features/disallow-doctype-decl", true);
        
        // 2. 备用防护 禁用外部通用实体
        factory.setFeature("http://xml.org/sax/features/external-general-entities", false);
        
        // 3. 备用防护 禁用外部参数实体
        factory.setFeature("http://xml.org/sax/features/external-parameter-entities", false);
        
        // 4. 启用命名空间感知（提高安全性）
        // 作用：使解析器能够正确处理命名空间，避免命名空间混淆攻击
        factory.setNamespaceAware(true);
        
        // 5. 禁用验证（减少攻击面）
        // 作用：不使用DTD验证，避免验证过程中的潜在风险
        factory.setValidating(false);
        
        SAXParser parser = factory.newSAXParser();
        
        // 使用安全配置的解析器处理XML数据
        // InputSource包装StringReader，避免文件路径相关的攻击向量
        parser.parse(new InputSource(new StringReader(xmlData)), new DefaultHandler());
    }

```
#### 场景3:XPath注入结合XXE
漏洞代码：
```java
import javax.xml.xpath.XPath;
import javax.xml.xpath.XPathFactory;
import javax.xml.xpath.XPathExpression;
import org.xml.sax.InputSource;

public class XXEXPathVulnerable {
    
    public static void vulnerableXPathQuery(String xmlData, String userInput) throws Exception {
        XPathFactory xpathFactory = XPathFactory.newInstance();
        XPath xpath = xpathFactory.newXPath();
        
        // 用户输入直接拼接到XPath
        String expression = "//user[username='" + userInput + "']";
        XPathExpression expr = xpath.compile(expression);
        
        // 同时存在XXE风险
        Object result = expr.evaluate(new InputSource(new StringReader(xmlData)));
    }
}
```
已修复代码：
```java
import javax.xml.stream.XMLInputFactory;
import javax.xml.stream.XMLStreamReader;
import java.io.StringReader;

// 使用安全的XMLInputFactory（StAX）
public class XXESafeStAX {
    
    public static void parseSafeStAX(String xmlData) throws Exception {
        XMLInputFactory factory = XMLInputFactory.newInstance();
        
        // 安全配置
        // 禁用DTD支持 防止基于DTD的XXE攻击
        // 设置为false后，解析器将忽略所有DTD声明
        factory.setProperty(XMLInputFactory.SUPPORT_DTD, false);
        
        // 禁用外部实体支持 防止外部实体注入攻击
        // 设置为false阻止解析外部实体引用
        factory.setProperty(XMLInputFactory.IS_SUPPORTING_EXTERNAL_ENTITIES, false);
        
        XMLStreamReader reader = factory.createXMLStreamReader(new StringReader(xmlData));
        while (reader.hasNext()) {
            reader.next();
            // 处理XML
        }
        reader.close();
    }
}
```
#### 场景4:XML反序列化漏洞
漏洞代码：
```java
import java.beans.XMLDecoder;
import java.io.ByteArrayInputStream;

public class XXEXMLDecoder {
    
    public static void vulnerableXMLDeserialization(String xmlData) {
        // XMLDecoder会执行XML中的代码
        XMLDecoder decoder = new XMLDecoder(new ByteArrayInputStream(xmlData.getBytes()));
        Object obj = decoder.readObject(); // xml反序列化
        decoder.close();
    }
    
    // 恶意XML示例
    public static String createMaliciousXML() {
        return "<?xml version=\"1.0\"?>"
            + "<java version=\"1.4.0\" class=\"java.beans.XMLDecoder\">"
            + "  <object class=\"java.lang.Runtime\" method=\"getRuntime\">"
            + "    <void method=\"exec\">"
            + "      <array class=\"java.lang.String\" length=\"1\">"
            + "        <void index=\"0\"><string>calc.exe</string></void>"
            + "      </array>"
            + "    </void>"
            + "  </object>"
            + "</java>";
    }
}
```
已修复代码：
```java
import java.beans.XMLDecoder;
import java.io.ByteArrayInputStream;
import java.util.HashSet;
import java.util.Set;
import java.util.regex.Pattern;

public class SafeXMLDeserializer {
    
    // 允许反序列化的类白名单
    private static final Set<String> ALLOWED_CLASSES = new HashSet<>();
    private static final Pattern DANGEROUS_PATTERN = Pattern.compile(
        "java\\.lang\\.(Runtime|ProcessBuilder)|exec\\(|newInstance\\(|forName\\(",
        Pattern.CASE_INSENSITIVE
    );
    
    static {
        // 只允许基本类型和安全的类 根据实际需求修改
        ALLOWED_CLASSES.add("java.lang.String");
        ALLOWED_CLASSES.add("java.lang.Integer");
        ALLOWED_CLASSES.add("java.lang.Long");
        ALLOWED_CLASSES.add("java.lang.Double");
        ALLOWED_CLASSES.add("java.util.Date");
        ALLOWED_CLASSES.add("java.util.ArrayList");
        ALLOWED_CLASSES.add("java.util.HashMap");
    }
    
    /**
     * 安全的XML反序列化方法
     */
    public static Object safeDeserialize(String xmlData) {
        if (xmlData == null || xmlData.trim().isEmpty()) {
            throw new IllegalArgumentException("XML数据不能为空");
        }
        
        // 检测恶意内容
        if (DANGEROUS_PATTERN.matcher(xmlData).find()) {
            throw new SecurityException("检测到危险的XML内容");
        }
        
        try (XMLDecoder decoder = new XMLDecoder(new ByteArrayInputStream(xmlData.getBytes()))) {
            Object result = decoder.readObject();
            
            // 验证反序列化的类在白名单中
            if (result != null && !ALLOWED_CLASSES.contains(result.getClass().getName())) {
                throw new SecurityException("不允许反序列化类: " + result.getClass().getName());
            }
            
            return result;
        }
    }
}
```
### 修复建议描述
1. 对于不需要文档类型定义（DTD）功能的场景，推荐完全禁用 DTD 处理。
2. 对于需要 DTD 功能但希望防止外部实体攻击的场景，采用限制性配置。
3. 在 XML 解析前对输入数据进行预处理，过滤恶意内容。
### 人工审计建议：
1. 识别代码库中的 XML 处理组件​，如果已安全禁用DTD则没有风险或有严格的数据预处理，过滤恶意内容，则没有风险。
2. 未禁用DTD且未进行数据预处理，则有风险。
```shell
# 搜索关键词 - 全局搜索
grep -r "DocumentBuilderFactory" src/
grep -r "SAXParserFactory" src/  
grep -r "XMLInputFactory" src/
grep -r "TransformerFactory" src/
grep -r "SAXReader" src/          # DOM4J
grep -r "SAXBuilder" src/         # JDOM
grep -r "XMLDecoder" src/         # 特殊风险点
grep -r "XPathExpression" src/    # XPath 处理

# 搜索配置文件中的 XML 相关配置
grep -r "xml" pom.xml build.gradle
find . -name "*.xml" -type f | head -20
```


## 命令注入
### 概念
Java 命令注入是一种安全漏洞，当应用程序使用不可信的用户输入来构造操作系统命令，并且没有进行适当的过滤或转义时，攻击者就可以通过精心构造的输入，在应用程序的上下文中执行非预期的、恶意的操作系统命令。其本质在于，程序将用户输入数据错误地当作了命令的一部分来执行。
### 危害
1. **服务器完全沦陷**​：攻击者可以执行任意系统命令，从而完全控制运行该Java应用的服务器。
2. ​**数据泄露**​：读取服务器上的敏感文件，如数据库密码、配置文件、用户数据等。
3. ​**数据篡改或删除**​：修改网页内容、删除数据库或重要文件，导致服务中断。
4. ​**内网渗透**​：以被攻陷的服务器为跳板，进一步攻击内网中的其他系统。
5. ​**植入恶意软件**​：下载并运行木马、挖矿程序、勒索软件等。
### 典型漏洞场景
#### 场景 1：使用 Runtime.exec()执行系统命令
漏洞代码：
```java
import java.io.*;

// 允许用户输入ip地址 执行ping操作漏洞代码示例
public class VulnerablePing {
    public static void main(String[] args) throws IOException {
        String ip = args[0]; // 用户直接控制输入，例如 "8.8.8.8; cat /etc/passwd"
        
        // 漏洞：直接将用户输入拼接到命令中
        String command = "ping -c 4 " + ip;
        Process process = Runtime.getRuntime().exec(command); //命令执行触发
        
        // 打印结果
        BufferedReader reader = new BufferedReader(new InputStreamReader(process.getInputStream()));
        String line;
        while ((line = reader.readLine()) != null) {
            System.out.println(line);
        }
    }
}
```
已修复代码：
```java
// 采用白名单对用户输入进行检查 例如，对于IP地址，只允许数字和点

import java.io.*;

public class FixedPingWithWhitelist {
    public static void main(String[] args) throws IOException {
        String ip = args[0];
        
        // 修复：使用白名单验证输入
        if (!ip.matches("^[0-9.]+$")) { // 实际应用应使用更严格的IP地址正则表达式
            throw new IllegalArgumentException("Invalid IP address format.");
        }
        
        String command = "ping -c 4 " + ip;
        Process process = Runtime.getRuntime().exec(command);
        
        
    }
}

```
#### 场景 2：使用 `ProcessBuilder`但参数拼接不当
漏洞代码：
```java
import java.io.*;
import java.util.*;

public class VulnerableProcessBuilder {
    public static void main(String[] args) throws IOException {
        String fileName = args[0]; // 用户输入文件名 如： "important_file.txt; rm -rf /"
        
        // 错误用法：仍然将命令作为一个字符串整体传入
        ProcessBuilder pb = new ProcessBuilder("/bin/sh", "-c", "ls -l " + fileName);
        pb.redirectErrorStream(true);
        Process process = pb.start();
        
        // ... 打印结果
    }
}
```
已修复代码：
```java
//使用 ProcessBuilder并分离命令与参数
import java.io.*;
import java.util.*;

public class FixedProcessBuilder {
    public static void main(String[] args) throws IOException {
        String fileName = args[0]; // 用户输入文件名 如： "important_file.txt; rm -rf /"
        
        // 修复：将命令和参数作为独立的字符串传入，避免使用Shell解释器 
        //修复后执行的实际命令​：`ls -l "important_file.txt; rm -rf /"`   其中 rm -rf / 不在被当作单独的命令执行
        ProcessBuilder pb = new ProcessBuilder("ls", "-l", fileName);
        pb.redirectErrorStream(true);
        Process process = pb.start();
        
        // ... 打印结果
    }
}
```
#### 场景 3：通过脚本引擎间接执行命令
漏洞代码：
```java
import javax.script.*;

public class VulnerableScriptEngine {
    public static void main(String[] args) throws ScriptException {
        String userScript = args[0]; // 用户输入的脚本 如："‘rm -rf /‘.execute().text"
        
        ScriptEngineManager manager = new ScriptEngineManager();
        ScriptEngine engine = manager.getEngineByName("groovy");
        
        // 漏洞：直接执行用户控制的脚本 Groovy 脚本中的 `‘command‘.execute()`可以执行系统命令
        Object result = engine.eval(userScript);
        System.out.println("Result: " + result);
    }
}
```
已修复代码：
```java
//完全禁用脚本中的系统命令执行能力
import javax.script.*;

public class SaferScriptEngine {
    public static void main(String[] args) throws ScriptException {
        if (args.length < 1) {
            System.out.println("Usage: java SaferScriptEngine <script>");
            return;
        }
        
        String userScript = args[0];
        
        ScriptEngineManager manager = new ScriptEngineManager();
        ScriptEngine engine = manager.getEngineByName("groovy");
        
        // 使用 Groovy 沙箱或限制脚本能力
        // 设置安全管理器来限制危险操作
        System.setSecurityManager(new SecurityManager());
        
        try {
            Object result = engine.eval(userScript);
            System.out.println("Result: " + result);
        } catch (SecurityException e) {
            System.out.println("Security violation: " + e.getMessage());
        }
    }
}

```
### 修复建议描述
1. 用户可控的参数建议不要直接传入命令执行的函数中，必须传入执行命令时，可以采用白名单列表，建立命令执行的对应关系，如传入1、2分别对应不同的命令内容，不允许直接传入命令内容。
2. 禁止将用户输入直接拼接到命令中执行
3. 在无法使用被名单策略的场景下，可以使用可以采取黑名单策略，如正则匹配、函数或方法黑名单、关键字黑名单，但黑名单策略随着时间的推移容易被绕过。
### 人工审计建议
关键字搜索：
```java
// 高风险关键词
Runtime.getRuntime().exec(
ProcessBuilder(
new ProcessBuilder(
/bin/sh
cmd /c
scriptEngine.eval(
```
人工审计命令注入时，需要考虑漏洞可触发的问题，要求参数必须用户可控。
可触达的条件如下：
1. 用户参数可控，用户可以修改传入的参数内容；
2. 用户输入内容不会被代码中安全检查策略阻断或安全检查策略可以被绕过；
3. 用户输入内容会在命令执行的关键函数中形成新的命令并执行。
## 路径遍历
### 概念
路径遍历漏洞，也称为目录遍历漏洞，是一种因对用户输入验证不严而导致的安全漏洞。攻击者通过构造特殊的输入（通常包含 ../等目录跳转序列），使应用程序访问或操作其本不应访问的文件系统路径。简单来说，就是程序本意是读取或写入一个特定目录下的文件（如 images/avatar.jpg），但由于使用了未经验证的用户输入来拼接路径，攻击者可以通过输入 ../../../etc/passwd这样的字符串，让程序跳转到预期之外的目录，从而读取、修改甚至删除敏感文件。
### 危害
路径遍历漏洞的危害非常严重，通常会导致：
- 敏感信息泄露​：读取服务器上的任意文件，如：
- 配置文件：/etc/passwd, /etc/shadow（Linux），C:\Windows\System32\drivers\etc\hosts（Windows）
- 应用程序源代码、配置文件（web.xml, application.properties）
- 数据库连接凭证、SSL 私钥等。
- 文件篡改或写入​：在服务器上写入恶意文件，例如 WebShell，从而获取服务器控制权。
- 拒绝服务​：删除或篡改关键系统文件，导致应用程序或整个系统崩溃。
- 逻辑破坏​：破坏应用程序的正常运行逻辑。
### 典型漏洞场景
#### 场景1：文件下载/查看功能
漏洞代码：
```java
@RestController
public class FileController {

    // 假设文件都存放在 "/opt/app/uploads/" 目录下
    private String BASE_PATH = "/opt/app/uploads/";

    @GetMapping("/download")
    public void downloadFile(@RequestParam("filename") String filename, 
                             HttpServletResponse response) {
        // 直接拼接用户输入的文件名，未做任何过滤 恶意内容被拼接
        File file = new File(BASE_PATH + filename);
		//如用户发送请求 GET /download?filename=../../../etc/passwd
        try (FileInputStream fis = new FileInputStream(file); //读取非预期的文件
             OutputStream os = response.getOutputStream()) {

            // ... 设置 response headers (Content-Type, Content-Disposition) ...

            byte[] buffer = new byte[1024];
            int bytesRead;
            while ((bytesRead = fis.read(buffer)) != -1) {
                os.write(buffer, 0, bytesRead);
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```
已修复代码：
```java
// 修复的思想是 ​规范化路径，然后验证其是否仍在允许的基目录内。
@RestController
public class FileController {

    private String BASE_PATH = "/opt/app/uploads/";

    @GetMapping("/download")
    public void downloadFile(@RequestParam("filename") String filename, 
                             HttpServletResponse response) throws IOException {

        // 1 对输入进行基本的清理
        String safeFilename = FilenameUtils.getName(filename); // 使用 Apache Commons IO 库，只取文件名部分，去掉路径

        // 2 拼接基路径和清理后的文件名
        Path basePath = Paths.get(BASE_PATH).toAbsolutePath().normalize();
        Path filePath = basePath.resolve(safeFilename).normalize();

        // 3 关键验证规范化后的路径是否仍然以基路径开头
        if (!filePath.startsWith(basePath)) {
            // 路径被遍历出去了，拒绝请求
            response.sendError(403, "Access Denied: Invalid file path.");
            return;
        }

        File file = filePath.toFile();
        if (!file.exists()) {
            response.sendError(404, "File not found.");
            return;
        }

        //  安全的文件流复制操作 
        try (FileInputStream fis = new FileInputStream(file);
             OutputStream os = response.getOutputStream()) {
            // 设置 headers 
            IOUtils.copy(fis, os); // 使用 Apache Commons IO 简化流操作
        }
    }
}
```
#### 场景2：文件上传功能
漏洞代码：
```java
// 上传功能不仅保存文件，还使用原始文件名，并且后续有引用该文件的操作，也可能存在路径遍历风险
@PostMapping("/upload")
public String uploadFile(@RequestParam("file") MultipartFile file) {
    if (file.isEmpty()) {
        return "Upload failed.";
    }

    try {
        // 使用用户提供的原始文件名直接保存 存在问题
        String originalFileName = file.getOriginalFilename();
        File destFile = new File("/opt/app/uploads/" + originalFileName);
        file.transferTo(destFile); // 文件被保存到 destFile 指定的路径
        return "Upload success: " + originalFileName;
    } catch (IOException e) {
        e.printStackTrace();
        return "Upload failed.";
    }
}
// 攻击说明：假如攻击者可以上传一个名为 `malicious.jsp`的文件，但将文件名修改为 `../../../tomcat/webapps/ROOT/malicious.jsp`。如果应用有足够权限，这个 WebShell 就会被上传到 Web 根目录，从而可以被直接访问执行。
```
已修复代码：
```java
//**忽略用户提供的路径，使用自己生成的、安全的文件名**
@PostMapping("/upload")
public String uploadFile(@RequestParam("file") MultipartFile file) {
    if (file.isEmpty()) {
        return "Upload failed.";
    }

    try {
        //  1 获取原始文件名并剥离路径
        String originalFileName = file.getOriginalFilename();
        String safeFileName = FilenameUtils.getName(originalFileName); // 使用 Apache Commons IO 库，只取文件名部分，去掉路径

        // 2 生成一个唯一的、安全的存储文件名 防止覆盖和注入
        // 如：使用 UUID + 文件扩展名
        String fileExtension = safeFileName.substring(safeFileName.lastIndexOf("."));
        String storedFileName = UUID.randomUUID().toString() + fileExtension;

        // 3 确定保存路径
        Path basePath = Paths.get("/opt/app/uploads").toAbsolutePath().normalize();
        Path destPath = basePath.resolve(storedFileName);

        // 再次确保目录正确 虽然这里 resolve 的是随机名，但习惯性检查
        if (!destPath.normalize().startsWith(basePath)) {
            throw new IOException("Invalid file path.");
        }

        // 创建目标目录 如果不存在
        Files.createDirectories(destPath.getParent());

        // 保存文件
        file.transferTo(destPath.toFile());

        // 在数据库中记录 originalFileName 和 storedFileName 的映射关系
        // fileService.saveFileMapping(originalFileName, storedFileName, ...);

        return "Upload success. File stored as: " + storedFileName;
    } catch (IOException e) {
        e.printStackTrace();
        return "Upload failed.";
    }
}
```
#### 场景3：zip压缩包解压
```java
//这是一个特殊的路径遍历漏洞，发生在解压不受信任的 ZIP 压缩包时
public void extractZip(File zipFile) throws IOException {
    byte[] buffer = new byte[1024];
    try (ZipInputStream zis = new ZipInputStream(new FileInputStream(zipFile))) {
        ZipEntry entry = zis.getNextEntry();
        while (entry != null) {
            // 危险：直接使用 ZIP 条目名称作为输出路径
            File newFile = new File("/opt/app/extracted/" + entry.getName());

            // 如果条目是目录，则创建
            if (entry.isDirectory()) {
                newFile.mkdirs();
            } else {
                // 写入文件
                try (FileOutputStream fos = new FileOutputStream(newFile)) {
                    int len;
                    while ((len = zis.read(buffer)) > 0) {
                        fos.write(buffer, 0, len);
                    }
                }
            }
            entry = zis.getNextEntry();
        }
        zis.closeEntry();
    }
}

//攻击者可以创建一个恶意的 ZIP 文件，其中包含一个名为 `../../../../tmp/evil.sh`的条目。当程序解压时，这个文件就会被写入到系统的 `/tmp`目录下
```
已修复代码：
```java
//修复思路 规范化路径并验证
public void extractZip(File zipFile) throws IOException {
    byte[] buffer = new byte[1024];
    Path basePath = Paths.get("/opt/app/extracted").toAbsolutePath().normalize();

    try (ZipInputStream zis = new ZipInputStream(new FileInputStream(zipFile))) {
        ZipEntry entry = zis.getNextEntry();
        while (entry != null) {
            // 1 对条目名称进行规范化验证
            Path targetPath = basePath.resolve(entry.getName()).normalize();

            // 2 关键 验证目标路径是否在基目录内
            if (!targetPath.startsWith(basePath)) {
                throw new IOException("ZIP entry contains illegal path traversal: " + entry.getName());
            }

            if (entry.isDirectory()) {
                Files.createDirectories(targetPath);
            } else {
                // 3  确保父目录存在
                Files.createDirectories(targetPath.getParent());
                try (FileOutputStream fos = new FileOutputStream(targetPath.toFile())) {
                    int len;
                    while ((len = zis.read(buffer)) > 0) {
                        fos.write(buffer, 0, len);
                    }
                }
            }
            entry = zis.getNextEntry();
        }
        zis.closeEntry();
    }
}

```
### 修复建议描述
1. 默认不信任用户输入，如果可能，只允许特定的、安全的字符出现在文件名中。
2. 始终使用 Path.normalize()解析路径，并检查最终路径是否在预期的基目录内。
3. 优先使用 java.nio.file.Paths和 Path，而不是简单的字符串拼接。
4. 对于上传的文件，使用程序生成的唯一名称，避免使用用户提供的名称。
5. 使用安全库​。如 Apache Commons IO 的 FilenameUtils，它提供了很多有用的路径处理函数。
### 人工审计建议：
路径遍历人工审计的核心思想，寻找所有将用户输入（直接或间接）用于文件系统操作的地方，并验证是否存在有效的安全控制
```java
在 IDE 中全局搜索以下关键类和API：

​经典 java.ioAPI:​​
new File(…)
FileInputStream, FileOutputStream
FileReader, FileWriter

​现代 java.nioAPI:​​
Paths.get(…), Path.of(…)
Files.readAllBytes(…), Files.write(…), Files.newInputStream(…), Files.lines(…)
FileSystem相关操作

​ZIP 解压相关:​​
ZipInputStream, ZipFile
ZipEntry.getName()

​Spring 框架相关:​​
MultipartFile.getOriginalFilename()
Resource接口的实现（如 UrlResource, FileSystemResource）

​文件工具类:​​
FileUtils(), IOUtils
Files()
```
审计过程需要注意用户输入的参数是可以产生目录变化才有问题，同时需要注意可能不安全的检验方法。
## 反射型 XSS
### 概念
Java 反射型XSS（跨站脚本攻击）是一种Web安全漏洞，发生在服务器将用户输入的数据未经适当处理直接返回给客户端浏览器时。攻击者可以注入恶意脚本，当其他用户访问受影响页面时，脚本会在其浏览器中执行。
### 危害
- 窃取用户会话Cookie和敏感信息
- 盗取用户凭证和进行未授权操作
- 劫持用户账户
- 传播恶意软件
- 网站篡改和钓鱼攻击
### 典型漏洞场景
#### 场景1.：JSP 直接输出用户输入
漏洞代码：
```jsp
<!-- 漏洞代码 获取请求中的q字段的值，直接在放在jsp页面中 将导致反射型xss问题-->
<%
    String searchQuery = request.getParameter("q");
%>
<div>搜索结果: <%= searchQuery %></div>
```
已修复代码：
```jsp
<!-- 修复后代码 JSP html转义处理-->
<%
    String searchQuery = request.getParameter("q");
    if (searchQuery != null) {
        // HTML转义处理
        searchQuery = searchQuery.replace("&", "&amp;")
                                .replace("<", "&lt;")
                                .replace(">", "&gt;")
                                .replace("\"", "&quot;")
                                .replace("'", "&#x27;");
    } else {
        searchQuery = "";
    }
%>

<div>搜索结果: <%= searchQuery %></div>
```
#### 场景2： Spring MVC 控制器直接返回用户输入
```java
// 模版直接取用户输入 导致反射型xss
@Controller
public class SearchController {
	/**
     * 处理搜索请求
     * 
     * @param query 用户输入的搜索关键词 - 直接从HTTP请求参数获取
     * @param model Spring MVC模型对象，用于向视图传递数据
     * @return 视图名称"search-results"
     * 
     * 漏洞详情：
     * 1. XSS攻击风险：query参数未经任何转义直接添加到模型
     * 2. 攻击示例：恶意用户可构造URL如：
     *    /search?query=<script>alert('XSS')</script>
     * 3. 如果模板中直接使用${searchTerm}且未转义，会执行恶意脚本
     */
    @GetMapping("/search")
    public String search(@RequestParam String query, Model model) {
        model.addAttribute("searchTerm", query); // 直接添加未转义的用户输入 触发xss
        return "search-results"; // 返回搜索结果页面视图
    }
}
```
已修复代码：
```java
//使用Spring的HtmlUtils进行转义
import org.springframework.web.util.HtmlUtils;

@Controller
public class SearchController {
    @GetMapping("/search")
    public String search(@RequestParam String query, Model model) {
        // 对用户输入进行HTML转义
        String escapedQuery = HtmlUtils.htmlEscape(query);
        model.addAttribute("searchTerm", escapedQuery);
        return "search-results";
    }
}
```
#### 场景3：Servlet 直接输出到响应
漏洞场景：
```java
// 直接获取用户输入，经简单拼接后返回给前端 导致反射型xss问题
protected void doGet(HttpServletRequest request, HttpServletResponse response) 
        throws ServletException, IOException {
    String username = request.getParameter("username"); //获取用户username字段输入内容
    response.getWriter().println("欢迎, " + username); // 直接输出用户输入到前端
}
```
已修复代码：
```java
// 自定义转移工具方法
protected void doGet(HttpServletRequest request, HttpServletResponse response) 
        throws ServletException, IOException {
    String username = request.getParameter("username");
    
    // 手动实现HTML转义
    String safeUsername = escapeHtml(username);
    
    response.setContentType("text/html; charset=UTF-8");
    response.getWriter().println("欢迎, " + safeUsername);
}

/**
 * HTML转义工具方法
 */
private String escapeHtml(String input) {
    if (input == null) {
        return "";
    }
    
    StringBuilder sb = new StringBuilder();
    for (char c : input.toCharArray()) {
        switch (c) {
            case '&': sb.append("&amp;"); break;
            case '<': sb.append("&lt;"); break;
            case '>': sb.append("&gt;"); break;
            case '"': sb.append("&quot;"); break;
            case '\'': sb.append("&#x27;"); break;
            case '/': sb.append("&#x2F;"); break;
            default: sb.append(c);
        }
    }
    return sb.toString();
}
```
#### 场景4.REST API 返回未转义数据
漏洞代码：
```java
// 将用户的输入不经处理和查询的结果输入一起返回前端导致反射型xss问题
@RestController
public class UserController {
    @GetMapping("/user/profile")
    public String getUserProfile(@RequestParam String userId) {
        // 从数据库获取用户信息
        String userBio = userService.getUserBio(userId); //查询结果中包含输入的userId的输入内容
        return "{\"bio\": \"" + userBio + "\"}"; // JSON中直接拼接
    }
}
```
已修复代码：
```java
import org.springframework.http.ResponseEntity;
import java.util.Collections;

@RestController
public class UserController {
    
    @GetMapping("/user/profile")
    public ResponseEntity<Map<String, String>> getUserProfile(@RequestParam String userId) {
        // 从数据库获取用户信息
        String userBio = userService.getUserBio(userId);
        
        // 使用ResponseEntity返回安全的JSON
        Map<String, String> response = Collections.singletonMap("bio", userBio);
        return ResponseEntity.ok(response);
    }
}
```
#### 场景5：错误消息显示用户输入
漏洞代码：
```java
// Spring MVC 控制器直接返回用户输入的用户名 导致反射型xss
@Controller
public class LoginController {
    @PostMapping("/login")
    public String login(@RequestParam String username, 
                       @RequestParam String password, 
                       Model model) {
        if (!authService.authenticate(username, password)) {
            model.addAttribute("error", "登录失败: 用户 " + username + " 不存在");
            return "login"; //错误时返回login 视图
        }
        return "redirect:/dashboard";
    }
}
```
已修复代码：
```java
// 使用Spring的HtmlUtils进行转义
import org.springframework.web.util.HtmlUtils;

@Controller
public class LoginController {
    
    @PostMapping("/login")
    public String login(@RequestParam String username, 
                       @RequestParam String password, 
                       Model model) {
        if (!authService.authenticate(username, password)) {
            // 对用户名进行HTML转义
            String safeUsername = HtmlUtils.htmlEscape(username);
            model.addAttribute("error", "登录失败: 用户 " + safeUsername + " 不存在");
            return "login";
        }
        return "redirect:/dashboard";
    }
}
```
#### 场景6：URL 重定向参数未验证
漏洞代码：
```java
// 直接从前端获取重定向的url参数，简单拼接处理后返回前端，导致反射型xss问题
@Controller
public class RedirectController {
    @GetMapping("/redirect")
    public String redirect(@RequestParam String url) {
        return "redirect:" + url; // 开放重定向也可能导致XSS
    }
}
```
已修复代码：
```java
//使用映射表限制重定向目标
@Controller
public class RedirectController {
    
    // 预定义的重定向映射
    private static final Map<String, String> ALLOWED_REDIRECTS = Map.of(
        "home", "/",
        "login", "/login",
        "profile", "/user/profile",
        "external", "https://trusted-site.com"
    );
    
    @GetMapping("/redirect")
    public String redirect(@RequestParam String target) {
        // 只允许预定义的重定向目标
        String redirectUrl = ALLOWED_REDIRECTS.get(target);
        if (redirectUrl != null) {
            return "redirect:" + redirectUrl;
        }
        
        // 默认重定向
        return "redirect:/";
    }
}
```
### 修复建议描述
- 将需要返回给前端的用户输入使用安全转义方法进行处理
1. Spring Web Utils​
```java
import org.springframework.web.util.HtmlUtils;
import org.springframework.web.util.JavaScriptUtils;
import org.springframework.web.util.UriUtils;

// HTML转义
String safeHtml = HtmlUtils.htmlEscape("<script>alert('xss')</script>");
// 结果: &lt;script&gt;alert('xss')&lt;/script&gt;

// HTML转义（十进制）
String safeHtmlDecimal = HtmlUtils.htmlEscapeDecimal("<script>");
// 结果: &#60;script&#62;

// JavaScript转义
String safeJs = JavaScriptUtils.javaScriptEscape("alert('xss')");
// 结果: alert(\'xss\')

// URL编码
String safeUrl = UriUtils.encodePathSegment("user input", "UTF-8");
```
2. Spring Security HTML转义​
```java
import org.springframework.security.web.util.TextEscapeUtils;

// 安全的HTML文本转义
String safeText = TextEscapeUtils.escapeEntities("user<input>");
```
3. Apache Commons工具类
```java
import org.apache.commons.text.StringEscapeUtils;

// HTML转义
String escapedHtml = StringEscapeUtils.escapeHtml4("<script>alert('xss')</script>");

// XML转义
String escapedXml = StringEscapeUtils.escapeXml11("<user>name</user>");

// JavaScript转义
String escapedJs = StringEscapeUtils.escapeEcmaScript("alert('xss')");

// CSV转义
String escapedCsv = StringEscapeUtils.escapeCsv("value,with,commas");
```
4. OWASP ESAPI
```java
import org.owasp.esapi.ESAPI;
import org.owasp.esapi.Encoder;

Encoder encoder = ESAPI.encoder();

// HTML内容转义
String safeHtml = encoder.encodeForHTML("<script>alert('xss')</script>");

// HTML属性转义
String safeAttr = encoder.encodeForHTMLAttribute("user\"input");

// JavaScript转义
String safeJs = encoder.encodeForJavaScript("user'; alert('xss')");

// CSS转义
String safeCss = encoder.encodeForCSS("expression(alert('xss'))");

// URL转义
String safeUrl = encoder.encodeForURL("user input&param=value");

// Base64编码
String base64 = encoder.encodeForBase64("sensitive data".getBytes(), false);
```
5. 也可以通过自定义转移工具方法进行转义处理
```java
/**
 * 上下文感知的XSS防护工具
 */
public class ContextAwareXSSEncoder {
    
    public enum Context {
        HTML_CONTENT,    // HTML文本内容
        HTML_ATTRIBUTE,  // HTML属性值
        JAVASCRIPT,      // JavaScript代码
        URL,            // URL参数
        CSS             // CSS样式
    }
    
    /**
     * 根据上下文进行转义
     */
    public static String encodeForContext(String input, Context context) {
        if (input == null) return "";
        
        switch (context) {
            case HTML_CONTENT:
                return escapeHtmlContent(input);
            case HTML_ATTRIBUTE:
                return escapeHtmlAttribute(input);
            case JAVASCRIPT:
                return escapeJavaScript(input);
            case URL:
                return encodeUrlParam(input);
            case CSS:
                return escapeCss(input);
            default:
                return escapeHtmlContent(input);
        }
    }
    
    private static String escapeHtmlContent(String input) {
        return input.replace("&", "&amp;")
                   .replace("<", "&lt;")
                   .replace(">", "&gt;")
                   .replace("\"", "&quot;")
                   .replace("'", "&#x27;");
    }
    
    private static String escapeHtmlAttribute(String input) {
        return input.replace("\"", "&quot;")
                   .replace("'", "&#x27;")
                   .replace("&", "&amp;");
    }
    
    private static String escapeJavaScript(String input) {
        return input.replace("\\", "\\\\")
                   .replace("\"", "\\\"")
                   .replace("'", "\\'")
                   .replace("\n", "\\n")
                   .replace("\r", "\\r")
                   .replace("\t", "\\t");
    }
    
    private static String encodeUrlParam(String input) {
        try {
            return URLEncoder.encode(input, StandardCharsets.UTF_8.name());
        } catch (Exception e) {
            return "";
        }
    }
    
    private static String escapeCss(String input) {
        return input.replace("\\", "\\\\")
                   .replace("\"", "\\\"")
                   .replace("'", "\\'");
    }
}
```
### 人工审计建议
重点关注：用户输入 → 未经处理 → 直接输出到前端​的功能代码
1. HTTP请求获取关键字
```java
request.getParameter
request.getHeader
request.getQueryString
request.getParameterValues
request.getParameterMap
@RequestParam
@PathVariable
@ModelAttribute
HttpServletRequest
getParameter
```
2. 直接输出关键字
```java
response.getWriter().print
response.getWriter().println
out.print
out.println
<%= %>
${ }
model.addAttribute
ModelAndView
PrintWriter
JspWriter
```
3. 常见输出场景关键字
```java
转发：forward:
重定向：redirect:
包含：include
错误页面：errorPage
消息显示：message, error, msg, info
搜索功能：search, query, q
用户名显示：username, user, name
评论/内容显示：content, comment, text, desc
```
## 存储型 XSS
### 概念
存储型XSS，也称为持久型XSS，是跨站脚本攻击中最危险的一种。攻击者将恶意脚本（通常以 JavaScript 代码的形式）提交到目标网站的服务器上并被永久存储​（例如存入数据库、文件系统、内存等）。当其他用户访问网站，浏览器从服务器加载并展示包含该恶意数据的内容时，恶意脚本就会在用户的浏览器中执行。由于恶意数据来源于可信的服务器，这种攻击具有很高的隐蔽性和传播性。核心特征​是恶意脚本被存储在服务器端，影响所有访问到该恶意数据的用户。
### 危害
- 存储型 XSS 的危害非常严重，主要包括：
- 盗取用户会话凭证​：窃取用户的 Cookie（如 Session ID），导致攻击者能直接登录用户账户。
- 钓鱼攻击​：在页面中伪造登录框，诱骗用户输入用户名和密码。
- 篡改页面内容​：修改网页显示，传播虚假信息。
- 强制弹窗和广告​：影响用户体验。
- 键盘记录​：监听用户的键盘输入，获取敏感信息。
- 蠕虫传播​：结合 CSRF 等技术，恶意脚本可以自动转发，形成 XSS 蠕虫，在用户间迅速扩散（如著名的新浪微博蠕虫事件）。
- 企业内网渗透​：结合浏览器漏洞，可能进一步攻击用户内网系统。
### 典型漏洞场景
#### 场景1：直接输出用户内容
漏洞代码：
```java
// 漏洞示例 评论Servlet  直接存储和输出用户输入
@WebServlet("/addComment")
public class CommentServlet extends HttpServlet {
    private List<Comment> comments = new ArrayList<>();
    
    protected void doPost(HttpServletRequest request, HttpServletResponse response) {
        String content = request.getParameter("content");
        String author = request.getParameter("author");
        
        // 直接存储，未过滤
        Comment comment = new Comment(content, author, new Date());
        comments.add(comment);
        
        // 未经过安全处理 会将恶意输入的xss代码存储到数据库 
        saveToDatabase(comment);
    }
}

// 漏洞触发 JSP页面直接输出
<%@ page import="java.util.List" %>
<%
List<Comment> commentList = getCommentsFromDB();
for(Comment comment : commentList) {
%>
    <div class="comment">
        <!-- 严重漏洞 直接输出未转义的用户内容 存储型xss漏洞触发 -->
        <h3>作者：<%= comment.getAuthor() %></h3>
        <div class="content"><%= comment.getContent() %></div>
    </div>
<%
}
%>
```
已修复代码：
后端代码
```java
// 后端代码：使用Spring Boot的修复示例
@RestController
public class CommentController {
    
    @PostMapping("/addComment")
    public String addComment(@RequestParam String content, 
                            @RequestParam String author) {
        // Spring会自动进行一些基本防护，但建议额外处理
        String safeContent = HtmlUtils.htmlEscape(content);
        String safeAuthor = HtmlUtils.htmlEscape(author);
        
        Comment comment = new Comment(safeContent, safeAuthor, new Date());
        commentService.save(comment);
        
        return "redirect:/comments";
    }
}

```
前端代码
```html
<!-- Thymeleaf模板自动转义 -->
<div th:each="comment : ${comments}">
    <h3>作者：<span th:text="${comment.author}">默认作者</span></h3>
    <div th:text="${comment.content}">默认内容</div>
</div>
```
#### 场景2：用户昵称和签名存储型XSS
```java
// 用户实体类 用户昵称和签名存储型XSS
public class User {
    private String nickname;  // 昵称
    private String signature; // 个性签名
    private String bio;       // 个人简介
    
    // 漏洞：setter方法直接赋值，无过滤
    public void setNickname(String nickname) {
        this.nickname = nickname; // 直接设置，危险！
    }
    
    public void setSignature(String signature) {
        this.signature = signature;
    }
}

// 用户服务类
@Service
public class UserService {
    
    public void updateUserProfile(User user) {
        // 使用字符串拼接SQL 此处处理存储型xss 还有sql注入风险
        String sql = "UPDATE users SET nickname = '" + user.getNickname() 
                   + "', signature = '" + user.getSignature() 
                   + "' WHERE id = " + user.getId();
        
        jdbcTemplate.update(sql); // 直接执行
    }
    
    public User getUserById(int id) {
        String sql = "SELECT * FROM users WHERE id = " + id;
        // 查询并返回用户...
        return user;
    }
}
```

### 修复建议描述
### 人工审计建议
## XXX
### 概念
### 危害
### 典型漏洞场景
### 修复建议描述
### 人工审计建议

感谢阅读，㊗️前程似锦！！！
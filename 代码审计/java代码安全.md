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

#### 场景7:
### 修复建议描述
## XXX
### 概念
### 危害
### 典型漏洞场景
### 修复示例代码
### 修复建议描述
## XXX
### 概念
### 危害
### 典型漏洞场景
### 修复示例代码
### 修复建议描述
感谢阅读，㊗️前程似锦！！！
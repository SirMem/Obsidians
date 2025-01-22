---
Project: "[[SpringBoot]]"
Status: 
tags:
  - Resources
Deadline: 
CreateTime: 2024-11-13
Connected:
---

#review

# Jwt(Auth) Utils API

## Create
根据key生成的算法，各个特征，以及签发过期时间生成Jwt token
```java
import com.auth0.jwt.JWT;
import com.auth0.jwt.algorithms.Algorithm;
import java.util.Date;

public class JwtUtil {

    private String secretKey = "yourSecretKey"; // 密钥

    // 使用 java-jwt 库生成 JWT
    public String generateToken(String subject,int id) {
        Algorithm algorithm = Algorithm.HMAC256(secretKey);  // 签名算法和密钥
        return JWT.create()  // 创建JWT构建器
		        .withJWTId(UUID.randomUUID().toString()) // 设置JWT的唯一标识符
                .withSubject(subject)  // 设置JWT的主题（通常是用户ID）
                .withChaim("id",id) //添加任意自定义声明，可以是任意类型的数据
                .withIssuedAt(new Date())  // 设置JWT的签发时间
                .withExpiresAt(new Date(System.currentTimeMillis() + 1000 * 60 * 60))  // 设置JWT的过期时间
                .sign(algorithm);  // 签名
    }
}

```

## JWTVerifier

`JWTVerifier` 是用于验证 JWT 的工具类。通过 `JWTVerifier`，你可以验证 JWT 是否有效、是否过期、是否被篡改等。

```java
import com.auth0.jwt.JWT;
import com.auth0.jwt.algorithms.Algorithm;
import com.auth0.jwt.JWTVerifier;
import com.auth0.jwt.interfaces.DecodedJWT;

public class JwtUtil {

    private String secretKey = "yourSecretKey"; // 密钥

    // 创建JWTVerifier并验证Token
    public DecodedJWT verifyToken(String token) {
        Algorithm algorithm = Algorithm.HMAC256(secretKey);  // 使用与生成JWT时相同的密钥和算法
        JWTVerifier verifier = JWT.require(algorithm)
                                  .withIssuer("auth0")  // 可选，设置期望的发行者
                                  .build();  // 创建JWTVerifier实例
        return verifier.verify(token);  // 验证并返回DecodedJWT对象
    }
}

```

- `verify(String token)`：验证一个 JWT Token 并返回一个 `DecodedJWT` 实例。验证过程包括验证签名、过期时间等。
- `withIssuer(String issuer)`：设置预期的发行人（Issuer）。验证时会检查 JWT 的 `iss` 声明是否与提供的值匹配。
- `withAudience(String audience)`：设置预期的受众（Audience）。验证时会检查 JWT 的 `aud` 声明是否与提供的值匹配。
- `withSubject(String subject)`：设置预期的主题（Subject）。验证时会检查 JWT 的 `sub` 声明是否与提供的值匹配。
- `withClaim(String claimName, Object claimValue)`：验证 JWT 中是否包含特定的声明，并检查该声明的值。
## DecodeJWT

`DecodedJWT` 是用来存储和访问解析后的 JWT 内容的类。当你使用 `JWTVerifier` 验证成功后，会得到一个 `DecodedJWT` 实例，它包含了 JWT 的所有信息，比如 `sub`（主题）、`exp`（过期时间）、自定义声明等。

```java
import com.auth0.jwt.interfaces.DecodedJWT;

public class JwtUtil {

    // 从DecodedJWT中提取数据
    public void extractClaims(DecodedJWT decodedJWT) {
        String subject = decodedJWT.getSubject();  // 获取主题
        String issuer = decodedJWT.getIssuer();  // 获取发行者
        Date expiresAt = decodedJWT.getExpiresAt();  // 获取过期时间
        String id = decodedJWT.getId();  // 获取JWT ID

        // 获取自定义声明
        Map<String, chaims> chaims = decodedJWT.getClaims();
        Integer idClaim = claims.get("id").asInt();
        
        String name = decodedJWT.getClaim("name").asString();
        Integer idClaim = decodedJWT.getClaim("id").asInt();

        System.out.println("Subject: " + subject);
        System.out.println("Issuer: " + issuer);
        System.out.println("ExpiresAt: " + expiresAt);
        System.out.println("ID Claim: " + idClaim);
    }
}

```

- `getSubject()`：获取 JWT 的主题（`sub` 声明）。
- `getIssuer()`：获取 JWT 的发行者（`iss` 声明）。
- `getAudience()`：获取 JWT 的受众（`aud` 声明）。
- `getClaim(String name)`：获取 JWT 中特定声明的值。返回一个 `Claim` 对象，可以通过 `Claim.asString()`、`Claim.asInt()` 等方法来获取声明的具体值。
- `getClaims` 返回一个 `Map<String, Claim>`，表示所有的 JWT 声明。
- `getExpiresAt()`：获取 JWT 的过期时间（`exp` 声明）。
- `getIssuedAt()`：获取 JWT 的签发时间（`iat` 声明）。
- `getId()`：获取 JWT 的唯一标识符（`jti` 声明）。
- `getToken()`：获取原始的 JWT 字符串。

## `JWTVerifier` 和 `DecodedJWT` 的协作：

1. **创建 `JWTVerifier`**：首先，你需要使用 `JWT.require()` 方法创建一个 `JWTVerifier` 实例，并设置验证规则（如预期的发行者、受众、声明等）。
2. **验证 JWT**：然后，使用 `verifier.verify(token)` 方法验证传入的 JWT Token。如果验证通过，它会返回一个 `DecodedJWT` 实例。
3. **从 `DecodedJWT` 中提取数据**：验证成功后，你可以通过 `DecodedJWT` 实例访问 JWT 中的所有声明信息，如 `sub`、`exp` 等。


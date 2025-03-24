#Title: Implementing Multi-Factor Authentication (MFA) in Spring Boot with Spring Security

Introduction In today’s digital world, securing user accounts is more critical than ever. Multi-Factor Authentication (MFA) provides an extra layer of security by requiring users to verify their identity using multiple methods, such as passwords and one-time passwords (OTPs). In this article, we will explore how to implement MFA in a Spring Boot application using Spring Security.

1. Understanding MFA
Multi-Factor Authentication (MFA) enhances security by requiring at least two of the following authentication factors:
•	Something You Know – A password or PIN.
•	Something You Have – A mobile device, security key, or authentication app.
•	Something You Are – Biometrics like fingerprints or facial recognition.

MFA prevents unauthorized access even if credentials are compromised.

3. Setting Up Spring Boot and Spring Security
Start by setting up a Spring Boot project with the necessary dependencies in pom.xml:
```
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-security</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-jpa</artifactId>
</dependency>
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <scope>runtime</scope>
</dependency>
```

Configure Spring Security in SecurityConfig.java:

```
@EnableWebSecurity
public class SecurityConfig {
    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
        http
            .csrf().disable()
            .authorizeHttpRequests(auth -> auth
                .requestMatchers("/api/auth/**").permitAll()
                .anyRequest().authenticated()
            )
            .formLogin()
            .and()
            .httpBasic();
        return http.build();
    }
}
```

3. Implementing MFA with OTP

Step 1: User Entity with MFA Fields

```
@Entity
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String username;
    private String password;
    private boolean isTwoFactorEnabled;
    private String secretKey;
    // Getters and Setters
}
```

Step 2: Generate and Validate OTPs

Use the Time-Based One-Time Password (TOTP) algorithm:

```
public class OTPUtil {
    public static String generateSecretKey() {
        return Base32.encode(randomBytes(10));
    }
    public static String generateTOTP(String secretKey) {
        return TOTP.getOTP(secretKey);
    }
}
```

Step 3: Implement MFA in Login Flow

```
@PostMapping("/login")
public ResponseEntity<?> login(@RequestBody LoginRequest request) {
    Authentication authentication = authenticationManager.authenticate(
            new UsernamePasswordAuthenticationToken(request.getUsername(), request.getPassword()));
    User user = userRepository.findByUsername(request.getUsername()).orElseThrow();
    if (user.isTwoFactorEnabled()) {
        return ResponseEntity.ok("Enter OTP sent to your email or authenticator app");
    }
    String token = jwtTokenProvider.generateToken(authentication);
    return ResponseEntity.ok(new AuthResponse(token));
}
```

Step 4: Verify OTP Before Granting Access

```
@PostMapping("/verify-otp")
public ResponseEntity<?> verifyOtp(@RequestBody OtpRequest otpRequest) {
    User user = userRepository.findByUsername(otpRequest.getUsername()).orElseThrow();
    if (!OTPUtil.verifyOTP(user.getSecretKey(), otpRequest.getOtp())) {
        return ResponseEntity.status(HttpStatus.UNAUTHORIZED).body("Invalid OTP");
    }
    String token = jwtTokenProvider.generateToken(user.getUsername());
    return ResponseEntity.ok(new AuthResponse(token));
}
```

4. Enhancing MFA Security
•	Use Twilio API for SMS-based OTPs.
•	Integrate with Google Authenticator.
•	Store and encrypt MFA preferences securely.
•	Use JWT for secure session management.

Conclusion

Implementing MFA in Spring Boot enhances security and protects user accounts from unauthorized access. By combining Spring Security with OTP-based authentication, we can build a secure and scalable authentication system.
Would you like to see additional features like biometric authentication or WebAuthn integration? Let me know in the comments!

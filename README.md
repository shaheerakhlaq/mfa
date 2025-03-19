# Spring Boot Multi-Factor Authentication (MFA) Example 🔒

[![Java](https://img.shields.io/badge/Java-17%2B-blue)](https://www.oracle.com/java/)
[![Spring Boot](https://img.shields.io/badge/Spring%20Boot-3.1.0-brightgreen)](https://spring.io/projects/spring-boot)
[![License](https://img.shields.io/badge/License-MIT-green)](https://opensource.org/licenses/MIT)

A step-by-step implementation of **Time-Based One-Time Password (TOTP)** MFA using Spring Security and Google Authenticator.

---

## Features ✨
- **TOTP-based MFA** using Google Authenticator/Authy
- QR code generation for MFA setup
- Session-based MFA verification flow
- Spring Security integration
- H2 database (for development)

---

## Prerequisites 📋
- Java 17+
- Maven 3.8+
- IDE (IntelliJ, Eclipse, or VS Code)
- TOTP Authenticator App (Google Authenticator, Authy, etc.)

---

## Technologies Used 🛠️
- **Spring Boot 3.1**
- **Spring Security**
- GoogleAuth Library (`com.warrenstrange:googleauth`)
- Thymeleaf (for templates)
- H2 Database (in-memory)

---

## Installation & Setup 🚀

### 1. Clone the Repository
```bash
git clone https://github.com/shaheerakhlaq/mfa.git
cd mfa

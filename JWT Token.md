## Spring Security 기반 인증 구조

### 1.SecurityConfig(시큐리티 설정 클래스)
- HTTP 요청 필터 체인을 설정
- 어떤 요청이 인증 필요/불필요한지 설정
- JWT 필터 등록

```java
@Configuration
@EnableWebSecurity
public class SecurityConfig {

    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
        http.csrf().disable()
            .authorizeHttpRequests(auth -> auth
                .requestMatchers("/auth/**").permitAll()
                .anyRequest().authenticated()
            )
            .addFilterBefore(jwtTokenFilter, UsernamePasswordAuthenticationFilter.class);

        return http.build();
    }
}
```

### 2.UserDetailsService 구현체
- 시큐리티 인증에서 사용자 정보 로드 역할
- loadUserByUsername() 구현 ->  DB에서 사용자 조회 및 UserDetails 반환
- 클라이언트가 로그인 요청을 하면 AuthenticationManager는 UserDetailsService 호출

### 3.PasswordEncoder
- 비밀번호 암호화를 위해 사용(ex.BCryptPasswordEncoder)
- 저장된 해시 값과 요청 값 비교

### 4.AuthService
- 로그인 요청 시 사용자 인증 및 JWT 토큰 생성
- 인증 성공 시 JwtTokenProvider로 토큰 발급

### 5.JwtTokenProvider
- JWT 토큰 생성 및 검증 담당

### 6.JwtTokenFilter
- HTTP 요청마다 실행되는 필터
- 토큰 유효성 검사 및 토큰 추출
- 인증 객체를 SecurityContext에 저장 → 이후 컨트롤러에서 @AuthenticationPrincipal로 접근 가능


[참고]
https://docs.spring.io/spring-security/reference/api/java/org/springframework/security/core/userdetails/UserDetailsService.html(https://docs.spring.io/spring-security/reference/api/java/org/springframework/security/core/userdetails/UserDetailsService.html)

# Spring Security v2.7.0 이상 filter chain에서 path 제외하기

## Config

    @Bean
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception{


        http
                .httpBasic(HttpBasicConfigurer::disable)
                .cors(corsConfigurationSource -> corsConfigurationSource.configurationSource(corsConfigurationSource()))
                .csrf(AbstractHttpConfigurer::disable)

                // disable session
                .sessionManagement((sessionManagement) ->
                        sessionManagement.sessionCreationPolicy(SessionCreationPolicy.STATELESS)
                )
                .authorizeHttpRequests(authorizeRequest ->
                        authorizeRequest
                                .requestMatchers("/api/login/**").permitAll()
                                .requestMatchers("/api/token/reissue/**").permitAll()
                                .requestMatchers("/login/oauth2/code/**").permitAll()
                                .requestMatchers("/login/oauth2/redirect/**").permitAll()
                                .requestMatchers("/login/**").permitAll()
                                .requestMatchers("/test/**").permitAll()
                                .requestMatchers("/favicon.ico").permitAll()
                                .requestMatchers("/swagger-ui/**").permitAll()
                                .requestMatchers("/api-docs/**").permitAll()
//                                .requestMatchers(new AntPathRequestMatcher("/api/token/**")).permitAll()
                                .requestMatchers("/error/**").permitAll()
                                .anyRequest().authenticated()
                )

                .exceptionHandling(handling -> handling.authenticationEntryPoint(authEntryPointJwt));


        http.addFilterBefore(new AuthTokenFilter(jwtUtils, customUserDetailsService, tokenService), UsernamePasswordAuthenticationFilter.class); // 여기서 jwt 인증
        http.addFilterBefore(jwtExceptionHandlerFilter, AuthTokenFilter.class); // jwt 예외처리 필터

        return http.build();

    }

## Filter(OncePerRequestFilter)

@Slf4j
@RequiredArgsConstructor
//@Component // 아 근데 빈등록해도 돌아가던데...흠ㅎ므
public class AuthTokenFilter extends OncePerRequestFilter {

    private final JwtUtils jwtUtils;
    private final CustomUserDetailsService customUserDetailsService;
    private final TokenService tokenService;
    private static final String BEARER = "Bearer ";


    @Override
    protected void doFilterInternal(HttpServletRequest request, HttpServletResponse response, FilterChain filterChain) throws ServletException, IOException {

        log.info("======= Request Path =========");
        String requestURI = request.getRequestURI();
        log.info("======= " + requestURI+" =======");

        checkToken(request);

        // antMatcher + 정규식으로 표현해줘야함, return 던져버리기

        filterChain.doFilter(request, response);
    }

    @Override
    protected boolean shouldNotFilter(HttpServletRequest request) throws ServletException {

        String[] excludePath = {
                "/api/login/kakao",
                "/api/token/reissue/access",
                "/api/token/reissue/refresh",
                "/login/oauth2/redirect",
                "/test/login",
                "/favicon.ico",
                "/swagger-ui/index.html",
                "/swagger-ui/favicon-16x16.png",
                "/swagger-ui/favicon-32x32.png",
                "/swagger-ui/swagger-initializer.js",
                "/swagger-ui/swagger-ui-standalone-preset.js",
                "/swagger-ui/swagger-ui-bundle.js",
                "/swagger-ui/index.css",
                "/swagger-ui/swagger-ui.css",
                "/api-docs/swagger-config",
                "/api-docs/YEAR%20API",
                "/error"


        };

        String path = request.getRequestURI();

        return Arrays.stream(excludePath).anyMatch(path::startsWith);

    }
}

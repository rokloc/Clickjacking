# クリックジャッキング
攻撃者が、攻撃者のサイトに正規サイトをiframe等で読み込んでログイン済みセッションを利用して透明なボタンを押させるなどして不正なリクエストをさせる攻撃

# 対策の心得
対策の本質は 攻撃者が自サイトから正規サイトを iframe 等で読み込めないようにすること

# 対策
## X-Frame-Optionsを使う（古め）
SpringSecurityではデフォルトでレスポンスに以下のヘッダが付与される
```
X-Frame-Options: DENY
```
結果そのサイトはiframeに一切埋め込めなくさせる事ができる

## CSP (Content-Security-Policy)を設定する（新しい）
```
//SecurityConfig
@Configuration
public class SecurityConfig {

    @Bean
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
        http
          // 他のセキュリティ設定もここに書く

          // クリックジャッキング対策の設定
          .headers(headers -> headers
              // iframe埋め込みを全面禁止（全サイトからのiframeを拒否）（SpringSecurityデフォルトなので記述不要）
              .frameOptions(frame -> frame.deny())
              // さらにCSPで同じオリジンからのiframe埋め込みのみ許可（不要かもだけどこの実装が推奨されている）
              .contentSecurityPolicy(csp -> csp
                  .policyDirectives("frame-ancestors 'self';")
              )
          );

        return http.build();
    }
}
```
<img width="1218" height="214" alt="スクリーンショット (18)" src="https://github.com/user-attachments/assets/f17ee64a-af4e-43ea-a889-1fc72b3f99d0" />








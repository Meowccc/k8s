#Oauth2 - Microsoft


##流程

> ###取得授權碼
>```http request
>https://login.microsoftonline.com/b50943ec-d112-4dc3-9260-815ee50a29c1/oauth2/v2.0/authorize?
>    client_id={{client_id}}
>    &response_type=code
>    &redirect_uri={{redirect_url}}
>    &response_mode=query
>    &scope=openid%20offline_access%20https%3A%2F%2Fgraph.microsoft.com%2Fmail.read
>    &state={{stage}}
>```
> * client_id
> * response_type
> * redirect_uri
> * response_mode
> * state
> ### Example:
>```http request
>https://login.microsoftonline.com/b50943ec-d112-4dc3-9260-815ee50a29c1/oauth2/v2.0/authorize?
>    client_id={{client_id}}
>    &response_type=code
>    &redirect_uri={{redirect_url}}
>    &response_mode=query
>    &scope=openid%20offline_access%20https%3A%2F%2Fgraph.microsoft.com%2Fmail.read
>    &state={{stage}}
>```
> ###回傳結果   (轉跳uri)
> 
>```http request
>https://tw.yahoo.com/?
>    code=0.AAAA7EMJtRLRw02SYIFe5QopwQZtbXPKKUtGpE90GN9Lm3pKAJ4.AQABAAIAAAD--DLA3VO7QrddgJg7WevrVXEUIi8VIDPlDyFzUKpPcs_-fx6BTe8q-Mjst3y3YLaOUL8MR57O1TfYe535J0CnA3KiuL4zhEBAnqFYcDvwRMYNiz3RA6-4HG7-TgV7hxreVjtC_ssEILz-18MCrRDOZIe9DoAYiUhwYLy9MiQp-cibKTDcR54-PwCK15C53MEbfw3ZUQ7-00m1DgcBFb2CaHWTv8zzHcua7KEelxpqjoqq7FUuNW1Lh9zMs_ekew3ub1zF_tT-9VnFQGzsCUCwMiV6tFYkql6kNSz9u7gKktL3YjLrdlPTry-VNWuGEpP8ED6GrLDJ8WoQ3UvTUrgg5yF0AHDAtEDeK4nF3nOD5umPzMxGI4PlEKgfo1j49W9SuDa7bKFituFRLGrGbZtLvzgcnalwjDNGUru8BSx7UbR6YUlJcs5mz1V-pT6aQ8NIAfRFlIpLb3xDuQrrvLQ9a2LgA4XI5t1pkfwzmbhcQZ7NNzzc9zjPbxzGk0qoCZZzScC1xW4xC07cBJz0T80oCSO4harRp61r2IzsogLKEuyrBP-A1SpTpjbB-ISMQmKDcEbZ40a5SEe4WROT3tAeXl7PlnrzACoHcWvsFThRJ9k7Z7TO-JVNMugm459I1e2Db67oUkAar2Y4dmsvDsglLFM8-Ia0KAAGqUbhvfpCJAQV_i5l88WxaUX3LaSt52_9gy8aHuqN61dJN21Xd1GHtwYf-cdjWkZlY0mMHBK1AQtl8s2lqfH23dMeKP2v3wFX7EVZuMRsL0mQJ-OXdrAaCwC08-BAHGOxYGseX3U66EWHHo-YEUVO-AgybFcHZbzUZx0YBKpYSEgNStTCbz_-LW6VQN5GNzI9EiJe_zhSdAVWV73U_XyUYlQocXDTL8chbK_unO4lc_J6SpJ27xA-c2grEFdgCGw96RmxEF0mmJNvYpagWRPoHeWeJ6_eqc1haRTjyIfkWM7bCIv0W58hW9Dt-sD3hwE8n5JjkPpJ1g_ZaRGR58Y3cdPwz9DOePuYodm5XUG9299ah8wT19DkA37KQbuqh9Ks14N9-5AjaGxWuHgn4q-thmG96P65mmu1dsOZKEHknU9U2f4RyzsSFkVoQjlYdi6d796pP_SlQtxPEUrL7WlkW1ZqXBgBMsLhnTr5VoMtN-W56QgsUsYcWc_tDarXa1P397kM_7OOMFHOZq-cTM0gA-bs0m5QLbloUxlo9mijWc8G0lyaLBP64qJo7BJ6ONZrChbdf6HKV7NLjzLSJjQGiVA_7wu_xikgAA
>    &state=12345
>    &session_state=ca138548-50cd-499a-8ed4-43ec21a6a406#
>```
> * code : 取得AccessToken的code
> * state : 驗證使用
> ---
> 
>> ###取得AccessToken
>```http request
>https://login.microsoftonline.com/b50943ec-d112-4dc3-9260-815ee50a29c1/oauth2/v2.0/authorize?
>    client_id={{client_id}}
>    &response_type=code%20id_token
>    &redirect_uri={{redirect_uri}}
>    &scope=openid%20https://graph.microsoft.com/Mail.Read&state=12345
>    &nonce={{nonce}}
>    &code_challenge={{client_secret}}
>    &code_challenge_method=S256
>```
> * client_id
> * response_type
> * redirect_uri
> * scope
> * nonce
> * code_challenge
> * code_challenge_method
> 
> ### Example:
>```http request
>https://login.microsoftonline.com/b50943ec-d112-4dc3-9260-815ee50a29c1/oauth2/v2.0/authorize?
>    client_id=736d6d06-29ca-464b-a44f-7418df4b9b7a
>    &response_type=code%20id_token
>    &redirect_uri=https%3A%2F%2Ftw%2Eyahoo%2Ecom&response_mode=fragment
>    &scope=openid%20https://graph.microsoft.com/Mail.Read&state=12345
>    &nonce=abcde
>    &code_challenge=kw4NkFwmeNdygVX84Rm89D4xbOjj3wz2HgDLZuNuoJg
>    &code_challenge_method=S256
>```
>
> 
> ---
>> ### 混合式


## 工具
- [Parse JWT](https://jwt.io/)
- [Generate PKCE](https://tonyxu-io.github.io/pkce-generator/)
- [Checkout Error Code](https://login.microsoftonline.com/error)


## 參考資料

- [What's PKCE](https://waynechu.cc/posts/298-what-is-pkce-proof-key-for-code-exchange)
- [OAuth2.0 PKCE](https://tonyxu.io/zh/posts/2018/oauth2-pkce-flow/)
- [OAuth3.0 Authorization Code](https://blog.yorkxin.org/posts/oauth2-4-1-auth-code-grant-flow.html)

## 聯絡作者

- [信箱](mailto:poshen0322@gmail.com)
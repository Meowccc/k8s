#Oauth2 - Microsoft (Client Login)


##流程
1. [取得授權碼](#no.1)
2. [取得AccessToken](#no.2)
3. [混合式](#no.3)





> ###<span id = "no.1">取得授權碼</span>
>```http request
>-X GET
>https://login.microsoftonline.com/{{tenant}}/oauth2/v2.0/authorize?
>    client_id={{client_id}}
>    &response_type=code
>    &redirect_uri={{redirect_url}}
>    &response_mode=query
>    &scope=openid%20offline_access%20https%3A%2F%2Fgraph.microsoft.com%2Fmail.read
>    &state={{stage}}
>```
> * tenant 租戶識別碼
> * client_id 用戶端識別碼
> * response_type 回傳的內容
> * redirect_uri 轉跳路徑
> * response_mode 
> * state
> 
> 
> #### Example:
>```http request
>-X GET
>https://login.microsoftonline.com/b50943ec-d112-4dc3-9260-815ee50a29c1/oauth2/v2.0/authorize?
>    client_id=736d6d06-29ca-464b-a44f-7418df4b9b7a
>    &response_type=code
>    &redirect_uri=https%3A%2F%2Ftw%2Eyahoo%2Ecom
>    &response_mode=query
>    &scope=openid%20offline_access%20https%3A%2F%2Fgraph.microsoft.com%2Fmail.read
>    &state=12345
>```
>
>
> ####回傳結果   (redirect uri)
> 
>```http request
>https://tw.yahoo.com/?
>    code=0.AAAA7EMJtRLRw02SYIFe5QopwQZtbXPKKUtGpE90GN9Lm3pKAJ4.AQABAAIAAAD--DLA3VO7QrddgJg7WevrVXEUIi8VIDPlDyFzUKpPcs_-fx6BTe8q-Mjst3y3YLaOUL8MR57O1TfYe535J0CnA3KiuL4zhEBAnqFYcDvwRMYNiz3RA6-4HG7-TgV7hxreVjtC_ssEILz-18MCrRDOZIe9DoAYiUhwYLy9MiQp-cibKTDcR54-PwCK15C53MEbfw3ZUQ7-00m1DgcBFb2CaHWTv8zzHcua7KEelxpqjoqq7FUuNW1Lh9zMs_ekew3ub1zF_tT-9VnFQGzsCUCwMiV6tFYkql6kNSz9u7gKktL3YjLrdlPTry-VNWuGEpP8ED6GrLDJ8WoQ3UvTUrgg5yF0AHDAtEDeK4nF3nOD5umPzMxGI4PlEKgfo1j49W9SuDa7bKFituFRLGrGbZtLvzgcnalwjDNGUru8BSx7UbR6YUlJcs5mz1V-pT6aQ8NIAfRFlIpLb3xDuQrrvLQ9a2LgA4XI5t1pkfwzmbhcQZ7NNzzc9zjPbxzGk0qoCZZzScC1xW4xC07cBJz0T80oCSO4harRp61r2IzsogLKEuyrBP-A1SpTpjbB-ISMQmKDcEbZ40a5SEe4WROT3tAeXl7PlnrzACoHcWvsFThRJ9k7Z7TO-JVNMugm459I1e2Db67oUkAar2Y4dmsvDsglLFM8-Ia0KAAGqUbhvfpCJAQV_i5l88WxaUX3LaSt52_9gy8aHuqN61dJN21Xd1GHtwYf-cdjWkZlY0mMHBK1AQtl8s2lqfH23dMeKP2v3wFX7EVZuMRsL0mQJ-OXdrAaCwC08-BAHGOxYGseX3U66EWHHo-YEUVO-AgybFcHZbzUZx0YBKpYSEgNStTCbz_-LW6VQN5GNzI9EiJe_zhSdAVWV73U_XyUYlQocXDTL8chbK_unO4lc_J6SpJ27xA-c2grEFdgCGw96RmxEF0mmJNvYpagWRPoHeWeJ6_eqc1haRTjyIfkWM7bCIv0W58hW9Dt-sD3hwE8n5JjkPpJ1g_ZaRGR58Y3cdPwz9DOePuYodm5XUG9299ah8wT19DkA37KQbuqh9Ks14N9-5AjaGxWuHgn4q-thmG96P65mmu1dsOZKEHknU9U2f4RyzsSFkVoQjlYdi6d796pP_SlQtxPEUrL7WlkW1ZqXBgBMsLhnTr5VoMtN-W56QgsUsYcWc_tDarXa1P397kM_7OOMFHOZq-cTM0gA-bs0m5QLbloUxlo9mijWc8G0lyaLBP64qJo7BJ6ONZrChbdf6HKV7NLjzLSJjQGiVA_7wu_xikgAA
>    &state=12345
>    &session_state=ca138548-50cd-499a-8ed4-43ec21a6a406#
>```
> * code : [取得AccessToken的code](#code)
> * state : 驗證使用
> ---
> 
> 
> 
> ###<span id = "no.2">取得AccessToken</span>
>```http request
>-X POST
>-H 'Content-Type: multipart/form-data'
>-d '{
>    "client_id"={{client_id}}
>    "scope"={{scope}}
>    "code":{{code}},
>    "redirect_uri"={{redirect_uri}}
>    "grant_type"="authorization_code"
>    "code_verifier"={{code_verifier}}
>    "client_secret"={{client_secret}}
>}'
>https://login.microsoftonline.com/{{tenant}}/oauth2/v2.0/token
>```
> * tenant
> * client_id
> * scope
> * code : <span id = "code">使用第一段取得授權碼的 code</span>
> * redirect_uri 轉跳的路徑
> * code_verifier
> * client_secret
> 
> 
> 
> #### Example:
>```http request
>-X POST
>-H 'Content-Type: multipart/form-data'
>-d '{
>    "client_id"="736d6d06-29ca-464b-a44f-7418df4b9b7a"
>    "scope"="https://graph.microsoft.com/Mail.Read"
>    "code":{{code}},
>    "redirect_uri"="https://tw.yahoo.com"
>    "grant_type"="authorization_code"
>    "code_verifier"="kw4NkFwmeNdygVX84Rm89D4xbOjj3wz2HgDLZuNuoJg"
>    "client_secret"="OltFIJ50LQ3UL15-_mPE.WjdM0tSdiOO--"
>}'
>https://login.microsoftonline.com/b50943ec-d112-4dc3-9260-815ee50a29c1/oauth2/v2.0/token
>```
>
>
> #### Response Sample : 
>```json
>{
>    "token_type": "Bearer",
>    "scope": "email openid profile https://graph.microsoft.com/Mail.Read https://graph.microsoft.com/Mail.Read.Shared https://graph.microsoft.com/Mail.ReadBasic https://graph.microsoft.com/Mail.ReadWrite https://graph.microsoft.com/Mail.ReadWrite.Shared https://graph.microsoft.com/Mail.Send https://graph.microsoft.com/Mail.Send.Shared https://graph.microsoft.com/User.Read",
>    "expires_in": 3599,
>    "ext_expires_in": 3599,
>    "access_token": "eyJ0eXAiOiJKV1QiLCJub25jZSI6IkU3WlZ2RW5TRHNna3NoV1V5MU1XMF8wMVVsRG15dGJrS1IyOXA5ZDlyR1kiLCJhbGciOiJSUzI1NiIsIng1dCI6Im5PbzNaRHJPRFhFSzFqS1doWHNsSFJfS1hFZyIsImtpZCI6Im5PbzNaRHJPRFhFSzFqS1doWHNsSFJfS1hFZyJ9.eyJhdWQiOiJodHRwczovL2dyYXBoLm1pY3Jvc29mdC5jb20iLCJpc3MiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC9iNTA5NDNlYy1kMTEyLTRkYzMtOTI2MC04MTVlZTUwYTI5YzEvIiwiaWF0IjoxNjE1MzU4MzQ4LCJuYmYiOjE2MTUzNTgzNDgsImV4cCI6MTYxNTM2MjI0OCwiYWNjdCI6MCwiYWNyIjoiMSIsImFjcnMiOlsidXJuOnVzZXI6cmVnaXN0ZXJzZWN1cml0eWluZm8iLCJ1cm46bWljcm9zb2Z0OnJlcTEiLCJ1cm46bWljcm9zb2Z0OnJlcTIiLCJ1cm46bWljcm9zb2Z0OnJlcTMiLCJjMSIsImMyIiwiYzMiLCJjNCIsImM1IiwiYzYiLCJjNyIsImM4IiwiYzkiLCJjMTAiLCJjMTEiLCJjMTIiLCJjMTMiLCJjMTQiLCJjMTUiLCJjMTYiLCJjMTciLCJjMTgiLCJjMTkiLCJjMjAiLCJjMjEiLCJjMjIiLCJjMjMiLCJjMjQiLCJjMjUiXSwiYWlvIjoiQVVRQXUvOFRBQUFBUFVUYVRYWktPa1pmdm9GM0tjbHF6azdTUHZGMGREcHZuMUFKaDhaRGxPWGpGRWg3cDBPcHBqa2Q5dTBDZUoxZVk5Yko0U0RUVG1EanhWZEdsRUNoWmc9PSIsImFsdHNlY2lkIjoiMTpsaXZlLmNvbTowMDAzQkZGREU1Q0E2OTIzIiwiYW1yIjpbInB3ZCJdLCJhcHBfZGlzcGxheW5hbWUiOiJ0ZXN0LWFwcCIsImFwcGlkIjoiNzM2ZDZkMDYtMjljYS00NjRiLWE0NGYtNzQxOGRmNGI5YjdhIiwiYXBwaWRhY3IiOiIxIiwiZW1haWwiOiJkMTAzMTkxMTVAZ21haWwuY29tIiwiZmFtaWx5X25hbWUiOiLmnpciLCJnaXZlbl9uYW1lIjoiZDEwMzE5MTE1QGdtYWlsLmNvbSIsImlkcCI6ImxpdmUuY29tIiwiaWR0eXAiOiJ1c2VyIiwiaXBhZGRyIjoiMTE0LjM0LjE3My4yMDkiLCJuYW1lIjoiZDEwMzE5MTE1QGdtYWlsLmNvbSDmnpciLCJvaWQiOiIxNWMxMjBlZi04YWVhLTQ1OWQtOTkwMS01NDg0M2E2YTFjODUiLCJwbGF0ZiI6IjMiLCJwdWlkIjoiMTAwMzIwMDBBQjcyNTc3MyIsInJoIjoiMC5BQUFBN0VNSnRSTFJ3MDJTWUlGZTVRb3B3UVp0YlhQS0tVdEdwRTkwR045TG0zcEtBSjQuIiwic2NwIjoiZW1haWwgTWFpbC5SZWFkIE1haWwuUmVhZC5TaGFyZWQgTWFpbC5SZWFkQmFzaWMgTWFpbC5SZWFkV3JpdGUgTWFpbC5SZWFkV3JpdGUuU2hhcmVkIE1haWwuU2VuZCBNYWlsLlNlbmQuU2hhcmVkIG9wZW5pZCBwcm9maWxlIFVzZXIuUmVhZCIsInN1YiI6Ii1kb2laUnhxaEJIeTVqTXlwblgta2gyYUF3eERZamRncmVLZ0N0eHNpODAiLCJ0ZW5hbnRfcmVnaW9uX3Njb3BlIjoiQVMiLCJ0aWQiOiJiNTA5NDNlYy1kMTEyLTRkYzMtOTI2MC04MTVlZTUwYTI5YzEiLCJ1bmlxdWVfbmFtZSI6ImxpdmUuY29tI2QxMDMxOTExNUBnbWFpbC5jb20iLCJ1dGkiOiJYN3oxeXJXUVowYVBIbEJ2blA4UUFBIiwidmVyIjoiMS4wIiwid2lkcyI6WyI2MmU5MDM5NC02OWY1LTQyMzctOTE5MC0wMTIxNzcxNDVlMTAiLCJiNzlmYmY0ZC0zZWY5LTQ2ODktODE0My03NmIxOTRlODU1MDkiXSwieG1zX3N0Ijp7InN1YiI6IlpRd2xNNC01N3JjYkxwZ0haQUlkTXdOaUpxUEpBc04tcWtUbmRJWDFzREUifSwieG1zX3RjZHQiOjE1ODU5Njg5NDF9.VF-b97HbCITEPl4r8EJMRDvWfkLoqcw3rt340s1ShU1fiFeCqOHVW-AaKwMXekh9btbxYC5G2PJvW0hkC643VCKfKGFT5KTugv7wEz_9ra7YWGTamuYcu10MENVdPmWm_Fe5mfhGMx9ld5pjnuOelkvAv_x99c81hw1j94s0Vr-T-E_uOpac5p_yUXklfN_0Pl4XdT57fSTg0UlQleY-40EJ014byNpFlIMBaLFFP3s7MeSD7hWboVAWZccib0c09n5JCYOxB0eSXQ5o0HRZE5G7yH2h-IKOiQXx90-ceixmALLVT3TAymxxv4nn_djq4hlIAXZfFhTiUAHznD3DJw",
>    "refresh_token": "0.AAAA7EMJtRLRw02SYIFe5QopwQZtbXPKKUtGpE90GN9Lm3pKAJ4.AgABAAAAAAD--DLA3VO7QrddgJg7WevrAgDs_wQA9P-Y6mpCrLsaVuO4Frv_xU2mtQHI1DNzfv9YNMLNKGcKzFYiJvsXRowI3Nt9VHRhjzss2y4uR42eQWipcyEYWuZ75QIAh4QvbglqSvo95hMXN0qNpPvsLtWCjqPkz_B-Rb63HaXQo0SeRb2zC-cbEY5BHGHAgbfnh_yqR30KsoPsGfioy3unxtZH3iZOesD-cRrOsgKylnTGkQ4JjCflKDK8oc2AqOZdpSEtUYL9aEVtd93MQxoON4thV5RdIzE9TdMosWt5vt0fHeA96MXX-EpG3F0BRBViPw-vFiuedFaVtH38yAWoHivSr6_DO73jRoHfnmnYCyR2yoQh0dCwq97uKVvKdWmieQXyHQqsaNtbuDnREQzJHh3Z6-huQNcBzZABeESieJO_1I4CN6G12k2Lp2JAcu2jE-XMTyMVeMIgeary87jFiv0SqWHBnlFHcx-xFeda9dXvFDlQ1qFs8ugP3YOAXpBfCbHnqelxwj8bs8cMgdMoyH7Ox3oLuoctYIYxfnstfKnGI-5m7naHmAnvzILNuF0YYZ52xOzID9FNGeCCquGpvz32Qzkww9bLf43vn789nhMNFvXYfogKDOHHRMlMGaapxgC2_x1TqnKHKDxpLUow-l4SvR-gJcYlN7A1pnbCTCwBT3U8cDFmXTeUA1SAIu01L-ZbgwoLqZGPY-nw6XqdibJki5F9ygDNnQtA6W9UB1-4IQYiiqMRYNUX2jkWhb8JqeKfcNnHOk2yEcvk3IC5r9tQw1mPJLrWXgQeEiMxNRm3FOHCDF-WGLiPHuHBR9h9NDgqM8cnnbSXPQcveoU0dGlHxEFhGog9b50KtSM3DyCncqHRTuvHVg-rF9uyF3-BLj3IH4Hl-UDzus1n3Hmbw-wLmkJJNqre1V_mfrBn_DxxYiltwn7MotKtNUEczYUt2KcOvBrOD7p7f8e95cKV2DO1ARq3cxRdR_0Jbxqh5wHKmu9VaU96YU9wClg7CBuZK-HnA_aChkgpVTA_MIscWv9F9AILgfG7A57OMQaOWyBE2ZX6cPIbF7AsjafhJb_OsTz7ZqO4ZhM_uKNvjNGHSmkELI01roubEmMgFpRTeBZJLiJs_4UOL8TbY7mQWbjQKWPRooKHhXmKtkj5s634VdJZck0LVP1BWwwv41YbiWGOovVwQmBBOkXJpMzfmuZuXUpnvs36kRiTDufnY_MM-Cq8CVMFFqVf_wPE7dY3ntbrzBzTccytlSDWUILCP4e41eaODsLDSyReOwHiFzWY8GJHgBmEyz8W6ebd4N-m6GX2_ReBUs961_8nQmvPzpTX24cl0Z1_ixg-82zyC5rRYKi8_qhGPAffWWK0Q7v-4OXg80FqFtESKriRoartqkiLom4qMfCjld-4T5V2Eqq6GSPo0lw4DfkD3jbFi5tgKOMpsMlA0TT06La3c6QEBjkxfZ0--26S0ABmwogKATz7-_VngbUj_FyMd0QTLPjiXQY9vHC9t56v9buD3ul4FSBDuM_x54_IIc4kXP_yRCvmG_WaCzB09YWMKZHNRIunmUb5oD1f-1muDef5LRPUl1sdoWx6XY3xGdGsmfZtkjfI7VH5dWYO2_Om0bG79YcQvVRgFVIBKBKf3GB2ArQ_NJOUPp9K",
>    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImtpZCI6Im5PbzNaRHJPRFhFSzFqS1doWHNsSFJfS1hFZyJ9.eyJhdWQiOiI3MzZkNmQwNi0yOWNhLTQ2NGItYTQ0Zi03NDE4ZGY0YjliN2EiLCJpc3MiOiJodHRwczovL2xvZ2luLm1pY3Jvc29mdG9ubGluZS5jb20vYjUwOTQzZWMtZDExMi00ZGMzLTkyNjAtODE1ZWU1MGEyOWMxL3YyLjAiLCJpYXQiOjE2MTUzNTgzNDgsIm5iZiI6MTYxNTM1ODM0OCwiZXhwIjoxNjE1MzYyMjQ4LCJhaW8iOiJBV1FBbS84VEFBQUFiZ0prTWlKUjhQZHNmTzFHUFBFb3NBMFVLRFZNdzllZXU4SVdUaCs0S1d1NlVwdXhMZlVJS204ZW5qOTFCc2dPamM3UHB5Tmp1bFZLazV0OVFVYWwvS3RCUmUzdkZyTHVJbW1zK3Y2QngrclZZc0tuYldqNk93T1RGRlJPQjExSiIsImVtYWlsIjoiZDEwMzE5MTE1QGdtYWlsLmNvbSIsImlkcCI6Imh0dHBzOi8vc3RzLndpbmRvd3MubmV0LzkxODgwNDBkLTZjNjctNGM1Yi1iMTEyLTM2YTMwNGI2NmRhZC8iLCJyaCI6IjAuQUFBQTdFTUp0UkxSdzAyU1lJRmU1UW9wd1FadGJYUEtLVXRHcEU5MEdOOUxtM3BLQUo0LiIsInN1YiI6IlpRd2xNNC01N3JjYkxwZ0haQUlkTXdOaUpxUEpBc04tcWtUbmRJWDFzREUiLCJ0aWQiOiJiNTA5NDNlYy1kMTEyLTRkYzMtOTI2MC04MTVlZTUwYTI5YzEiLCJ1dGkiOiJYN3oxeXJXUVowYVBIbEJ2blA4UUFBIiwidmVyIjoiMi4wIn0.A0gxaM4PI3DOOcRp0YQQYrKdI0Q_PE38UNemluRfjyMRP7mEP-Fz7SsHHFVYMCy7cYl345tvoh9sLw4-MOY5J-u5_wXDvpTz7Y8LdG25ry55CE_2A8kcp7UvXsTfDMuAQYeRwxQvmqdSXTpgqt_DI_TVcdEx2TNSWJ0ZkPbFZWgu-kCpsgxSv-kLu2po_aC-aKBGg5dSlE5NV2Ax0eWbzeBVFJAkuunDN8R9PRhxw1Y3boByMmmjTdThEmp8V1R_4hADcLN0IVoRhpTkNMu_KVwG0lKwHE1TiFPWD1RcajdfGzaXv7dgaq1YzYty7qm6mdzFOi6PAbn0hjWzIocTZw"
>}
>```
> * token_type: Token Type
> * scope : 取得範圍
> * expires_in : 過期時間
> * ext_expires_in : 過期時間
> * access_token : 訪問令牌
> * refresh_token : 更新令牌
> * id_token : JWT Token
> 
> 
> ---
> 
> 
> ### <span id = "no.3">混合式</span>
>
> ```http request
>-X GET
> https://login.microsoftonline.com/b50943ec-d112-4dc3-9260-815ee50a29c1/oauth2/v2.0/authorize?
>    client_id=736d6d06-29ca-464b-a44f-7418df4b9b7a
>    &response_type=code%20id_token
>    &redirect_uri={{redirect_uri}}
>    &response_mode=fragment
>    &scope=openid%20https://graph.microsoft.com/Mail.Read
>    &state={{state}}
>    &nonce={{nonce}}
>    &code_challenge={{code_challenge}}
>    &code_challenge_method=S256
> ```
> * client_id : 用戶端識別碼
> * response_type : 需要回傳code 或是 id_token
> * redirect_uri : 轉跳的路徑
> * response_mode : 
> * scope : 要求的範圍
> * state :
> * nonce : 
> * code_challenge : 
> * code_challenge_method : 
> 
> #### Example
> ```http request
>-X GET
> https://login.microsoftonline.com/b50943ec-d112-4dc3-9260-815ee50a29c1/oauth2/v2.0/authorize?
>     client_id=736d6d06-29ca-464b-a44f-7418df4b9b7a
>     &response_type=code%20id_token
>     &redirect_uri=https%3A%2F%2Ftw%2Eyahoo%2Ecom
>     &response_mode=fragment
>     &scope=openid%20https://graph.microsoft.com/Mail.Read
>     &state=12345
>     &nonce=abcde
>     &code_challenge=kw4NkFwmeNdygVX84Rm89D4xbOjj3wz2HgDLZuNuoJg
>     &code_challenge_method=S256
> ```
> 
> #### 回傳結果
>```http request
>https://tw.yahoo.com/#
>    code=0.AAAA7EMJtRLRw02SYIFe5QopwQZtbXPKKUtGpE90GN9Lm3oBAMs.AQABAAIAAAD--DLA3VO7QrddgJg7Wevr0XnexsS0gS-YhiW8AwAJ73MN22aTbndwKhQg_Qve1GGWqI2Z4EimxzavL-lo_hB93Zdl2YVdwXu90kgDKCI6SJMmqDuMjy39EqQdnaO1iejBm6uhjSPbT5INuc-J9h2mCvkf7I1hwqn7tsteZySHvV3qK8NhGj0-a31YZbX-AgoG6qMpkYi_2KAD90K1PyqyRUBe6oG1MCZSGrKrFV4AHO1ASa96g62k5_m4VMgwLst5IBmwXmLJbXZWzgMVUtznuT3hvHT4FoEkeF4C26q79TIsWtVh3VKVJJ-c9N33XOhKTrk8ZMJ6QGQnrdfcfPL1ZL6MQMz8Dc3Yv9o8sYSDFT8sJ-QyGuvAz-BP14KFHu7SMS7tp4rRQlt2uo_ItA2_jkUrES3NBZBfEvLkBqkY5aiZnH0-u8V57zuhmpY4qyM3MeVDf5Hi2LHR3S3J8c_1wTy7UPaiPzqwOpdOSh5TEDbPGnZwfq_fA_7aqPGI9fL0j1K3gUjogkUewrD9tSfjbT9L2fexGSVu-rscOllO4rhJOglo-ApDIxMWuZ9CmiqX-zXMK30GQ4RaTdIpzoeKSdxCc63UE-Xi1eO9V8iltTipEOtZYU6kFwwzhQXohScgrqVHleiO3GetaPqfU7QB2RRuEQilFPsRJkdBW4-WswzoCu7eZm4rb7eZpcJDqMWh9M5EVS2F5l4Z9thaoXg4EvbabkclEffTrz0IHDxOsKi_oHDr6HDAGEfK28UaSRriarZZoFg2zZMRCeFBW-YzAoy_AMJiNjf4Uv2pQb13_g61SPsnaJ0HEbhrgS5CDjzWlHIkcS5lxj7_GswJW9qLptifuJ46DhnvlEtgdSFancpF_2qCBykPNO_Ke_vkYyH9Fh76ha3m1aMS5AKcrj2KLWvrk-uQoxRID26L7Q24UeSEGI7ntGDj0bvbTgAjjEi64UDgK-BmtMBqigQZcETeZYdDORS0hCrXkF09jluCxI9GwHr-scA6WrbtPudoeiP6MRAu75K2-0M0wFKPgh4Dkcmsx1JlDNv9-8-Q4KjyjQr5v2a6NEEnctwKbLZ7JWHyCHzU6jtHlxcjvc9zSi7bGJUUuzvYp8T3_BGlYiHy-0Gn7k9Bc6k7j6sgcRsmDNFtfiKDKMuo3AvUyQ7ILNgQu4_tQgTnp1eypkbBYvTHv2GhW5Y3OPQuRJFx36NIXoCF70Y32dJe56enCCrwETRV8ytJTc1JW7olGaCVR_lcCt9HiXfQRDMjteD9Bdo9VDLxb-6BGi6IVCZaZeZFdbTCwRD5FQPNPX_cGcqhcXHSYyAA
>    &id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImtpZCI6Im5PbzNaRHJPRFhFSzFqS1doWHNsSFJfS1hFZyJ9.eyJhdWQiOiI3MzZkNmQwNi0yOWNhLTQ2NGItYTQ0Zi03NDE4ZGY0YjliN2EiLCJpc3MiOiJodHRwczovL2xvZ2luLm1pY3Jvc29mdG9ubGluZS5jb20vYjUwOTQzZWMtZDExMi00ZGMzLTkyNjAtODE1ZWU1MGEyOWMxL3YyLjAiLCJpYXQiOjE2MTUzNDgyNDYsIm5iZiI6MTYxNTM0ODI0NiwiZXhwIjoxNjE1MzUyMTQ2LCJhaW8iOiJBV1FBbS84VEFBQUFDV1l6THQ5RFpEZk5yNWkwVHVJWkZsTzJFWEovVlJMMHA0b0NkWVN1RXEydnB0dFNvaXJoWVFBaEpvdDdrNDIwalQ3RUVVc2U4ZmE3d3N4RFJ5aFI3ZE5ZSU9GNVdEZENVU2lXVk95SWxHRkxsM0t5VDZWYXA4QU5zTjhSWGNwYyIsImNfaGFzaCI6Ik41M0lvZGNpWEZidk9oTFQxVWxId1EiLCJlbWFpbCI6ImQxMDMxOTExNUBnbWFpbC5jb20iLCJpZHAiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC85MTg4MDQwZC02YzY3LTRjNWItYjExMi0zNmEzMDRiNjZkYWQvIiwibm9uY2UiOiJhYmNkZSIsInJoIjoiMC5BQUFBN0VNSnRSTFJ3MDJTWUlGZTVRb3B3UVp0YlhQS0tVdEdwRTkwR045TG0zcEtBSjQuIiwic3ViIjoiWlF3bE00LTU3cmNiTHBnSFpBSWRNd05pSnFQSkFzTi1xa1RuZElYMXNERSIsInRpZCI6ImI1MDk0M2VjLWQxMTItNGRjMy05MjYwLTgxNWVlNTBhMjljMSIsInV0aSI6IkVRN0w3YVdldGtLYlhJOTVjRTVLQUEiLCJ2ZXIiOiIyLjAifQ.maBxpdeajxrtZ-RBxU-OjpL5xWwr_fDxS_3sOCVxMHL5cjrZ5XIJfHdTWy2dM_p5YNHwNysbJJicatZG4KvZS0HbsPCTIpq4RM5PRtfpnWBNnyJhWffjQMv__O1bgNWJFohO0N5osRWEUK02jzbEwBuqfgRtaoqOLcTeThcVIoJgAUYSyObgKx3He41OAiwmlJuT9mG7afVyX4F7Vc4UNdOqkpIcIjDLfhkjVAGgLNwrJj0zJcGE0H3Ar7soYhH__kQETrrvaNUSvrGzDHaARh2CdL2_pTih03YN0eZ2JBiF-Fa9D23RDA_YNay5dVZ0L3HhY2_ejWRyC0SgPNk5Lw
>    &state=12345
>    &session_state=37dc51b9-2ae9-4da0-9315-bb659b6d2314
>```
> * code : [取得AccessToken的code](#code)
> * id_token : (JWT Token)
> * state : 
> * session_state : 


## 工具
- [Parse JWT](https://jwt.io/)
- [Generate PKCE](https://tonyxu-io.github.io/pkce-generator/)
- [Checkout Error Code](https://login.microsoftonline.com/error)


## 參考資料

- [What's PKCE](https://waynechu.cc/posts/298-what-is-pkce-proof-key-for-code-exchange)
- [OAuth2.0 PKCE](https://tonyxu.io/zh/posts/2018/oauth2-pkce-flow/)
- [OAuth2.0 Authorization Code](https://blog.yorkxin.org/posts/oauth2-4-1-auth-code-grant-flow.html)

## 聯絡作者

- [信箱](mailto:poshen0322@gmail.com)


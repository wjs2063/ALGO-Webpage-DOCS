
 
### social login 자료가 담겨있는 github reposit 이다
```

https://github.com/tomasvotava/fastapi-sso
```


### KAKAO URL 

https://developers.kakao.com/docs/latest/ko/kakaologin/common#additional-consent-scope
읽어보자

실제 서비스를 하면 정보파기규정을 지켜야하니 조심하자. 



어쨋든 카카오Develope 를 가입하고 서비스가입을 하면 REST_API,CLIENT_SECRET 을 발급받을수있다 그것을 아래 해당 소스코드에 입력하자  
공유기 사용자라면 공유기 설정 홈페이지에 접속하여 포트포워딩을 해야한다.  
공유기  IP -> 내부 IP 변환 이므로  결국 외부사람이 접속할떄는 공유기 IP :PORT 로 접속하면 내부 IP:PORT 로 접속이 가능하게 포트포워딩을 해주면된다.  
또한 redirect url 설정 및 OPEN_ID 신청 을 




```

"""Kakao Login Example
"""

import os
import uvicorn
from fastapi import FastAPI, Request
from fastapi_sso.sso.kakao import KakaoSSO

os.environ["OAUTHLIB_INSECURE_TRANSPORT"] = "1"
CLIENT_ID = 발급받은 너의 REST_API ID 를 넣자
CLIENT_SECRET = 발급받은 너의 CLIENT_SECRET 넣자

app = FastAPI()

sso = KakaoSSO(
    client_id=CLIENT_ID,
    client_secret=CLIENT_SECRET,
    redirect_uri="http://[포트포워딩한 너의 공유기 IP]:[너의 PORT]/auth/callback",
    allow_insecure_http=True,
)


@app.get("/auth/login")
async def auth_init():
    """Initialize auth and redirect"""
    return await sso.get_login_redirect()


@app.get("/auth/callback")
async def auth_callback(request: Request):
    """Verify login"""
    return await sso.verify_and_process(request, params={"client_secret": CLIENT_SECRET})

"""
if __name__ == "__main__":
    uvicorn.run(app="examples.kakao:app", host="127.0.0.1", port=8000, reload=True)
"""
```





```
from fastapi_sso.sso.base import DiscoveryDocument, OpenID, SSOBase


class KakaoSSO(SSOBase):
    """Class providing login using Kakao OAuth"""

    provider = "kakao"
    scope = ["openid"]
    version = "v2"

    async def get_discovery_document(self) -> DiscoveryDocument:
        return {
            "authorization_endpoint": "https://kauth.kakao.com/oauth/authorize",
            "token_endpoint": "https://kauth.kakao.com/oauth/token",
            "userinfo_endpoint": f"https://kapi.kakao.com/{self.version}/user/me",
        }

    @classmethod
    async def openid_from_response(cls, response: dict) -> OpenID:
        return OpenID(display_name=response["properties"]["nickname"], provider=cls.provider)


```

### Trouble Shooting URL

https://developers.kakao.com/docs/latest/ko/kakaologin/trouble-shooting








## 결과
<img width="500" alt="스크린샷 2023-02-15 오후 1 35 58" src="https://user-images.githubusercontent.com/76778082/218931844-1779431a-c6bb-4197-bca7-d7959f2500c1.png">




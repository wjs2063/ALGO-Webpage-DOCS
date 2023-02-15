
 
### social login 자료가 담겨있는 github reposit 이다
```

https://github.com/tomasvotava/fastapi-sso
```

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
    redirect_uri="http://172.30.1.56:50002/auth/callback",
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

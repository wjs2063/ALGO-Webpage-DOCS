# ALGO-Webpage-DOCS
알고리즘 웹페이지  환경설정 및 문서제작 (소스는 현재 비공개)





## OAUTH 2
Open Authorization 
인증 프로토콜이아니라 일부 권한만 받아 주는것, Authroization Code (Token) 을 발급하여 잠시 이용가능하게 해주는것 

1. Resource Owner (사용자 , 유저 )
2. Client ( 서버 , 나의 fastsapi server )
3. Resource Server (구글,네이버,카카오,깃허브 등의 서버)
4. Access Token (만료기한이있는 접근토큰)

## JWT

Json
Web
Token 



## CORS Error Message

CORS -> Same Origin Policy -> 동일 origin(도메인)에 대해서만 가능하게 하도록 하는 보안정책
즉 요청 브라우저에서 다른 도메인으로 갈때 나오는 보안 정책 에러이다. 

fastapi 에서는 wild card 혹은 frontend ip:port 를 적어주어 해결한다

```
from fastapi.middleware.cors import CORSMiddleware



origins = [
    "*"
]

app.add_middleware(
    CORSMiddleware,
    allow_origins=origins,
    allow_credentials=True,
    allow_methods=["*"],
    allow_headers=["*"],
)


@app.get("/")
async def main():
    return {"message": "Welcome to ALGO_WEB_PAGE!!"}

```

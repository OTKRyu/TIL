# CORS

우회법

- 크롬이 업데이트가 되면 될 수록 cors 우회법이 거의 다 막혔다
- 그러므로 초기부터 이를 될 수 있도록 설정하던가, 개발단계에서는 우회 명령어를 써서 하는게 좋다.

Using the current latest chrome **Version 92.0.4515.107 (Official Build) (64-bit)**

**windows :** click the start button then copy paste the below (change the ***D:\temp*** to your liking).:

```js
chrome.exe  --disable-site-isolation-trials --disable-web-security --user-data-dir="C:~\temp"
```

**Linux :** start a terminal then run the below command (change the **~/tmp** directory to your liking)

```js
google-chrome --disable-site-isolation-trials --disable-web-security --user-data-dir="~/tmp"
```

**Note : This solution will start chrome in an isolated sandbox and it will not affect the main chrome profile.**
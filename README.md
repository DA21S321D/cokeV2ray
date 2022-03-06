**因为v2ray 中vmess是较为广泛的协议，支持的设备也多，个人感觉没必要弄太多协议**
### 服务端

[![Deploy](https://www.herokucdn.com/deploy/button.png)](https://dashboard.heroku.com/new?template=https://github.com/chuccp/cokeV2ray) 

部署后，访问直接页面为404，但是不影响使用
### 客户端
"outbounds":
```json   
  "outbounds": [
    {
      "tag": "proxy",
      "protocol": "vmess",
      "settings": {
        "vnext": [
          {
            "address": "xxxx.herokuapp.com",
            "port": 443,
            "users": [
              {
                "id": "24b4b1e1-7a89-45f6-858c-242cf53b5bdc",
                "alterId": 0,
                "email": "t@t.tt",
                "security": "none"
              }
            ]
          }
        ]
      },
      "streamSettings": {
        "network": "ws",
        "security": "tls",
        "tlsSettings": {
          "allowInsecure": false
        },
        "wsSettings": {
          "path": "/wowowowo"
        }
      }
    }
  ]
```
cloudflare workers 脚本，可以多部署heroku服务，轮询使用，间接的提升速度
```addEventListener("fetch", event => {
event.respondWith(handleRequest(event.request))
})

const urls = ['1.herokuapp.com', '2.herokuapp.com', '3.herokuapp.com']
let index = 0
async function handleRequest(request) {
console.log(index)
let host = urls[index]
if(index==0){
index = urls.length;
}
index--;
let url = new URL(request.url);
url.hostname = host;
let request2 = new Request(url, request);
return fetch(request2)
}
```
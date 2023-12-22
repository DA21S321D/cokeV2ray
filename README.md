**本项目是使用v2ray api直接封装，后续会跟随v2ray升级而升级，采用编译的方式安装，理论上被ban的概率较低**

**因为v2ray 中vmess是较为广泛的协议，支持的设备也多，个人感觉没必要弄太多协议**

**有自己服务器的建议使用 https://github.com/chuccp/v2rayAuto 部署服务**

### 服务端



[![Deploy](https://www.herokucdn.com/deploy/button.png)](https://dashboard.heroku.com/new?template=https://github.com/DA21S321D/cokeV2ray) 

由于本GitHub地址被限制 ，使用者可以将项目fork到自己的GitHub

https://dashboard.heroku.com/new?template=https://github.com/chuccp/cokeV2ray  

然后将 https://github.com/chuccp/cokeV2ray 改成自己的fork后的新地址，最好将cokeV2ray改成其它名称

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
cloudflare workers 脚本，可以多部署heroku服务，随机使用，间接的提升速度，分散流量，添加或替换
const urls = ['1.herokuapp.com', '2.herokuapp.com', '3.herokuapp.com']
中的地址
```
addEventListener("fetch", event => {
  event.respondWith(handleRequest(event.request))
})

const urls = ['1.herokuapp.com', '2.herokuapp.com','3.herokuapp.com','4.herokuapp.com']
async function handleRequest(request) {
   let rurl = request.url
    let url = new URL(rurl);
   if(url.pathname=="" || url.pathname=="/"){
         url.hostname = "www.baidu.com";
        let request3 = new Request(url, request);
        return fetch(request3)
   }
  let host = urls[Math.floor(Math.random()*urls.length)];
  url.hostname = host;
  let request2 = new Request(url, request);
  return fetch(request2)
}
```

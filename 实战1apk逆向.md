

完成样本软件的去除签名校验、去广告与更新、布局优化

## 1.去除广告
关键字:initsdk、loadad、initad等
找到广告方法位置，直接算法助手添加自定义hook：类设置b.l.g.s，方法名设置d，启用拦截。


| 广告关键词                          | 厂商           | 文档                                                                                                                                                         |
| ----------------------------------- | -------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| com.qq.e.ads                        | 腾讯优量汇广告 | https://developers.adnet.qq.com/doc/android/union/union_splash                                                                                               |
| CSJAD、TTAdSdk、bytedance、pangolin | 穿山甲广告     | https://www.csjplatform.com/supportcenter/5395                                                                                                               |
| ADMob、google.ads                   | 谷歌广告       | https://developers.google.com/admob/android/app-open?hl=zh-cn#extend                                                                                         |
| TorchAd                             | 360广告        | https://easydoc.soft.360.cn/doc?project=186589faed863b0a24f15f9bcbafd5c7&doc=2cbbbe19c5cb90f5e7a41c7037b0029a&config=title_menu_toc                          |
| kwad                                | 快手广告       | https://u.kuaishou.com/home/help/detail/1334/1370/1310                                                                                                       |
| baidu.mobads                        | 百度广告       | http://bce.ssp.baidu.com/mssp/sdk/BaiduMobAds_MSSP_bd_SDK_android_v5.1.pdf                                                                                   |
| MimoSdk                             | 米盟广告       | https://t5.a.market.xiaomi.com/download/AdCenter/0d3a369516ee146e8a9d5c290985939da4624fe0a/AdCenter0d3a369516ee146e8a9d5c290985939da4624fe0a.html            |
| sigmob.sdk                          | sigmob广告     | https://doc.sigmob.com/#/Sigmob%E4%BD%BF%E7%94%A8%E6%8C%87%E5%8D%97/SDK%E9%9B%86%E6%88%90%E8%AF%B4%E6%98%8E/Android/SDK%E6%8E%A5%E5%85%A5%E9%85%8D%E7%BD%AE/ |
| TradPlus                                    |TradPlus聚合广告                |https://service.cocos.com/document/zh/tradplusad.html                                                                                                                                                              |
通过免广告关键字来实现部分广告的去除
```java
public void O(SaiUserInfo saiUserInfo, boolean z) {  
        g0.i(BaseApp.getInstance(), saiUserInfo);  //获取用户信息
        if (z) {  
            v.c().q(SaiSPKey.appToken, saiUserInfo.getToken());  //获取Token
        }  
        this.e.set(saiUserInfo);  
        this.h.setValue(saiUserInfo.getPic());  //设置用户头像
        if (saiUserInfo.getLogin_type() == 1) {  //判断登录状态，并设置用户信息
            this.g.set("点击登录");  
            this.f.set(Boolean.FALSE);  
        } else {  
            this.g.set(saiUserInfo.getNickname());  
            this.f.set(Boolean.TRUE);  
        }  
        ObservableField observableField = this.i;  
        observableField.set("ID:" + saiUserInfo.getUser_id());  //获取用户ID
        ObservableField observableField2 = this.j;  
        observableField2.set(SaiAppUtils.d(saiUserInfo.getInvited_count() + "人"));  //获取用户邀请人数
        if (saiUserInfo.getInvited_count() > 0) {  
            ObservableField observableField3 = this.k;  
            observableField3.set(SaiAppUtils.d(N(saiUserInfo.getInvited_count()) + "天 "));  
        } else {  
            this.k.set(SaiAppUtils.d("0天"));  
        }  
        if (saiUserInfo.getFree_time() * 1000 > System.currentTimeMillis()) {  //获取免广告时间
            this.n.set(0);  
            this.m.set(d.a(Long.valueOf(saiUserInfo.getFree_time() * 1000)));  
            this.l.set("剩余免广告：");  
            BaseApp.getInstance().setFreeTime(saiUserInfo.getFree_time() * 1000);  
            return;  
        }  
        this.l.set("邀请好友获得终身免广告特权");  
        this.n.set(8);  
        BaseApp.getInstance().setFreeTime(0L);  
    }
```

```java
public void initData() {
        super.initData();
        if (!v.c().b(SaiSPKey.AGREE_PRIVATE, false)) {
            ((SaiSplashViewModel) this.viewModel).b(b.s.c.b.a().c(SaiPrivateEvent.class).observeOn(AndroidSchedulers.mainThread()).subscribe(new Consumer() { // from class: b.l.i.v.d
                @Override // io.reactivex.functions.Consumer
                public final void accept(Object obj) {
                    SaiSplashActivity.this.g((SaiPrivateEvent) obj);
                }
            }));
            a0.a.h(this);
            return;
        }
        try {
            s.a.d();  //广告SDK初始化
        } catch (Exception e2) {
            e0.b("===========>>> " + e2.getMessage());
        }
        int g2 = v.c().g(SaiSPKey.INSTANCE.getLaunchCount(), 0);
        if (g2 != 0) {
            if (!v.c().b("extend_java_aa", false)) {
                v.c().s("extend_java_aa", true);
                g2 = 0;
            }
        } else {
            v.c().s("extend_java_aa", true);
        }
        if (g2 == 0) {
            showLoaddingDialog();
            ((SaiSplashViewModel) this.viewModel).b(b.s.c.b.a().c(SaiAppInitEvent.class).observeOn(AndroidSchedulers.mainThread()).subscribe(new Consumer() { // from class: b.l.i.v.a
                @Override // io.reactivex.functions.Consumer
                public final void accept(Object obj) {
                    SaiSplashActivity.this.i((SaiAppInitEvent) obj);
                }
            }));
        }
        g.a.a(g2);
        int i2 = g2 + 1;
        v.c().m(SaiSPKey.INSTANCE.getLaunchCount(), i2);
        c0 c0Var = c0.a;
        if (c0Var.f3990g == -1) {
            c0Var.f3990g = i2;
        }
        if (g2 != 0) {
            if (NetworkUtils.c()) {
                ((SaiSplashViewModel) this.viewModel).n();
                if (c0Var.l("1")) {
                    showLoaddingDialog();
                    SaiSplashAdActivity.invoke(this);
                    finish();
                } else {
                    n(b.DELAY);
                }
            } else {
                n(b.NONET);
            }
        } else {
            ((SaiSplashViewModel) this.viewModel).n();
        }
        if (i.c(d.f().toString(), c0Var.f3986c).equals(c0Var.j(R.string.app_cudgel))) {
            return;
        }
        e0.b("===========>>> app kill app_cudgel");
        System.exit(0);
    }
```
邀请人免广告时间功能
getFree_time()方法里有free_time变量，修改赋值即可。
或者使用算法助手自定义hook修改返回值。
```java
.method public getFree_time()J
    .registers 3

    .line 1
    iget-wide v0, p0, Lcom/pencil/saibeans/SaiUserInfo;->free_time:J

    const-wide v0, 0x32d57bf5e8L

    return-wide v0
.end method
```

## 2.去除签名校验
老规矩，先签个名直接安装，果不其然，闪退。

用算法助手拦截闪退，通过拦截日志找到系统退出的类方法代码。修改smile代码让其不闪退。

[吾爱破解安卓逆向入门教程《安卓逆向这档事》六、校验的N次方-签名校验对抗、PM代理、IO重定向](https://www.bilibili.com/video/BV1X24y1m7cj/?share_source=copy_web&vd_source=3bc2e3777e91ab913d8a64fe4871a3b1)

1.核心破解，免签名安装

2.一键去签名工具

[[实战破解]白描-动态代{过}{滤}理Hook签名校验](https://www.52pojie.cn/thread-1526854-1-1.html)

3.手动分析签名校验
下面是修改smali代码返回原始的签名值md5。
```java
.method public static f()Ljava/util/List;
    .registers 2
    .annotation build Landroidx/annotation/NonNull;
    .end annotation

    .annotation system Ldalvik/annotation/Signature;
        value = {
            "()",
            "Ljava/util/List<",
            "Ljava/lang/String;",
            ">;"
        }
    .end annotation

    .line 1
    new-instance v0, Ljava/util/ArrayList;

    invoke-direct {v0}, Ljava/util/ArrayList;-><init>()V

    const-string v1, "F9:6C:E9:5F:D5:47:BE:DF:81:15:E3:71:8A:10:54:45" 
    #创建了一个新的 ArrayList 实例，然后向列表中添加了一个字符串常量 "F9:6C:E9:5F:D5:47:BE:DF:81:15:E3:71:8A:10:54:45"

    invoke-virtual {v0, v1}, Ljava/util/ArrayList;->add(Ljava/lang/Object;)Z


    return-object v0
.end method

```

![](_assets_00/7462852ab6eb72614bcf04006c7899b8889.webp)

## 3. 去除更新弹窗
算法助手-弹窗定位，日志。
算法助手-屏蔽关键词弹窗
下面是类似弹窗代码，直接再前面返回false即可。
```java
public static boolean a(boolean z, SaiUpgradeInfo saiUpgradeInfo) {  
        if (!a.b.b() && saiUpgradeInfo != null) {  
            try {  
                //获取版本号进行对比
                if (saiUpgradeInfo.getVersion_code() >= b.c.a.b.d.i()) {  
                    boolean b2 = v.c().b(SaiSPKey.INSTANCE.getUpgrade_dialog(), false);  
                    if (z || !b2) {  
                        if (saiUpgradeInfo.getIs_same() == 1) {  
                            v c = v.c();  
                            if (!c.b("appUpgrade_" + saiUpgradeInfo.getVersion_code(), false)) {  
                                new d(b.c.a.b.a.h(), saiUpgradeInfo).show();  
                                v c2 = v.c();  
                                c2.s("appUpgrade_" + saiUpgradeInfo.getVersion_code(), true);  
                                return true;  
                            }  
                        } else if (saiUpgradeInfo.getVersion_code() > b.c.a.b.d.i()) {  
                            new d(b.c.a.b.a.h(), saiUpgradeInfo).show();  
                            return true;  
                        } else if (z) {  
                            ToastUtils.v("已经是最新版本");  
                        }  
                        e0.b("========>>> 包升级更新：${it.url}");  
                    }  
                } else if (z) {  
                    ToastUtils.v("已经是最新版本");  
                }  
            } catch (Exception unused) {  
            }  
        }  
        return false;  
    }
```
## 4.布局优化
[【吾爱破解安卓逆向入门教程《安卓逆向这档事》四、恭喜你获得广告&弹窗静默卡】](https://www.bilibili.com/video/BV14v4y1D7yA/?share_source=copy_web&vd_source=3bc2e3777e91ab913d8a64fe4871a3b1)

利用开发助手APP，查看布局定位。

1.修改xml中的属性值
| 元素名称 | 属性名称 | 描述 |
| --- | --- | --- |
| LinearLayout | android:gravity | 设置布局内部元素的位置，这里设置为中心和左对齐 |
| LinearLayout | android:orientation | 设置 `LinearLayout` 的方向，这里设置为水平 |
| LinearLayout | android:tag | 为视图设置标签，这里设置为 "binding_13" |
| LinearLayout | android:background | 设置背景图片，这里引用了名为 "sai_sp_stroke_divider" 的资源 |
| LinearLayout | android:paddingLeft | 设置左填充距离，这里引用了名为 "dp_15" 的资源 |
| LinearLayout | android:paddingTop | 设置顶部填充距离，这里设置为 10dp |
| LinearLayout | android:paddingBottom | 设置底部填充距离，这里设置为 10dp |
| LinearLayout | android:visibility | 设置视图可见性，这里设置为 "gone" |
| LinearLayout | android:layout_width | 设置布局宽度，这里设置为 0dp |
| LinearLayout | android:layout_height | 设置布局高度，这里设置为包裹内容 |
| LinearLayout | android:layout_marginRight | 设置右边距，这里引用了名为 "dp_8" 的资源 |
| LinearLayout | android:layout_weight | 设置布局权重，这里设置为 1.0 |
| LinearLayout | android:paddingVertical | 设置垂直方向的填充距离，这里设置为 10dp |
| ImageView | android:layout_width | 设置图片视图宽度，这里引用了名为 "dp_32" 的资源 |
| ImageView | android:layout_height | 设置图片视图高度，这里引用了名为 "dp_32" 的资源 |
| ImageView | android:src | 设置图片资源，这里引用了名为 "icon_mine_invitecode" 的资源 |
| TextView | android:textSize | 设置文本字体大小，这里引用了名为 "sp_14" 的资源 |
| TextView | android:textColor | 设置文本颜色，这里引用了名为 "common_h0" 的资源 |
| TextView | android:layout_width | 设置文本视图宽度，这里设置为包裹内容 |
| TextView | android:layout_height | 设置文本视图高度，这里设置为包裹内容 |
| TextView | android:layout_marginLeft | 设置左边距，这里引用了名为 "dp_10" 的资源 |
| TextView | android:text | 设置文本内容，这里设置为 "填写邀请码" |

```xml
android:visibility="gone"
```
2.上帝模式优化布局
上帝模式APP，拖动删除。
[[白嫖大法]某影视邀请协议分析](https://www.52pojie.cn/thread-1452996-1-1.html)

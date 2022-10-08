# reptile
Java爬虫实例
# 爬虫数据来源

## 一、天气

### 1、中国气象局

* https://weather.cma.cn/web/weather/map.html
* 难度：一般
####  流程

* 先通过城市名，查询城市编码（get请求，返回json）

  ```java
  //city-城市名
  String Url = "https://weather.cma.cn/api/autocomplete?q=" + URLEncoder.encode(city) + "&limit=5&timestamp=" + new Date().getTime();
  ```

* 通过城市编码，查询当前天气（get请求，返回json）

  ```java
  //adcode-城市编码
  String Url_now = "https://weather.cma.cn/api/now/" + adcode;
  ```
#### 结果

* 位置路径 
* 降雨量
* 温度 
* 气压 
* 湿度 
* 风向 
* 风标 
* 风速 
* 更新时间 
* 预警信息[标题,内容,发布时间]

### 2、天气网

* https://www.tianqi.com/

* 难度：一般
#### 流程

* 通过城市名，查询对应城市的天气url地址（get请求，返回json）

  ```java
  //city-城市名
  String url = "https://www.tianqi.com/tianqi/ctiy?keyword=" + city;
  ```

* 再根据url地址请求获取对应页面（get请求，返回html）

#### 结果

* city:城市名,
* date:日期,
* now:当前气温,
* weather:当日天气情况
* ,
* shidu:湿度,
* feng:风向,
* iwaixian:紫外线强度,
* kongqi:空气质量,
* pm:PM2.5,
* sun:日出日落时间

## 二、影视

### 1、茶杯狐

* https://cupfox.app/
* 难度：简单
#### 流程

* 直接通过影视名直接搜索，获得搜索结果界面（get请求，返回html）

  ```java
  //name-影视名
  String url = "https://cupfox.app/search?key=" + name;
  ```

#### 结果

* name:影视名,
*  url:播放地址, 
* web_name:播放平台名

## 三、音乐

### 1、音乐直链搜索

* https://music.liuzhijin.cn/
* 难度：简单
#### 流程

* 请求接口，查询获取数据（post请求，返回json）

  ```java
  String url = "https://music.liuzhijin.cn/";
  Map<String, Object> body = new HashMap<>();
  //音乐名
  body.put("input", name);
  //页码
  body.put("page", 1);
  //搜索方式，通过音乐名
  body.put("filter", "name");
  //搜索平台 (qq：QQ音乐；netease：网易云)
  body.put("type", type);
  String json = HttpRequest.post(url)        
      .form(body)        
      .header("X-Requested-With", "XMLHttpRequest")        
      .contentType("application/x-www-form-urlencoded;charset=UTF-8")        .execute().body();
  ```

#### 结果

* 歌名
* 歌手
* 平台
* 封面地址
* 音乐文件地址
* 来源地址
* 歌词

## 四、IP查询

### 1、 IP地址查询-【qqzeng-ip】 

* https://www.qqzeng.com/ip/
* 难度：困难
#### 流程

* 构造cookie ID ，模拟浏览器请求，通过验证请求，需要注意请求参数cid的加密方式（get请求，无返回）

  ```java
  private String getIPInfo3Cookie() {        
      HashMap<String, Object> m = new HashMap<>();        
      //APP名        
      m.put("app", "");        
      //设备名        
      m.put("br", "Pc");        
      //浏览器        
      m.put("bs", "Edg");        
      //浏览器版本        
      m.put("bsv", 103);        
      String cid = String.valueOf((int) (Math.round(2147483647 * Math.abs(Math.random() - 1)) * Integer.valueOf(TimeUtil.getTime(TimeUtil.MilliSecond)) % 1E10));        
      //Cookie ID        
      m.put("cid", cid);        
      m.put("df", "");        
      //页面地址        
      m.put("dl", "https://www.qqzeng.com/ip/");        
      m.put("dr", "");        
      //页面title        
      m.put("dt", "IP地址查询-【qqzeng-ip】");        
      m.put("dw", "");        
      //浏览器是否已启用 Java        
      m.put("je", 1);        
      //操作系统        
      m.put("os", "Windows");        
      //操作系统版本        
      m.put("osv", "10");        
      //时间戳        
      m.put("r", new Date().getTime());        
      //显示图像的调色板的位深度        
      m.put("sd", "24-bit");        
      m.put("sid", 1);        
      //窗口大小        
      m.put("sr", "1536x864");        
      //浏览器的语言版本        
      m.put("ul", "zh-cn");        
      m.put("vp", "374x705");        
      String json = JSONUtil.toJsonStr(m);
      //        System.out.println(json);        
      HttpCookie cookie1 = new HttpCookie("qqzeng_cid", cid);        
      HttpCookie cookie2 = new HttpCookie("f_ip", "qqzengip");        
      String user_agent="Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/103.0.5060.134 Safari/537.36 Edg/103.0.1264.77";        
      //模拟请求获取Cookie        
      String body = HttpRequest.get("https://www.qqzeng-ip.com/api/analytics")
          .form(json)                
          .cookie(cookie1, cookie2)                
          .header(Header.REFERER, "https://www.qqzeng.com/")                
          .header(Header.USER_AGENT,user_agent)                
          .execute().body();        
      return cid;    
  }
  ```

* 然后请求接口数据，获取查询结果（get请求，返回js数据）

  ```java
  HashMap<String, Object> map = new HashMap<>();
  long time = new Date().getTime();
  String jq = "jQuery" + ("2.1.4" + Math.random()).replace(".", "");
  String Url = "https://www.qqzeng-ip.com/api/ip?" +        
      "callback=" + jq +        
      "_" + (time - RandomrUtil.createRandom(1, 50)) + "&" +        
      "ip=" + ip + "&_=" + time;
  String cid = getIPInfo3Cookie();
  HttpCookie cookie1 = new HttpCookie("qqzeng_cid", cid);
  HttpCookie cookie2 = new HttpCookie("f_ip", "qqzengip");
  String s = HttpRequest.get(Url)        
      .header(Header.REFERER, "https://www.qqzeng.com/")        
      .cookie(cookie1, cookie2)        
      .execute().body();
  ```

#### 结果

* 大洲
*  国家 
*  省份 
*  城市 
*  区县 
*  运营商 
*  区域码
*  国家 英文 
*  国家 简码 
*  经度 
*  纬度 
*  版本

## 五、豆瓣

### 1、豆瓣电影top250

* https://movie.douban.com/top250
* 难度：简单
#### 流程

* 直接请求访问页面，主要注意的是分页请求（get请求，返回html）

  ```java
  String Url = "https://movie.douban.com/top250";
  //page 页码 [1,10]
  int i = page - 1;    
  if (i > 0 && i < 10) {        
      Url += "?start=" + (25 * i);    
  }
  ```

#### 结果

### 2、豆瓣搜索

* https://www.douban.com/
* 难度：简单
#### 流程

* 直接通过关键字搜索（get请求，返回html）

  ```java
  String Url = "https://www.douban.com/search?q=" + key;
  ```

#### 结果

* url:豆瓣链接,
* img_url:封面,
* type:类型,
* name:名称,
* rating_nums:评分信息,
* subject_cast:主题演员,
* introduce:介绍

## 六、百度

### 1、疫情数据

* https://voice.baidu.com/act/newpneumonia/newpneumonia/?from=osari_aladin_banner
* 难度：一般
#### 流程

* 请求接口，需要注意请求参数的加密方式（get请求，返回js数据）注意返回数据量有点大

  ```java
  int ceil = (int) Math.ceil(1e5 * Math.random());
  String Url = "https://voice.baidu.com/api/newpneumonia?from=page&callback=jsonp_" + new Date().getTime() + "_" + ceil;
  ```

#### 结果

* 各省级疫情信息
* 全国疫情概要信息
* 国内疫情风险地区
* ...

## 七、有道

### 1、有道翻译

* https://fanyi.youdao.com/
* 难度：困难
#### 流程

* 构造请求参数和cookie

  ```java
  String ncoo=""+2147483647 * Math.random();
  //这里要给定一个ip地址
  String ipByHost = NetUtil.getIpByHost("www.baidu.com");
  String app_v="5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/103.0.5060.134 Safari/537.36 Edg/103.0.1264.77";
  String cookie="OUTFOX_SEARCH_USER_ID=-257520112@"+ipByHost+
      ";OUTFOX_SEARCH_USER_ID_NCOO="+ncoo+
      "; ___rl__test__cookies="+new Date().getTime();
  String salt = time + String.valueOf(RandomrUtil.createRandom(0, 9));
  String sign = EncryptUtil.getMd5("fanyideskweb" + e + salt + "Ygy_4c=r#e#4EX^NUGUc5");
  ```

* 请求接口，查询结果（post请求，返回json）

  ```java
  Map<String,Object> map=new HashMap<>();
  map.put("from","AUTO");
  map.put("to","AUTO");
  //要查询的内容
  map.put("i",e);
  map.put("smartresult","dict");
  map.put("client","fanyideskweb");
  long time = new Date().getTime();
  map.put("lts", time);
  map.put("salt", salt);
  map.put("sign",sign);
  map.put("bv",EncryptUtil.getMd5(app_v));
  map.put("doctype","json");
  map.put("version","2.1");
  map.put("keyfrom","fanyi.web");
  map.put("action","FY_BY_CLICKBUTTION");
  String json = HttpRequest.post(url).form(map)        
      .header(Header.REFERER, "https://fanyi.youdao.com/")        
      .header(Header.USER_AGENT,"Mozilla/"+app_v)        
      .cookie(cookie)        
      .execute().body();
  ```

#### 结果

* 翻译结果
* 翻译类型
* 相似结果

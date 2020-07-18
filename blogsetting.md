---
description: "其实主要是图床搭建啦\U0001F613"
---

# 博客简要配置

> 图床：PicGo+阿里云对象存储OSS服务
>
> 本地markdown编辑器：typora
>
> 博客直接用gitbook
>
> **PicGo简要介绍**：使用Electron+Vue开发的图床上传和获取Url的软件。

## 图床配置：

![typora&#x7684;&#x56FE;&#x7247;&#x914D;&#x7F6E;](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20200716173601669.png)

## 阿里云OSS配置

### 注册账号并实名认证\[略\]

### 购买服务

![bucket&#x521B;&#x5EFA;](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20200716174859140.png)

**填写Bucket名称，其余按照个人需要选取配置，当然也可以是默认**

### 获取配置项

```javascript
{
  "picBed": {
    "uploader": "aliyun",
    "aliyun": {
     // 必填
    "accessKeyId": "",
    "accessKeySecret": "",
    "bucket": "", 
    "area": "", 
    "customUrl": "", 
     // 可不修改
    "path": "img/", //在你的仓库下面创建文件夹存储，方便查找
     "options": "" // 针对图片的一些后缀处理参数 PicGo 2.2.0+ PicGo-Core 1.4.0+
    }
  },
  "picgoPlugins": {}
}
```

#### 获取accessKeyId、accessKeySecret

**accessKey创建并获取**

![accessKey&#x5165;&#x53E3;](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20200716175028051.png)

![](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20200716175421637.png)

#### 获取bucket、area、path

![&#x914D;&#x7F6E;&#x9879;&#x83B7;&#x53D6;](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20200716174531342.png)

### 填入config.json并测试

![&#x914D;&#x7F6E;&#x6D4B;&#x8BD5;](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20200716175726772.png)

我的测试失败了，但是仍然能够上传成功，所以不用太担心

![&#x6D4B;&#x8BD5;&#x7ED3;&#x679C;](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20200716175811610.png)

## GitBook使用

最后就是注册GitBook账号创建自己的Space了。

**优点：**

选择GitBook主要是因为个人用户可以免费，而且UI看起来是最干净的。

**缺点：**

①当然免费用户不能**多人协作**和**使用个性化主题**；

②还有一个挺致命的问题，就是国内用户打开太慢了。

> 不过总结来说，上面的的缺点主要是因为没钱。

​


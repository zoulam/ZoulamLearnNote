# \[spider\] cherrio

```bash
npm install express -S
npm install request cherrio -D
```



`robots.txt` 是爬虫协议，指网页允许被爬取的范围 直接在域名后面添加 `/robots.txt`即可查看，通常不能查看的部分都会显示为 **403 **或 **404**

```javascript
const express = require('express')
const app = express()
const request = require('request')
const cheerio = require('cheerio')
app.get('/', (req, res) => {
    request('https://sports.sina.cn/nba/2020-11-10/detail-iiznctke0618872.d.html?cre=tianyi&mod=wspt&loc=-2&r=-1&rfunc=18&tj=cxvertical_wap_wspt&tr=73&vt=4&pos=10', function (error, response, body) {
        if (!error && response.statusCode == 200) {
            const $ = cheerio.load(body, { decodeEntities: false })
            let title = $('h1').text()
            let avatar = `http:${$('.weibo_img img')['0'].attribs.src}`
            let author = $('.weibo_user').text()
            let time = $('.weibo_time').text().trim()
            let abstract = $('.art_p').text()
            res.json({
                "user": {
                    "author": author || '未命名的文章',
                    "avatar": avatar || "https://avatars1.githubusercontent.com/u/63615089?s=400&v=4"
                },
                "title": title || "未命名的标题",
                "createAt": `${new Date().getFullYear()}年${time}`,
                "content": abstract || '',
                "comments": [
                    {
                        "user": 'lala',
                        "commont": "文章还行"
                    }
                ]
            })
        }
    });
})
app.listen(3000, () => {
    console.log(`http://localhost:3000`);
})
```
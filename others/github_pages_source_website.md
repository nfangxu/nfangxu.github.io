### 思路分享: 关于我用 GitHub Pages 部署了一个带数据库的网站

- 传送门: [Github](https://github.com/nfangxu/nfangxu.github.io/tree/main/v)
- 传送门: [资源站](https://3c4j.com/v/)

#### 正文

本次分享适用于资源内容更新不频繁的数据库操作! 比如简单的博客内容,资源站等~~

GitHub pages 只支持静态文件, 无法使用数据库等写入的操作, 于是想到了用 sqlite 作为存储的数据库.

通过一个脚本程序, 叫内容写入到 sqlite 的文件中, 然后通过 js 读取进行页面渲染.

脚本程序我选择了使用 Golang, 他可以轻松的构建各个平台系统可运行的执行脚本, 方便使用 GitHub Actions 进行自动执行, 内容如下:

```yaml
name: txdb
on:
  # 可手动执行
  workflow_dispatch:
  schedule:
    # 每天 0 点定时执行, 北京时间 8 点
    - cron: '0 0 * * *'

jobs:
  # 我的执行文件在 {ROOT}/v/ 下面, 执行时切换了目录
  default:
    runs-on: macos-12
    steps:
      - uses: actions/checkout@v3

      - name: Allow script to run
        run: cd v && chmod +x ./txdb

      - name: Run script
        run: cd v && ./txdb

      - name: Push
        run: |
          git config --global user.name 'nfangxu'
          git config --global user.email 'nfangxu@gmail.com'
          git add .
          git commit -am "auto commit"
          git pull && git push
```

前端内容读取, 使用 sql.js, 按照文档书写, 部分内容如下:

```javascript
initSqlJs({
    // 这里直接引用了官方的 cdn 文件, 没有自己创建
    locateFile: file => `https://cdnjs.cloudflare.com/ajax/libs/sql.js/1.8.0/${file}`
}).then((SQL) => {
    const xhr = new XMLHttpRequest();
    // 拼装 db 文件名, 用于远程拉取
    let db = 'db/tx' + thisDB() + '.3c4j'
    if (search() === '') {
        // 对第一页内容进行单独缓存, 减少首次加载的延迟
        if (thisPage() === 1) {
            db = 'db/p1.tx' + thisDB() + '.3c4j'
        }
    }
    xhr.open('GET', db, true);
    xhr.responseType = 'arraybuffer';
    // 加了一个进度条, 用于db文件加载过程中的 loading 进度
    xhr.onprogress = e => {
        if (e.lengthComputable) {
            let per = parseInt(e.loaded / e.total * 100);
            let ld = document.querySelector('#loading')
            ld.querySelector('.progress-bar').textContent = '加载中: ' + per + '%'
            ld.querySelector('.progress-bar').style.width = per + '%'
        }
    }
    // db 文件加载完毕, 执行数据库查询操作
    xhr.onload = e => {
        // 移除进度条
        document.querySelector('#loading').remove()
        // 解析 Url 中的查询参数
        let q = new URLSearchParams(window.location.search)
        const uInt8Array = new Uint8Array(xhr.response);
        const db = new SQL.Database(uInt8Array);
        // Start: 拼装 SQL 语句
        let sql = 'SELECT id,vod_id,vod_name,type_id,type_name,vod_pic,vod_remarks,vod_year,vod_state FROM videos '
        let where = 'where'
        if (search()) {
            let prefix = ' '
            if (where !== 'where') {
                prefix = ' and '
            }
            let k = '"%' + search() + '%"'
            where += prefix + '(vod_name like ' + k + ' or vod_sub like ' + k + ' or vod_actor like ' + k + ' or vod_director like ' + k + ')'
        }
        if (where !== 'where') {
            sql += where
        }
        sql += ' order by updated_at desc,vod_year desc '
        if (thisPage() > 1) {
            sql += 'limit ' + 60 * (thisPage() - 1) + ',60'
        } else {
            sql += 'limit 60'
        }
        // End
        // 执行并渲染页面
        const contents = db.exec(sql);
        contents[0].values.forEach((v) => {
            createVideoItem(...v)
        })
    };
    xhr.send();
});
```

没有使用 react 或者 vue, 只是简单的 html + js, 页面效果使用 bootstrap 快速搭建的. 希望以上内容能对各位有启发~

> 代码有些粗糙, 勿喷

#### 说点什么

- GitHub 对文件有限制, 最大不可超过 100m, 超过 50m 的时候, 会有提醒, 而且文件过大之后, db 文件的拉取也会慢
- 通过对数据库不同的表进行拆分, 可有效的提升加载速度, 不同的内容可生成不同的数据库, 但是仍然需要全量的表用来搜索

#### Thanks

- [Bootstrap](https://getbootstrap.com/)
- [sql.js](https://sql.js.org/#/)

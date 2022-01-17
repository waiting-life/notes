### 异步

1. **pretty-markdown-pdf**

```js
// pretty-markdown-pdf

const fs = require('fs')
const path = require('path')
const prettyMdPdf = require('pretty-markdown-pdf')

function readdir(dir) {
  fs.stat(dir, (err, stat) => {
    if (err) {
      console.log(err)
    }
    if (stat.isDirectory()) {
      fs.readdir(dir, (err, fileList) => {
        if (err) {
          console.log(err)
        }
        for (const file of fileList) {
          if (file !== '.vuepress' && file !== '.DS_Store') {
            readdir(path.join(dir, file))
          }
        }
      })
    } else if (dir.endsWith('.md')) {
      fs.readFile(dir, 'utf8', (err, data) => {
        if (err) {
          console.log(err)
        }
        const paths = dir.split('/')
        const fileName = paths[paths.length - 1].replace('.md', '.pdf')

        prettyMdPdf.convertMd({
          markdownFilePath: dir,
          outputFilePath: path.join(__dirname, `../dist/${fileName}`),
        })
      })
    }
  })
}

fs.stat(path.join(__dirname, '../dist'), (err, stat) => {
  if (err) {
    fs.mkdirSync(path.join(__dirname, '../dist'))
  }
  readdir(path.join(__dirname, '../docs'))
})

```

2. **md-to-pdf**

```js
const fs = require('fs')
const path = require('path')
const { mdToPdf } = require('md-to-pdf')

function readdir(dir) {
  fs.stat(dir, (err, stat) => {
    if (err) {
      console.log(err)
    }
    if (stat.isDirectory()) {
      fs.readdir(dir, (err, fileList) => {
        if (err) {
          console.log(err)
        }
        for (const file of fileList) {
          if (file !== '.vuepress' && file !== '.DS_Store') {
            readdir(path.join(dir, file))
          }
        }
      })
    } else if (dir.endsWith('.md')) {
      fs.readFile(dir, 'utf8', async (err, data) => {
        if (err) {
          console.log(err)
        }
        const paths = dir.split('/')
        const fileName = paths[paths.length - 1].replace('.md', '.pdf')

        await mdToPdf(
          { content: data },
          { dest: path.join(__dirname, `../dist/${fileName}`) }
        )
      })
    }
  })
}

fs.stat(path.join(__dirname, '../dist'), (err, stat) => {
  if (err) {
    fs.mkdirSync(path.join(__dirname, '../dist'))
  }
  readdir(path.join(__dirname, '../docs'))
})

// ;(async () => {
//   const pdf = await mdToPdf({
//     path: path.join(__dirname, '../docs/html/6.判断元素进入可视区viewport.md'),
//   }).catch(console.error)

//   if (pdf) {
//     console.log('???', pdf)
//     fs.writeFileSync(path.join(__dirname, `../test.pdf`), pdf.content)
//   }
// })()
```



3. **md-html-pdf**

```js
const fs = require('fs')
const path = require('path')
const md = require('markdown-it')({
  html: false,
  linkify: false,
  typographer: true,
  highlight: function (str, lang) {
    let code = md.utils.escapeHtml(str)
    if (lang && hljs.getLanguage(lang)) {
      code = hljs.highlight(lang, str, true).value
    }
    return `<pre class="hljs"><code>${code}</code></pre>`
  },
})
const hljs = require('highlight.js')
const pdf = require('html-pdf')

function readdir(dir) {
  fs.stat(dir, (err, stat) => {
    if (err) {
      console.log(err)
    }
    if (stat.isDirectory()) {
      fs.readdir(dir, (err, fileList) => {
        if (err) {
          console.log(err)
        }
        for (const file of fileList) {
          if (file !== '.vuepress' && file !== '.DS_Store') {
            readdir(path.join(dir, file))
          }
        }
      })
    } else if (dir.endsWith('.md')) {
      fs.readFile(dir, 'utf8', (err, data) => {
        if (err) {
          console.log(err)
        }
        const paths = dir.split('/')
        const fileName = paths[paths.length - 1].replace('.md', '.pdf')
        const htmlFile = md.render(data)

        pdf
          .create(htmlFile)
          .toFile(path.join(__dirname, `../dist/${fileName}`), (err, res) => {
            if (err) return console.log(err)
            console.log(res)
          })
      })
    }
  })
}

fs.stat(path.join(__dirname, '../dist'), (err, stat) => {
  if (err) {
    fs.mkdirSync(path.join(__dirname, '../dist'))
  }
  readdir(path.join(__dirname, '../docs'))
})

```

4. **markdown-pdf**

```js
const fs = require('fs')
const path = require('path')
const markdownpdf = require('markdown-pdf')

function readdir(dir) {
  fs.stat(dir, (err, stat) => {
    if (err) {
      console.log(err)
    }
    if (stat.isDirectory()) {
      fs.readdir(dir, (err, fileList) => {
        if (err) {
          console.log(err)
        }
        for (const file of fileList) {
          if (file !== '.vuepress' && file !== '.DS_Store') {
            readdir(path.join(dir, file))
          }
        }
      })
    } else if (dir.endsWith('.md')) {
      fs.readFile(dir, 'utf8', (err, data) => {
        if (err) {
          console.log(err)
        }
        const fileName = dir.match(/.+\/(.+\.md)$/)?.[1]?.replace('md', 'pdf')

        markdownpdf()
          .from.string(data)
          .to(path.join(__dirname, `../dist/${fileName}`))

        // fs.createReadStream(dir)
        //   .pipe(markdownpdf())
        //   .pipe(fs.createWriteStream(path.join(__dirname, `../dist/${fileName}`)))
      })
    }
  })
}

fs.stat(path.join(__dirname, '../dist'), (err, stat) => {
  if (err) {
    fs.mkdirSync(path.join(__dirname, '../dist'))
  }
  readdir(path.join(__dirname, '../docs'))
})

```



### 同步

1. **md-pretty-pdf**

```js
const fs = require('fs')
const path = require('path')
const prettyMdPdf = require('pretty-markdown-pdf')

const mds = []
function readdir(dir) {
  const stat = fs.statSync(dir)
  if (stat.isDirectory()) {
    const fileList = fs.readdirSync(dir)
    for (const file of fileList) {
      if (file !== '.vuepress' && file !== '.DS_Store') {
        readdir(path.join(dir, file))
      }
    }
  } else if (dir.endsWith('.md')) {
    mds.push(dir)
  }
}

fs.stat(path.join(__dirname, '../dist'), async (err, stat) => {
  if (err) {
    fs.mkdirSync(path.join(__dirname, '../dist'))
  }
  readdir(path.join(__dirname, '../docs'))
  for (const md of mds) {
    const fileName = md.match(/.+\/(.+\.md)$/)?.[1]?.replace('md', 'pdf')
    await prettyMdPdf.convertMd({
      markdownFilePath: md,
      outputFilePath: path.join(__dirname, `../dist/${fileName}`),
    })
  }
})

```

2. **md-to-pdf**

```js
const fs = require('fs')
const path = require('path')
const { mdToPdf } = require('md-to-pdf')

const mds = []
function readdir(dir) {
  const stat = fs.statSync(dir)
  if (stat.isDirectory()) {
    const fileList = fs.readdirSync(dir)
    for (const file of fileList) {
      if (file !== '.vuepress' && file !== '.DS_Store') {
        readdir(path.join(dir, file))
      }
    }
  } else if (dir.endsWith('.md')) {
    mds.push(dir)
  }
}

fs.stat(path.join(__dirname, '../dist'), async (err, stat) => {
  if (err) {
    fs.mkdirSync(path.join(__dirname, '../dist'))
  }
  readdir(path.join(__dirname, '../docs'))
  for (const md of mds) {
    const fileName = md.match(/.+\/(.+\.md)$/)?.[1]?.replace('md', 'pdf')
    const pdf = await mdToPdf({ path: md })
    if (pdf) {
      fs.writeFileSync(path.join(__dirname, `../dist/${fileName}`), pdf.content)
    }
  }
})

```


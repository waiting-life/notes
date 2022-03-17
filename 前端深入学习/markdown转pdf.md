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



3. 优化markdown转pdf

```js
const fs = require('fs')
const path = require('path')
const { mdToPdf } = require('md-to-pdf')

class Scheduler {
  constructor(count) {
    this.count = count
    this.queue = []
    this.run = []
  }

  add(task) {
    this.queue.push(task)
    return this.schedule()
  }

  schedule() {
    if (this.run.length < this.count && this.queue.length) {
      const task = this.queue.shift()
      const promise = task().then(() => {
        this.run.splice(this.run.indexOf(promise), 1)
      })
      this.run.push(promise)
      return promise
    } else {
      return Promise.race(this.run).then(() => this.schedule())
    }
  }
}

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

const tarDir = process.argv?.[2] || 'pdf'
const tarDirPath = path.join(process.cwd(), tarDir)

const scheduler = new Scheduler(10)

fs.stat(tarDirPath, async (err, stat) => {
  if (err) {
    fs.mkdirSync(tarDirPath)
  }
  readdir(path.join(__dirname, '../docs'))

  const results = []
  const step = 10
  for (let i = 0; i < mds.length; i = i + step) {
    results.push(mds.slice(i, i + step))
  }

  for (const result of results) {
    await Promise.all(
      result.map((md) => {
        const fileName =
          Date.now() +
          '-' +
          md.match(/.+\/(.+\.md)$/)?.[1]?.replace('md', 'pdf')
        scheduler.add(async () => {
          const pdf = await mdToPdf({ path: md })
          if (pdf) {
            fs.writeFileSync(path.join(tarDirPath, fileName), pdf.content)
          }
        })
      })
    )
  }
})

```



```js
console.log('任务开始时间', new Date().toLocaleString())
console.log('任务结束时间', new Date().toLocaleString())
```

```js
for (const md of mds) {
    const fileName =
      Date.now() + '-' + md.match(/.+\/(.+\.md)$/)?.[1]?.replace('md', 'pdf')
    scheduler.add(async () => {
      const pdf = await mdToPdf({ path: md })
      if (pdf) {
        fs.writeFileSync(path.join(tarDirPath, fileName), pdf.content)
      }
    })
  }
```





```js
const fs = require('fs')
const path = require('path')
const { mdToPdf } = require('md-to-pdf')
const Promise = require('bluebird')

class Scheduler {
  constructor(count) {
    this.count = count
    this.queue = []
    this.run = []
  }

  add(task) {
    this.queue.push(task)
    return this.schedule()
  }

  schedule() {
    if (this.run.length < this.count && this.queue.length) {
      const task = this.queue.shift()
      const promise = task().then(() => {
        this.run.splice(this.run.indexOf(promise), 1)
      })
      this.run.push(promise)
      return promise
    } else {
      return Promise.race(this.run).then(() => this.schedule())
    }
  }
}

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

const tarDir = process.argv?.[2] || 'pdf'
const tarDirPath = path.join(process.cwd(), tarDir)

const scheduler = new Scheduler(10)

fs.stat(tarDirPath, async (err, stat) => {
  if (err) {
    fs.mkdirSync(tarDirPath)
  }
  readdir(path.join(__dirname, '../docs'))

  const results = []
  const step = 10
  for (let i = 0; i < mds.length; i = i + step) {
    results.push(mds.slice(i, i + step))
  }

  for (const result of results) {
    await Promise.all(
      result.map((md) => {
        const fileName =
          Date.now() +
          '-' +
          md.match(/.+\/(.+\.md)$/)?.[1]?.replace('md', 'pdf')
        // const pdf = await mdToPdf({ path: md })
        // if (pdf) {
        //   fs.writeFileSync(path.join(tarDirPath, fileName), pdf.content)
        // }
        scheduler.add(async () => {
          const pdf = await mdToPdf({ path: md })
          if (pdf) {
            fs.writeFileSync(path.join(tarDirPath, fileName), pdf.content)
          }
        })
      })
    )
  }
})

```





​	**mdToPdf遍历所有文件夹下面文件合并到两一个文件夹**

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

const tarDir = process.argv?.[2] || 'pdf'
const tarDirPath = path.join(process.cwd(), tarDir)

console.log('任务开始时间', new Date().toLocaleString())

fs.stat(tarDirPath, async (err, stat) => {
  if (err) {
    fs.mkdirSync(tarDirPath)
  }
  readdir(path.join(__dirname, '../docs'))

  const results = []
  const step = 10
  for (let i = 0; i < mds.length; i = i + step) {
    results.push(mds.slice(i, i + step))
  }

  for (const result of results) {
    await Promise.all(
      result.map(async (md) => {
        const fileName =
          Date.now() +
          '-' +
          md.match(/.+\/(.+\.md)$/)?.[1]?.replace('md', 'pdf')

        const pdf = await mdToPdf({ path: md })
        if (pdf) {
          fs.writeFileSync(path.join(tarDirPath, fileName), pdf.content)
        }
      })
    )
  }
  console.log('任务结束时间', new Date().toLocaleString())
})

```

**将一个目录里面的md文件转换为pdf并且带目录的放到另一个文件夹**

```js
const fs = require('fs-extra')
const path = require('path')
const rimraf = require('rimraf')
const { mdToPdf } = require('md-to-pdf')

class Scheduler {
  constructor(count) {
    this.count = count
    this.queue = []
    this.run = []
  }

  add(task) {
    this.queue.push(task)
    return this.schedule()
  }

  schedule() {
    if (this.run.length < this.count && this.queue.length) {
      const task = this.queue.shift()
      const promise = task().then(() => {
        this.run.splice(this.run.indexOf(promise), 1)
      })
      this.run.push(promise)
      return promise
    } else {
      return Promise.race(this.run).then(() => this.schedule())
    }
  }
}

const tarDir = process.argv?.[2] || 'pdf'
const tarDirPath = path.join(process.cwd(), tarDir)
const scheduler = new Scheduler(5)

const results = []
function readdir(dir) {
  fs.readdir(dir, (err, files) => {
    if (err) {
      console.log(err)
    }
    for (const file of files) {
      const filePath = path.join(dir, file)
      fs.stat(filePath, (err, stat) => {
        if (err) {
          console.log(err)
        }
        if (stat.isDirectory()) {
          readdir(filePath)
        } else if (filePath.endsWith('.md')) {
          scheduler.add(async () => {
            await mdToPdf({ path: filePath }).then((pdf) => {
              const newName = filePath.replace('.md', '.pdf')
              fs.renameSync(filePath, newName)
              fs.writeFile(newName, pdf.content, (err) => {
                if (err) {
                  console.log(err)
                }
              })
            })
          })
        }
      })
    }
  })
}

fs.stat(tarDirPath, async (err, stat) => {
  if (stat && stat.isDirectory()) {
    rimraf(tarDirPath, (err, data) => {
      if (err) {
        console.log(err)
      } else {
        copyDir()
      }
    })
  } else {
    copyDir()
  }
})

function copyDir() {
  const origin = path.join(__dirname, '../docs')
  fs.copy(origin, tarDirPath)
    .then(() => {
      readdir(tarDirPath)
    })
    .catch((err) => console.log(err))
}

```





**test**

```js
import fs from 'fs-extra'
import { fileURLToPath } from 'url'
import path, { dirname } from 'path'
import rimraf from 'rimraf'
import { mdToPdf } from 'md-to-pdf'

const __filename = fileURLToPath(import.meta.url)
const __dirname = dirname(__filename)

const tarDir = process.argv[2] || 'pdf'
const target = path.join(process.cwd(), tarDir)

class Scheduler {
  constructor(count) {
    this.count = count
    this.queue = []
    this.run = []
  }
  add(task) {
    this.queue.push(task)
    return this.schedule()
  }
  schedule() {
    if (this.run.length < this.count && this.queue.length) {
      const task = this.queue.shift()
      const promise = task().then(() => {
        this.run.splice(this.run.indexOf(promise), 1)
      })
      this.run.push(promise)
      return promise
    } else {
      return Promise.race(this.run).then(() => this.schedule())
    }
  }
}
const scheduler = new Scheduler(10)

fs.stat(target, async (err, stat) => {
  if (stat && stat.isDirectory()) {
    await rm(target)
    copyDir()
  } else {
    copyDir()
  }
})

async function copyDir() {
  const origin = path.join(__dirname, '../docs')
  try {
    await fs.copy(origin, target)
    readdir(target)
  } catch (error) {
    console.log(error)
  }
}

// 包装rimraf
function rm(path) {
  return new Promise((resolve) => rimraf(path, resolve))
}

// 递归处理文件
async function readdir(dir) {
  const files = await fs.readdir(dir)
  for (const file of files) {
    const filePath = path.join(dir, file)
    const stat = await fs.stat(filePath)
    if (stat.isDirectory()) {
      readdir(filePath)
    } else if (filePath.endsWith('.md')) {
      scheduler.add(async () => {
        const newFilePath = filePath.replace('.md', '.pdf')

        const pdf = await mdToPdf({ path: filePath })
        await rm(filePath)
        fs.outputFile(newFilePath, pdf.content)
      })
    }
  }
}

```

```js
const fs = require('fs')
const path = require('path')
const { mdToPdf } = require('md-to-pdf')
;(async () => {
  const pdf = await mdToPdf({ path: path.join(__dirname, '../docs/README.md') })
  fs.writeFileSync(path.join(__dirname, '../test/README.pdf'), pdf.content)
})()
```

**删除特定文件**

```js
function readdir(dir) {
  fs.readdir(dir, async (err, files) => {
    for (const file of files) {
      if (file === '.vuepress' || file === 'images') {
        rimraf(path.join(dir, file), (err, data) => {
          if (err) {
            console.log(err)
          }
        })
      } else {
        const filePath = path.join(dir, file)
        const stat = await fs.stat(filePath)
        if (stat.isDirectory()) {
          readdir(filePath)
        } else if (filePath.endsWith('.md')) {
          console.log(filePath)
        }
      }
    }
  })
}
```


<template>
  <div id="app">
    <StyleEditor ref="styleEditor" :code="currentStyle"></StyleEditor>
    <editor ref="editor" :markdown="currentMarkdown" :enableHtml="enableHtml"></editor>
  </div>
</template>

<script>
  import StyleEditor from './components/StyleEditor'
  import editor from './components/ResumeEditor'
  import './assets/reset.css'
  export default {
    name: 'app',
    components: {
      StyleEditor,
      editor
    },
    data() {
      return {
        interval: 50,
        currentStyle: '',
        enableHtml: false,
        fullStyle: [
          `/*
* Dear LJ
* 都说理科生的浪漫是泥石流
* 那么作为一个不折不扣的理科生
* 这次我要用我所学的知识书写理科生的浪漫
* 现在就让我用代码来为你呈现
* Show time, just enjoy❤️
*/

/* begin coding */
* {
  transition: all .3s;
}
/* 既然要浪漫, 白色背景太单调了, 那么什么颜色浪漫呢 */
html {
  color: rgb(255,69,0);
  background: rgb(255,182,193);
}
/* 修改下布局 */
.styleEditor {
  padding: .5em;
  border: 1px solid;
  overflow: auto;
  width: 90vw;
  margin: 2.5vh 5vw;
  height: 90%;
}

.styleEditor {
  height: 50%;
  position: fixed; left: 0; top: 0;
}
/* 让代码好看些 */
.token.selector{
  color: rgb(106,90,205);
}
.token.property{
  color: rgb(255,0,255);
}
.token.punctuation{
  color: white;
}
.token.function{
  color: rgb(219,112,147);
}

/* 来点3D效果 */
html{
  perspective: 1000px;
}
.styleEditor {
  transform: rotateX(-10deg) translateZ(-50px) ;
}

/* 接下来准备一张"纸"，该开始真正的表演了 */
.editor{
  position: fixed;
  top:50%; left: 0;
  padding: .5em;  margin: 2.5vh;
  width: 90vw; height: 45%;
  border: 1px solid; border-color: rgba(255,52,179,0.5);
  background: rgba(255,165,0,0.3); color: rgb(148,0,211);
  overflow: auto;
}

/* 好了，下面请听我说 */


`,
          `
/* ps: 这张"纸"是可以上下滑动的哦
 * 现在这张纸有点过于单调, 我们来把它转成 HTML
 */
`
          ,
          `
/* 先来点样式 */
.editor {
  background: rgba(255,165,0,0.3); color: rgb(148,0,211);
  padding: .5em;
  margin: 2.5vh 5vw;
  top:5%; left: 0;
  width: 90vw; height: 90%;
  position: fixed;
  border: 1px solid; border-color: rgba(255,52,179,0.5);
  overflow: auto;
}
.styleEditor {
  opacity: 0;
}
.editor h2{
  display: inline-block;
  border-bottom: 1px solid;
  margin: 1em 0 .5em;
}
`],
        currentMarkdown: '',
        fullMarkdown: `
<h2>Dear 刘洁:</h2></br>
<article>

<p>&nbsp;&nbsp;在茫茫人海中有幸认识了你，也许
这是一种缘分。感谢今年的快手 WWDC T 沙龙技术分享，
让我能有幸得以认识你，并且开始了我们的故事。不知道
你相不相信一见钟情，可能那天看到你后我就喜欢上了你。
这一切似乎都是上天的安排。
</p>

<p>&nbsp;&nbsp;你知道吗，那天第一次和你约会我的
心情是十分复杂的，有激动、兴奋又外加些许忐忑。激动
兴奋的是，很开心能够约你出来，忐忑的是怕自己不够好。
那晚，我们一起在鸟巢看了烟花，我感觉是很浪漫的事情，
后面我想和你一起去迪士尼看一次更浪漫的。
</p>

<p>&nbsp;&nbsp;和你聊了这么久后我发现你并没有我想像
中的那么高冷，反而是那么平易近人，和你聊天很愉快，这给
了我勇气还有信心来跟你说说我的心里话。
</p>

<p>&nbsp;&nbsp;北漂的日子，五味杂陈，工作日常两点一线
枯燥而又乏味，现在因为有你，我的生活开始多姿多彩，下班后
见到你就能抚平我上班的疲惫，一切似乎都在慢慢变好。
</p>

<p>&nbsp;&nbsp;我们从相遇到相爱不过短短一个月，但
是这一个月的时间说长也不长说短也不短，发生了很多让人
难忘的事情。感谢你对我的包容，陪我尬聊，让我有跟你
表白的勇气。余生很短，刚好有你，下面让我们一起书写
我们后面的篇章。
</p>

<p><font size="3" color=rgb(213,26,33)>
一屋
</font></p>
<p><font size="3" color=rgb(213,26,33>
二人
</font></p>
<p><font size="3" color=rgb(213,26,33>
三餐
</font></p>
<p><font size="3" color=rgb(213,26,33>
四季
</font></p>
<p><font size="3" color=rgb(213,26,33>
拥抱给你
</font></p>
<p><font size="3" color=rgb(213,26,33>
等待给你
</font></p>
<p><font size="3" color=rgb(213,26,33>
我的一切
</font></p>
<p><font size="3" color=rgb(213,26,33>
just for you
</font></p>

</article>

`
      }
    },
    created() {
      this.makeResume()
    },

    methods: {
      makeResume: async function () {
        await this.progressivelyShowStyle(0)
        await this.progressivelyShowResume()
        await this.progressivelyShowStyle(1)
        await this.showHtml()
        await this.progressivelyShowStyle(2)
      },
      showHtml: function () {
        return new Promise((resolve, reject) => {
          this.enableHtml = true
          this.$nextTick(() => {
            this.$refs.editor.goTop()
          })
          resolve()
        })
      },
      progressivelyShowStyle(n) {
        return new Promise((resolve, reject) => {
          let interval = this.interval
          let showStyle = (async function () {
            let style = this.fullStyle[n]
            if (!style) { return }
            // 计算前 n 个 style 的字符总数
            let length = this.fullStyle.filter((_, index) => index <= n).map((item) => item.length).reduce((p, c) => p + c, 0)
            let prefixLength = length - style.length
            if (this.currentStyle.length < length) {
              let l = this.currentStyle.length - prefixLength
              let char = style.substring(l, l + 1) || ' '
              this.currentStyle += char
              if (style.substring(l - 1, l) === '\n' && this.$refs.styleEditor) {
                this.$nextTick(() => {
                  this.$refs.styleEditor.goBottom()
                })
              }
              setTimeout(showStyle, interval)
            } else {
              resolve()
            }
          }).bind(this)
          showStyle()
        })
      },
      progressivelyShowResume() {
        return new Promise((resolve, reject) => {
          let length = this.fullMarkdown.length
          let interval = this.interval
          let showResume = () => {
            if (this.currentMarkdown.length < length) {
              this.currentMarkdown = this.fullMarkdown.substring(0, this.currentMarkdown.length + 1)
              let lastChar = this.currentMarkdown[this.currentMarkdown.length - 1]
              let prevChar = this.currentMarkdown[this.currentMarkdown.length - 2]
              if (prevChar === '\n' && this.$refs.editor) {
                this.$nextTick(() => this.$refs.editor.goBottom())
              }
              setTimeout(showResume, interval)
            } else {
              resolve()
            }
          }
          showResume()
        })
      }
    }
  }

</script>

<style scoped>
  #app {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    min-height: 100vh; position: relative;
  }

  html {
    min-height: 100vh;
  }
  *{
    box-sizing: border-box;
  }

</style>

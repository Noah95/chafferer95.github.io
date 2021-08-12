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
        interval: 60,
        currentStyle: '',
        enableHtml: false,
        fullStyle: [
          `/*
* Hello 梁小花,我是Chafferer
* 上次看到了一篇文章《你所学的专业该怎么表白》
* 那么这次我就用我的专业知识尝试一下
* 都说理科生不懂什么是浪漫
* 现在我就稍稍浪漫一下,那么接下来请看我的表演
* Emmm... 请欣赏
*/

/* begin coding */
* {
  transition: all .3s;
}
/* 既然要浪漫,白色背景太单调了,什么颜色浪漫呢 */
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

/* 接下来准备一张"纸",该开始真正的表演了 */
.editor{
  position: fixed;
  top:50%; left: 0;
  padding: .5em;  margin: 2.5vh;
  width: 90vw; height: 45%;
  border: 1px solid; border-color: rgba(255,52,179,0.5);
  background: rgba(255,165,0,0.3); color: rgb(148,0,211);
  overflow: auto;
}

/* 好了,下面请听我说 */


`,
          `
/* ps:这张"纸"是可以上下滑动的哦
 * 我想我也是开创了用HTML表白的先河了吧,哈哈哈
 * 现在样式有点丑, Emmm...现在我把它转成HTML
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
<h2>Dear 梁文珍:</h2></br>
<article>

<p>&nbsp;&nbsp;在茫茫人海中有幸认识了你,也许这
是一种缘分。还记得那晚你跟我道的晚安嘛，对独自在北
京的我来说这声晚安是多么温暖，你知道吗那晚真的被你
暖到了，也谢谢你的那声晚安让我得以认识你。
</p>

<p>&nbsp;&nbsp;你知道吗,曾经有段时间我特别后悔
选择了北京这座城市,这座城市对我来说是那样得陌生,
没有熟悉的朋友,离家还那么远,独自一人在外面,我想每
一个人都是害怕孤独的吧。感谢上天让我认识了你，让我
对以后的北京生活充满了憧憬，希望以后的日子能有你陪着。
</p>

<p>&nbsp;&nbsp;我知道也许现在说什么我喜欢你之类
的有点浮夸,毕竟我们尚未见过面,所有的交集都是在虚拟
的网络空间,但是我的心里活动我的种种行为告诉我我已经
喜欢上了你,我不想自己骗自己,更不想错过了你 >_<
</p>

<p>&nbsp;&nbsp;和你聊了几次后我发现你并没有我想像
中的那么高冷,反而是那么平易近人,和你聊天很愉快,我想
能够和你在一起肯定是一件很美妙的事情。
</p>

<p>&nbsp;&nbsp;艰难困苦,玉汝于成,我不管在追求美好
事物的道路上有多大的艰难险阻,我也不急着听你的答案,我
只希望你能够给我一个机会去走进你的内心,了解更多关于你,
我也将敞开我的心扉,我希望我们的故事才刚刚开始。
</p>

<p><font size="3" color=rgb(213,26,33)>
轻轻贴近你的耳朵
</font></p>
<p><font size="3" color=rgb(213,26,33>
情话永远不嫌太多,对你说
</font></p>
<p><font size="3" color=rgb(213,26,33>
一全听你的
</font></p>
<p><font size="3" color=rgb(213,26,33>
二给你好的
</font></p>
<p><font size="3" color=rgb(213,26,33>
数到三永远爱你一个
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

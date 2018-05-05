<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <transition-group name="fade" tag="div" class="position-absolute">
      <div v-for="(item,index) in data" v-if="index===pageIndex" :key="index" class="position-absolute">
        <h4>{{item.question}}</h4>
        <i>{{ index+1+'/'+data.length }}</i>
        <ul>
          <li v-for="(answersItem,answersIndex) in item.answers" :class="{'check': answersIndex+1 === item.currIndex}"
              @click="currCheck(item,answersIndex+1)">
            {{answersItem}}
          </li>
        </ul>
        <p class="float-right">
          <a href="javascript:;" v-text="setButton(index,data.length).text"
             :class="setButton(index,data.length).className" @click="nextPage(index)"></a>
        </p>
      </div>
    </transition-group>
    <router-link class="btn btn--red" tag="a" to="/address">{{this.score}}</router-link>
    <transition2></transition2>
  </div>

</template>

<script>
  import transition2 from './transition2';
  export default {
    name: 'HelloWorld',
    data() {
      return {
        msg: '这是一个测试项目',
        defaultButton: {
          'text': '',
          'ckassName': ''
        },
        correctAnswerArray: [],
        userAnswerArray:[],
        trueAnswerArray:[],
        trueAnswer: false,
        pageIndex: 0,
        score: 0,
        itemIndex: [],
        data: [{
          'question': 'jQuery是什么？',
          'answers': ['JavaScript库', 'CSS库', 'PHP框架', '以上都不是'],
          'correctAnswer': 1
        }, {
          'question': '找出不同类的一项?',
          'answers': ['写字台', '沙发', '电视', '桌布'],
          'correctAnswer': 3
        }, {
          'question': '国土面积最大的国家是：',
          'answers': ['美国', '中国', '俄罗斯', '加拿大'],
          'correctAnswer': 3
        }, {'question': '月亮距离地球多远？', 'answers': ['18万公里', '38万公里', '100万公里', '180万公里'], 'correctAnswer': 2}]
      }
    },
    mounted() {
      this.$nextTick(() => {

      })
    },
    methods: {
      setButton(index, length) {
        if (index === length - 1) {
          return this.defaultButton = {
            'text': '完成',
            'className': 'final'
          }
        }
        return this.defaultButton = {
          'text': '下一题',
          'className': 'next'
        }
      },
      currCheck(item, answersIndex) {
        this.$set(item, 'currIndex', answersIndex);
        if (typeof item.currIndex === "undefined") {
          this.$set(item, 'currIndex', answersIndex);

        }
        this.itemIndex.push(item.currIndex);
      },
      //  点击下一题进入下一个页面
      nextPage(index) {
        //  判断点击的是下一题还是已完成，如果是下一题则跳转，反之则已完成
        if (this.pageIndex < this.data.length - 1) {
          this.pageIndex = index + 1;
        } else if(this.pageIndex === this.data.length - 1) {
          //  消除数组会累加push的bug。在每一次push前先清空。
          this.correctAnswerArray = [];
          this.userAnswerArray = [];
          this.trueAnswerArray = [];
          this.data.forEach((item,dataIndex)=>{
            this.correctAnswerArray.push(item.correctAnswer);
            this.userAnswerArray.push(item.currIndex);
          });
          for(let i=0; i < this.correctAnswerArray.length;i++){

            if(this.correctAnswerArray[i] === this.userAnswerArray[i]){
               this.trueAnswer = true;
            }else{
              this.trueAnswer = false
            }
            this.trueAnswerArray.push(this.trueAnswer)
          }
          // console.log(this.correctAnswerArray);
          // console.log(this.userAnswerArray);
          // console.log(this.trueAnswerArray);
        }

      }
    },
    computed: {},
    components: {
      'transition2': transition2
    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .hello {
    width: 500px;
    height: 350px;
    margin: 20px auto;
    position: relative;
  }
  .position-absolute{
    position: absolute;
    left: 0;
    top: 40px;
    width: 100%
  }
  h1, h2 {
    font-weight: normal;
  }

  h4 {
    font-weight: bold;
  }

  .float-right {
    text-align: right;

  }

  .float-right a {
    background: red;
    color: #f0f0f0;
    padding: 0 5px;
    border-radius: 4px;
  }
  .btn{
    position: absolute;
    left: 50%;
    bottom: 0;
    transform: translateX(-50%);
  }
  ul {
    list-style-type: none;
    padding: 0;
  }

  li {
    margin: 20px;
  }

  .check {
    background: #ccc;
  }

  a {
    color: #42b983;
  }

  /* 动画的类 */
  .fade-enter-active, .fade-leave-active {
    transition: all 1s ease;
  }
  .fade-enter-active {
    opacity: 1;
  }

  .fade-leave-active,.fade-enter{
    opacity: 0;
  }



</style>

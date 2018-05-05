<template>
  <div class="box">
    <div class="content">{{ content }}</div>
    <input type="text" v-model="inputContent" @keyup.enter="validate()">
  </div>
</template>

<script>
  export default {
    name: "FillInTheBlanks",
    data() {
      return {
        content: 'I like that apple',
        inputContent: '',
        trueInput: false,
        trueInputArray: []
      }
    },
    methods: {
      validate() {
        //  将首尾的空格删掉

        // let regularInputContent = this.inputContent.replace(/^\s+|\s+$/gm,'');
        for (let i = 0; i < this.regularInputContent.length; i++) {
          if (this.contentArray.length === this.regularInputContent.length) {
            if (this.regularInputContent [i] === this.contentArray[i]) {
              this.trueInput = true;
            } else {
              this.trueInput = false;
              console.log(i)  //  获取到填写错误的数组
            }
            this.trueInputArray.push(this.trueInput)
          } else {
            console.log('错误')
          }

        }
        // console.log(this.contentArray);
        // console.log(this.regularInputContent);

        console.log(this.trueInputArray);
        this.inputContent = ''
      }
    },
    computed: {
      //  将输入框的值用正则过滤掉
      regularInputContent() {
        //  1.消除首尾的空格
        //  2.并且当单词间的空格有2个或2个以上时合为一个
        //  3.将表单中的值拆分为一个数组（方便查找错误的单词）
        return this.inputContent.replace(/^\s+|\s+$/gm, '').replace(/\s{2,}/g, ' ').split(" ");
      },
      //  将答案字段由字符串转为数组
      contentArray() {
        return this.content.split(" ")
      }
    }
  }
</script>

<style scoped>

</style>

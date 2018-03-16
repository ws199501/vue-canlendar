<template>
  <div class="canlendar">
    <p><router-link to="/">Go to hello</router-link></p>
    <h1>{{ msg }}<input v-model="year"><input v-model="month" @change="change(month)"></h1>
    <table class="box table">
      <th v-for="w in week" :key="w">星期{{w}}</th>
      <tr v-for="i in box.length/7" :key="i">
        <td v-for="j in 7" :key="j" v-bind:class="{other: box[(i - 1) * 7 + j - 1].other}">{{box[(i - 1) * 7 + j - 1].day}}</td>
      </tr>
    </table>
    <table class="box message">
      <tr v-for="i in box.length/7" :key="i">
        <td v-for="j in 7" :key="j" colspan="1">
          <div v-bind:class="{start: box[(i - 1) * 7 + j - 1].do}" @click="down()">
            {{box[(i - 1) * 7 + j - 1].title}}
          </div>
        </td>
      </tr>
    </table>
  </div>
</template>

<script>
export default {
  name: 'canlendar',
  data () {
    return {
      msg: 'canlendar',
      box: [],
      week: ['日', '一', '二', '三', '四', '五', '六'],
      year: 2018,
      month: 2,
      res: [
        {'date': '2018-01-01', 'title': '吃饭'},
        {'date': '2018-01-02', 'title': '吃饭'},
        {'date': '2018-01-03', 'title': '玩'},
        {'date': '2018-01-12', 'title': '吃饭'}
      ]
    }
  },
  mounted: function () {
    this.upd()
  },
  methods: {
    upd: function () {
      var arr = []
      // 这个月的第一天星期几
      var fistday = (new Date(this.year, this.month - 1, 1)).getDay()
      var f = fistday
      // 这个月几天
      var y = this.month === 12 ? this.year + 1 : this.year
      var m = this.month === 12 ? 1 : this.month
      var num = new Date(new Date(y, m, 1) - 1).getDate()
      // 上个月的最后一天是几号
      var lastday = new Date(new Date(this.year, this.month - 1, 1) - 1).getDate()
      var sum = num + fistday > 35 ? 42 : 35
      while (fistday-- > 0) {
        arr.unshift({'day': lastday--, 'other': 1})
      }
      var count = 1
      while (num-- > 0) {
        arr.push({'day': count++})
      }
      var countB = 1
      while (sum - arr.length > 0) {
        arr.push({'day': countB++, 'other': 1})
      }
      this.res.forEach(r => {
        var i = r.date ? parseInt(r.date.split('-')[2]) + parseInt(f) : null
        arr[i - 1]['do'] = 1
        arr[i - 1]['title'] = r.title
      })
      this.box = arr
    },
    change: function (v) {
      if (v < 13 && v > 0) {
        this.month = v
      } else {
        this.month = 1
      }
      this.upd()
    },
    down: function () {
      console.log('down')
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
* {
  margin: 0;
  padding: 0;
}
h1, h2 {
  font-weight: normal;
}
.canlendar {
  width: 100%;
  height: 80vh;
}
.box {
  position: absolute;
  left: 10%;
  width: 80%;
  height: 80%;
  border-collapse: collapse;
}
.message {
  margin-top: 30px;
  height: calc(80% - 30px);
}
.message td div {
  margin: 50% 10% 0 10%;
  box-sizing: border-box;
  height: 28px;
  line-height: 28px;
}
.box td {
  width: calc(100% / 7);
}
.table td {
  border: 1px solid #ccc;
}
.box th {
  box-sizing: border-box;
  height: 30px;
}
.table td.other {
  background: #eee;
}
.start {
  background-color: #c6cde4;
  border-radius: 15px;
}
</style>

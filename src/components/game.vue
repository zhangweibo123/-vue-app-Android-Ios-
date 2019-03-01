<template>
  <div class="game">
    <p v-if="this.counter%2 ==0">请黑棋落子</p>
    <p v-if="this.counter%2 !==0">请白棋落子</p>
    <div class="gridFather">
      <table>
        <tr v-for="m in 15">
          <td v-for="n in 15" @click="downClick($event)"></td>
        </tr>
      </table>
      <!-- <div class="grid" v-for="i in 225" @click="downClick($event)">{{i}}</div> -->
    </div>
  </div>
</template>

<script>
export default {
  name: "game",
  data() {
    return {
      counter: 0,
      dotarray: ["", "", "", ""]
    };
  },
  methods: {
    downClick(e) {
      if ($(e.currentTarget).html() !== "") {
        return false;
      }
      this.counter++;
      if (this.counter % 2 == 0) {
        e.currentTarget.innerHTML = "<div class='whiteDot'></div>";
      } else {
        e.currentTarget.innerHTML = "<div class='blackDot'></div>";
      }
    //   this.$nextTick(function(){
    //       console.log('dddddddddd')
    //   })
      setTimeout(function(){
        this.isSuccess(e);
      }.bind(this),1000)
      
    },
    //  判断是否获胜
    isSuccess(e) {
      // 清空一下
      this.dotarray = ["", "", "", ""];
      var tds = document.getElementsByTagName("td");
      var curr = {
        x: e.target.cellIndex,
        y: e.target.parentElement.rowIndex,
        color: this.counter % 2 !== 0 ? "b" : "w"
      };
      for (var i = 0, tmp = {}; i < 225; i++) {
        tmp = {
          x: tds[i].cellIndex,
          y: tds[i].parentElement.rowIndex,
          color:
            $(tds[i])
              .find("div")
              .attr("class") == "blackDot"
              ? "b"
              : $(tds[i])
                  .find("div")
                  .attr("class") == "whiteDot"
              ? "w"
              : "."
        };

        if (curr.y == tmp.y) {
          // 在一个横线上
          this.dotarray[0] += tmp.color;
        }
        if (curr.x == tmp.x) {
          // 在一个竖线上
          this.dotarray[1] += tmp.color;
        }
        if (curr.x + curr.y == tmp.x + tmp.y) {
          //在左斜线上
          this.dotarray[2] += tmp.color;
        }
        if (curr.x - tmp.x == curr.y - tmp.y) {
          //在右斜线上
          this.dotarray[3] += tmp.color;
        }
      }
      // 判断4条线上，有没有连续的4个w,或4个b
      var wincolor = this.counter % 2 == 0 ? "wwwww" : "bbbbb";
      // 判断一下每条线
      for (var i = 0; i < 4; i++) {
        if (this.dotarray[i].indexOf(wincolor) >= 0) {
          if (this.counter % 2 !== 0) {
            alert("黑棋赢");
          } else alert("白棋赢");
          break;
        }
      }
      // console.log(this.dotarray)
    }
  }
};
</script>
<style>
.gridFather {
  width: 37.5vw;
  cursor: pointer;
  margin: auto;
}
.gridFather table td {
  border: 1px solid #dddddd;
  width: 2.5vw;
  height: 2.5vw;
}
.blackDot {
  width: 80%;
  height: 80%;
  background-image: linear-gradient(217deg, #673322 0%, #000000 100%);
  border-radius: 50%;
  border: 1px solid #dddddd;
  margin: auto;
  margin-top: 10%;
}
.whiteDot {
  width: 80%;
  height: 80%;
  background-image: linear-gradient(135deg, #defcf9 0%, #c3bef0 100%);
  border-radius: 50%;
  border: 1px solid #dddddd;
  margin: auto;
  margin-top: 10%;
}
</style>

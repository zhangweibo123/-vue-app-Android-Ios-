<template>
  <div class="game">
    <div style="clear:both;overflow:hidden;margin:auto;width:37.5vw;;margin:10px auto">
      <span style="float:left;font-size:22px;">web五子棋</span> 
      <div class='blackDot' style="width:20px;height:20px;float:right;display: inline-block;margin: 0px;margin-left:10px" v-if="this.counter%2 ==0"></div>
      <div class='whiteDot' style="width:20px;height:20px;float:right;display: inline-block;margin: 0px;margin-left:10px" v-if="this.counter%2 !==0"></div>
    </div>
  
    <div class="gridFather">
      <table>
        <tr v-for="m in 15">
          <td v-for="n in 15" @click="flag && downClick($event)"></td>
        </tr>
      </table>
    </div>
    <div style="margin-top:10px">
      <el-button :disabled = "counter <= 0 || !this.flag" @click="goBack">悔棋</el-button>
      <el-button type="primary" @click="againNew">重新开局</el-button>
    </div>
    
  </div>
</template>

<script>
export default {
  name: "game",
  data() {
    return {
      counter: 0,
      dotarray: ["", "", "", ""],
      flag:true
    };
  },
  methods: {
    downClick(e) {
      if ($(e.currentTarget).html() !== "") {
        return false;
      }
      this.counter++;
      if (this.counter % 2 == 0) {
        e.currentTarget.innerHTML = "<div class='whiteDot' id=q" + this.counter + "></div>";
      } else {
        e.currentTarget.innerHTML = "<div class='blackDot'  id=q" + this.counter + "></div>";
      }
      this.isSuccess(e);
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
          this.flag = false
          if (this.counter % 2 !== 0) {
            this.$alert('黑棋获胜', {
              confirmButtonText: '确定',
            });
          } else{
               this.$alert('白棋获胜', {
                confirmButtonText: '确定',
              });
          };
          break;
        }
      }
      // console.log(this.dotarray)
    },
    // 悔棋功能
    goBack() {
      let thisid = '#q' + this.counter
      $(thisid).remove();
      this.counter --
    },
    // 重新开始
    againNew() {
      $( "div[id*=q]" ).remove();
      this.flag = true
      this.counter = 0
    }
  }
};
</script>
<style>
.game{width: 80%;margin: auto;text-align: center}
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
table{
  border-collapse:collapse;
}

@media screen and (max-width: 700px) {
  .gridFather {
      width: 90vw;
      margin: auto
    }
  .gridFather table td {
      width: 6vw;
      height: 6vw;
    }
    .game{width: 100%}
}
</style>

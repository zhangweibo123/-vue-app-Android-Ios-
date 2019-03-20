<template>
  <div class="game">
    <iframe frameborder="no" border="0" marginwidth="0" marginheight="0" 
    width=330 height=86 src="https://music.163.com/outchain/player?type=2&id=1336971085&auto=1&height=66"></iframe>
    <div id="thistitle">
      <span style="float:left;font-size:22px;">五子棋</span> 
      <div class='blackDot' style="width:20px;height:20px;float:right;display: inline-block;margin: 0px;margin-left:10px" v-if="this.counter%2 ==0"></div>
      <div class='whiteDot' style="width:20px;height:20px;float:right;display: inline-block;margin: 0px;margin-left:10px" v-if="this.counter%2 !==0"></div>
      <div style="width:120px;height:40px;float:right;display: inline-block;margin: 0px;margin-left:10px">
          {{oppenDanger?'危险检测已开启':''}}
      </div>
    </div>
  
    <div class="gridFather">
      <table>
        <tr v-for="m in 15">
          <td v-for="n in 15" @click="flag && downClick($event)"></td>
        </tr>
      </table>
    </div>
    <div class="bottons">
      <el-button :disabled = "counter <= 0 || !this.flag" @click="goBack">悔棋</el-button>
      <el-button type="primary" @click="againNew">重开</el-button>
      <el-button type="primary" @click="oppenDanger = !oppenDanger">{{oppenDanger?'关闭':'开启'}}危检</el-button>
      <el-button type="primary" @click="opennumber">计数</el-button>

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
      flag:true,
      oppenDanger:true,
      isNumber:false,
    };
  },
  methods: {
    downClick(e) {
      if ($(e.currentTarget).html() !== "") {
        return false;
      }
      this.counter++;
      if (this.counter % 2 == 0) {
        e.currentTarget.innerHTML = `<div class='whiteDot' id=q${this.counter}><span class="displayNum">${this.counter}</span></div>`;
      } else {
        e.currentTarget.innerHTML = `<div class='blackDot' id=q${this.counter}><span class="displayNum">${this.counter}</span></div>`;
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

      if(this.oppenDanger){
        this.danger(this.dotarray)
      }else{}
    },
    // 危险检测
    danger(arr) {
      let blackDan1 = '.bbb.'
      let blackDan2 = '.bbbb.'
      let blackDan3 = 'wbbbb.'
      let blackDan4 = '.bbbbw'
      let blackDan5 = '.bb.b.'
      let blackDan6 = '.b.bb.'
      let blackDan7 = 'bbb.b'
      let blackDan8 = 'bb.bb'
      let blackDan9 = 'b.bbb'

      let whiteDan1 = '.www.'
      let whiteDan2 = '.wwww.'
      let whiteDan3 = 'bwwww.'
      let whiteDan4 = '.wwwwb'
      let whiteDan5 = '.ww.w.'
      let whiteDan6 = '.b.bb.'
      let whiteDan7 = 'www.w'
      let whiteDan8 = 'ww.ww'
      let whiteDan9 = 'w.www'
      if (this.counter % 2 !== 0) {
          for (var i = 0; i < 4; i++) {
            if (
              arr[i].indexOf(blackDan1) >= 0 
            || arr[i].indexOf(blackDan2) >= 0 
            || arr[i].indexOf(blackDan3) >= 0 
            || arr[i].indexOf(blackDan4) >= 0 
            || arr[i].indexOf(blackDan5) >= 0
            || arr[i].indexOf(blackDan6) >= 0
            || arr[i].indexOf(blackDan7) >= 0
            || arr[i].indexOf(blackDan8) >= 0
            || arr[i].indexOf(blackDan9) >= 0
            ) {
              this.$message({
                  message: '黑棋危险',
                  type: 'warning'
              });
            }
          }
      }else{
          for (var i = 0; i < 4; i++) {
            if (
                 arr[i].indexOf(whiteDan1) >= 0 
            || arr[i].indexOf(whiteDan2) >= 0 
            || arr[i].indexOf(whiteDan3) >= 0 
            || arr[i].indexOf(whiteDan4) >= 0 
            || arr[i].indexOf(whiteDan5) >= 0
            || arr[i].indexOf(whiteDan6) >= 0
            || arr[i].indexOf(whiteDan7) >= 0
            || arr[i].indexOf(whiteDan8) >= 0
            || arr[i].indexOf(whiteDan9) >= 0             
            ) {
               this.$message({
                  message: '白棋危险',
                  type: 'warning'
                });
            }
          }
      }

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
    },
    //计数功能
    opennumber() {
      this.isNumber = !this.isNumber
      if(this.isNumber){
        $('.displayNum').css('display','none')
      }else{
        $('.displayNum').css('display','inline')
      }
      
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
  font-size: 12px;
  background-image: linear-gradient(217deg, #673322 0%, #000000 100%);
  border-radius: 50%;
  border: 1px solid #dddddd;
  margin: auto;
  margin-top: 10%;
  color: white
}
.whiteDot {
  width: 80%;
  height: 80%;
  font-size: 12px;
  background-image: linear-gradient(135deg, #defcf9 0%, #c3bef0 100%);
  border-radius: 50%;
  border: 1px solid #dddddd;
  margin: auto;
  margin-top: 10%;
}
table{
  border-collapse:collapse;
}
#thistitle{clear:both;overflow:hidden;margin:auto;width:37.5vw;margin:10px auto}
@media screen and (max-width: 700px) {
  #thistitle{width:90vw;}
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
.displayNum{display: none;}
.bottons button{margin-top: 20px}
</style>

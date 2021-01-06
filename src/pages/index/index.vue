<template>
  <view class="pageIndex">
    <view class="btnBoxbig">
      <view class="btnbox2">
        <button
          class="headBtn"
          type="primary"
          :plain="!plain"
          @tap="changePlain(true)"
        >
          单选
        </button>
        <button
          class="headBtn"
          type="primary"
          :plain="plain"
          @tap="changePlain(false)"
        >
          多选
        </button>
      </view>
      <view class="btnbox2">
        <button
          v-for="i in 10"
          :key="i"
          class="checkBtn"
          type="primary"
          :plain="plainIndex != i"
          @tap="checkBtn(i)"
        >
          选 {{ i }}
        </button>
      </view>
    </view>
    <view class="st">
      <view v-if="plain" class="M1">
        <view
          v-for="i in 80"
          :key="i"
          :class="{
            numD: true,
            colorRed: getColor(i),
            animations: getColor(i) && !btnFlag,
          }"
          >{{ i }}</view
        >
      </view>
      <view v-if="!plain" class="M2">
        <view class="plain2Btn">
          <button
            v-for="i in 5"
            :key="i"
            class="checkBtn2"
            type="warn"
            :plain="plainIndexM2 != i"
            @tap="changeM2plain(i)"
          >
            {{ i }} 个
          </button>
        </view>
        <view class="plain2boxs">
          <view v-for="(arr, idx) in selectArr2" :key="idx" class="items">
            <view v-for="i in arr" :key="i" class="pie">{{ i }}</view>
          </view>
        </view>
      </view>
    </view>
    <button class="btns" type="primary" @tap="changeBtn">
      {{ btnFlag ? "停止" : "开始" }}
    </button>
  </view>
</template>

<script>
export default {
  name: "Index",
  components: {},
  data() {
    return {
      btnFlag: false,
      plainIndex: 5,
      plain: true,
      selectArr: [],
      plainIndexM2: 2,
      selectArr2: [],
    };
  },
  computed: {
    getColor(i) {
      return function (i) {
        for (const idx of this.selectArr) {
          if (idx == i) {
            return true;
          }
        }
        return false;
      };
    },
  },
  methods: {
    changeM2plain(i) {
      this.plainIndexM2 = i;
      let arr = [];
      for (let index = 1; index <= i; index++) {
        arr.push(this.arrDeWeight());
      }
      this.selectArr2 = arr;
    },
    changePlain(type) {
      this.btnFlag = false;
      this.plain = type;
      this.changeM2plain(this.plainIndexM2);
    },
    arrDeWeight() {
      let newArr = [];
      for (let index = 1; index <= 50; index++) {
        let num = Math.ceil(Math.random() * 80);
        if (newArr.indexOf(num) === -1) {
          newArr.push(num);
        }
        if (newArr.length === this.plainIndex) {
        return newArr;
      }
      }
    },
    getNewArr(arr) {
      let a = this.plainIndex - arr.length;
      for (let index = 1; index <= a; index++) {
        let num = Math.ceil(Math.random() * 80);
        if (arr.indexOf(num) == -1) {
          arr.push(num);
        } else {
          this.getNewArr(arr);
          return;
        }
      }
      return arr;
    },
    changeBtn() {
      if (this.plain) {
        let timer = setInterval(() => {
          if (!this.btnFlag) {
            console.log("清除定时器");
            clearInterval(timer);
            return;
          }
          this.selectArr = this.arrDeWeight();
        }, 100);
      } else {
        let timer = setInterval(() => {
          if (!this.btnFlag) {
            console.log("清除定时器");
            clearInterval(timer);
            return;
          }
          let arr = [];
          for (let index = 1; index <= this.plainIndexM2; index++) {
            arr.push(this.arrDeWeight());
          }
          this.selectArr2 = arr;
        }, 100);
      }
      this.btnFlag = !this.btnFlag;
    },
    checkBtn(num) {
      this.plainIndex = num;
      this.changeM2plain(this.plainIndexM2);
    },
  },
};
</script>

<style lang="less">
.pageIndex {
  width: 100vw;
  height: 100vh;
  background: #000000;
  position: relative;
  .btns {
    position: fixed;
    bottom: 0;
    left: 0;
    width: 100%;
    height: 160px;
    line-height: 160px;
    font-size: 60px;
    border: 0;
    border-radius: 0;
    color: #fff;
    z-index: 99;
  }
  .st {
    width: 100%;
    height: 660px;
    position: fixed;
    bottom: 170px;
    .M1 {
      width: 710px;
      padding: 20px;
      padding-top: 40px;
      display: flex;
      justify-content: space-around;
      flex-wrap: wrap;
      .numD {
        width: 56px;
        height: 56px;
        margin: 8px 6px;
        line-height: 56px;
        border-radius: 50%;
        border: 1px solid #fff;
        color: #fff;
        text-align: center;
      }
      .colorRed {
        background: #f71b1b;
      }
      .animations {
        animation: bounce-in 1s;
        animation-delay: 0.5s;
      }
      @keyframes bounce-in {
        0% {
          transform: scale(0.8);
        }
        50% {
          transform: scale(1.5);
        }
        100% {
          transform: scale(1);
        }
      }
    }
    .M2 {
      width: 100%;
      height: 100%;
      padding: 10px 0;
      .plain2Btn {
        padding: 10px 0;
        width: 700px;
        height: 100px;
        margin: 0 auto;
        display: flex;
        flex-wrap: nowrap;
        justify-content: space-around;
        .checkBtn2 {
          height: 6vh;
          line-height: 6vh;
          width: 16vw;
          margin: 2vw;
          font-size: 3vw;
        }
      }
      .plain2boxs {
        width: 100%;
        height: 450px;
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        padding: 6px 0;
        .items {
          width: 100%;
          height: 66px;
          margin: 8px 0;
          border-left: 10px solid #f71b1b;
          display: flex;
          justify-content: center;
          align-items: center;
          .pie {
            width: 56px;
            height: 56px;
            margin: 8px 6px;
            line-height: 56px;
            border-radius: 50%;
            background: #df2a2a;
            color: #fff;
            text-align: center;
          }
        }
      }
    }
  }
  .btnBoxbig {
    width: 100%;
    height: 280px;
    position: fixed;
    bottom: 840px;
    .btnbox2 {
      padding: 8px 0;
      display: flex;
      justify-content: space-around;
      flex-wrap: wrap;
      .headBtn {
        height: 80px;
        line-height: 80px;
        width: 220px;
      }
      .checkBtn {
        height: 72px;
        line-height: 72px;
        width: 120px;
        margin: 8px 4px;
        font-size: 28px;
        padding: 0;
        text-align: center;
      }
    }
  }
}
</style>

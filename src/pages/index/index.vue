<template>
  <view class="pageIndex">
    <view class="banner-layout">
<!--      <ad unit-id="adunit-aa9793e7ad4a60d3" ad-intervals="30"></ad>-->
      <!-- 小程序2 -->
<!--      <ad unit-id="adunit-438b892f7f05cd30" ad-intervals="30"></ad>-->
    </view>
    <view class="btnBoxbig">
      <view class="btnbox2">
        <button
          class="headBtn"
          type="primary"
          :plain="!plain"
          @tap="changePlain(true)"
        >
          单选模式
        </button>
        <button
          class="headBtn"
          type="primary"
          :plain="plain"
          @tap="changePlain(false)"
        >
          多选模式
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
        >{{ i }}
        </view
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
            {{ i }} 组
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
    onShareAppMessage() {
      return {
        title: '快8助手-便捷选号',
        path: "pages/index/index",
        imageUrl: 'http://file.koolss.com/img/20210108154605.jpg'
      }
    },
    onShareTimeline(){
      return {
        title: '快8助手-便捷选号',
        path: "pages/index/index",
        imageUrl: 'http://file.koolss.com/img/20210108154605.jpg'
      }
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
      }
    },
  };
</script>

<style lang="less">
  .banner-layout {
    width: 100vh;
    height: 300px;
  }

  .pageIndex {
    width: 100vw;
    height: 100vh;
    background: #000000;
    position: fixed;

    .btns {
      position: fixed;
      bottom: 0;
      left: 0;
      width: 100%;
      height: 120px;
      line-height: 120px;
      font-size: 60px;
      border: 0;
      border-radius: 0;
      color: #fff;
      z-index: 99;
    }

    .st {
      margin: auto;
      width: 730px;
      position: relative;
      //border: 1px solid #fff;
      height: 566px;
      .M1 {
        margin: auto;
        padding: 10px;
        display: flex;
        justify-content: space-around;
        flex-wrap: wrap;

        .numD {
          width: 50px;
          height: 50px;
          margin: 6px 8px;
          line-height: 50px;
          border-radius: 50%;
          border: 2px solid #fff;
          color: #fff;
          text-align: center;
        }

        .colorRed {
          background: #f71b1b;
        }

        .animations {
          animation: bounce-in 1s;
          animation-delay: 0.2s;
        }

        @keyframes bounce-in {
          0% {
            transform: scale(1);
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
        .plain2Btn {
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
          height: 400px;
          display: flex;
          flex-direction: column;
          justify-content: center;
          align-items: center;
          .items {
            width: 100%;
            height: 60px;
            margin: 5px 0;
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
      margin: auto;
      width: 730px;
      position: relative;
      .btnbox2 {
        padding: 2px 0;
        display: flex;
        justify-content: space-around;
        flex-wrap: wrap;
        .headBtn {
          height: 60px;
          line-height: 60px;
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

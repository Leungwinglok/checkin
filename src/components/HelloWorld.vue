<template>
  <div class="hello">
    <p
      class="dinglan"
      style="white-space: nowrap;text-align: center;color: #cd4606;font-size: 300%;"
    >上班打卡请在9点半后再进行操作，下班打卡在18：30后进行操作</p>
    <!--<img class="touxiang" src="../assets/Refresh.png" @click="update">-->
    <!-- <i class="el-icon-refresh" @click="update"></i> -->
    <!-- <span class="text">点击左边图片可刷新！！！</span> -->
    <div class="block"></div>
    <div class="note" :style="note"></div>
    <el-carousel :interval="4000" type="card" height="500px">
      <el-carousel-item v-for="(item,key) in files" :key="key">
        <img class="img" :src="item">
      </el-carousel-item>
    </el-carousel>

    <!-- <el-carousel class="kapian" :interval="4000" type="card" height="300px">
            <el-carousel-item  v-for="(item,key) in imgList" :key="key">
                <img class="img" :src="item.src">
            </el-carousel-item>
    </el-carousel>-->
    <!-- <el-calendar class="timetable"
        v-model="value">
    </el-calendar>-->

    <el-table
      :data="tableData"
      :stripe="false"
      style="width: 100%;background: transparent;color: #fdf6ec;margin: 0 auto;"
    >
      <el-table-column fixed prop="id" label="序号" width="60" align="center" type="index"></el-table-column>
      <el-table-column fixed prop="deviceid" label="设备ID" width="180" align="center"></el-table-column>
      <el-table-column fixed prop="userid" label="手机号码" width="180" align="center"></el-table-column>
      <el-table-column fixed prop="name" label="原名" align="center"></el-table-column>
      <el-table-column fixed prop="nicname" label="马甲" align="center"></el-table-column>
      <el-table-column fixed prop="result" label="结果" align="center">
        <template slot-scope="result">
          <el-tag
            :type=" result.row.result == 1 ? 'success' : 'danger'"
          >{{ result.row.result == 1 ? '成功' : '失败'}}</el-tag>
        </template>
      </el-table-column>
      <el-table-column fixed prop="begintime" label="开始打卡时间" align="center"></el-table-column>
      <el-table-column fixed prop="endtime" label="打卡结束时间" align="center"></el-table-column>
      <el-table-column fixed="right" label="操作" width="100">
        <template slot-scope="wakeup">
          <el-button @click="handlewakeup(wakeup.row.deviceid)" type="text" size="small">唤起</el-button>
        </template>
      </el-table-column>
    </el-table>
    <el-carousel :interval="4000" type="card" height="200px"></el-carousel>
  </div>
</template>
<script>
import axios from "axios";

export default {
  name: "HelloWorld",
    data() {
    return {
      tableData: [],
      imgList: [
        // {
        //   src: require("../assets/1.jpeg")
        // },
        // {
        //   src: require("../assets/2.jpeg")
        // },
        // {
        //   src: require("../assets/3.jpeg")
        // },
        // {
        //   src: require("../assets/4.jpeg")
        // },
        // {
        //   src: require("../assets/5.jpeg")
        // },
        // {
        //   src: require("../assets/6.jpeg")
        // }
      ],
      note: [
        {
          backgroundImage: "url(" + require("../assets/save.jpg") + ")",
          backgroundRepeat: "no-repeat",
          backgroundSize: "25px auto",
          marginTop: "5px"
        }
      ],
      value: new Date(),
      files:[]
    };
  },
  created() {
    this.update();
  },
  mounted(){
    new Promise((resolve) => {
      this.getFiles()
      if (this.files.length > 0) {
        resolve()
      }
    }).then(() => {
      // console.log(this.files)
      // this.preload(this.files)
    }).catch(() => {
      
    })

  },
  methods: {
    update() {
      axios.get("http://192.168.88.13:5000/queryrecord").then(res => {
        //                    alert(res)
        this.tableData = res.data.data;
      });
    },
    handlewakeup(id) {
      axios.get(`http://192.168.88.13:5000/wakedevice?deviceid=${id}`);
    },
   getFiles () {
      const files = require.context('../assets/image', false, /.(png|jpg|jpeg|gif|bmp|webp)$/).keys()
      for (let item of files) {
        let file = require('../assets/image/' + item.split('/')[1])

        // base64的不加载
        if (file.indexOf('data:') !== 0) {
          this.files.push(file)
        }
      }
    },
  }
};
</script>

 <!--Add "scoped" attribute to limit CSS to this component only-->
<style>
.hello {
  width: 100vw;
  height: 100vh;
  background: url("../assets/save.jpg") no-repeat;
  /* background-position: center; */
  background-size: cover;
  overflow: auto;
}
.el-carousel__item h3 {
  color: #475669;
  font-size: 14px;
  opacity: 0.75;
  line-height: 200px;
  margin: 0;
}
.el-carousel__item:nth-child(2n) {
  background-color: transparent;
}

.el-carousel__item:nth-child(2n + 1) {
  background-color: transparent;
}

.el-table th,
.el-table tr {
  background: transparent;
}

.img {
  width: 100%;
  height: 500px;
}
/* .el-icon-refresh{
        font-size: 40px;
        color: #13ce66;
        float: left;
        font-size: 50px;
        
    } */
.carousel-item {
  background: transparent !important;
}
.text {
  /* margin: 10px ;
        font-family: "League-Gothic", Courier;
        text-transform: uppercase;
        font-size: 80px;
        line-height: 90px;
        text-shadow: 0 0 20px #fefcc9, 10px -10px 30px #feec85, -20px -20px 40px #ffae34, 20px -40px 50px #ec760c, -20px -60px 60px #cd4606, 0 -80px 70px #973716, 10px -90px 80px #451b0e; */
  margin-top: 10px;
  float: left;
  font-size: 35px;
}
.el-carousel__item h3 {
  color: #475669;
  font-size: 14px;
  opacity: 0.75;
  line-height: 200px;
  margin: 0;
}
.el-table thead {
  color: #000000;
}
.kapian {
  width: 75%;
  margin: 0 auto;
}
.touxiang {
  width: 350px;
  height: 200px;
}
.el-table__row {
  color:white(212, 64, 19, 0.473);
}
 .el-table__body tr.hover-row>td { 
   background-color: purple !important; 
}
 
/* .timetable {
        margin-left: 70%;
        font-size: 5px;
        background-color:lightblue;
        
    } */
.dinglan {
  width: 100%;
  background-color: yellow;
  margin-top: 0pc;
}
.el-carousel__item h3 {
    color: #475669;
    font-size: 18px;
    opacity: 0.75;
    line-height: 300px;
    margin: 0;
  }
  
  .el-carousel__item:nth-child(2n) {
    background-color: transparent;
  }
  
  .el-carousel__item:nth-child(2n+1) {
    background-color: transparent;
  }
</style>

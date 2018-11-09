<template>
    <div>
        <div class="allwidth topTitle">
            <div class="content flexItem flexBetween">
                <div class="indexNav flexItem flexBetween">
                    <div class="flexRow flexItem flexBetween">
                        <a href="javascript:void(0)" class="logo"></a>
                        <div class="flexRow navTitle">
                            <a href="JavaScript:void(0)">首页</a>
                            <!-- <a href="JavaScript:void(0)">发现</a> -->
                        </div>
                    </div> 
                    <div class="flexRow flexItem">
                        <el-input v-model="input" class="clinput" size="small" placeholder="请输入内容"></el-input><el-button size="small" @click="open3" type="primary">提问</el-button>
                    </div>
                </div>
                <div class="indexRight flexItem flexEnd">   
                        <div class="indexRightContent flexAround">
                            <a href="javascript:void(0)" class="msg flexCenterCenter"><i class="icon iconfont icon-lingsheng color8590a6"></i><span>89</span></a>
                            <a href="javascript:void(0)" class="msg flexCenterCenter"><i class="icon iconfont icon-duihua color8590a6"></i><span>100</span></a>
                            <span class="headerImg"></span>
                        </div>
                </div>
            </div>
        </div>
        <div class="content flexBetween padding10">
            <div class="indexNav flexBetween backfff">
                <el-tabs v-model="activeName" class="indexTab" @tab-click="handleClick">
                    <el-tab-pane label="用户管理" name="first" class="is-active paddingB20">
                        <div class="paddingTB10 borderBottomf0f2f7" v-for="item in newsList" :key="item.id">
                             <div class="fontBold marginB5">{{item.title}}</div>
                            <div class="indexListContent">
                                {{item.content}}
                            </div>
                            <div class="commentBox flexRow font14 marginT10">
                                <div class="praise flexItem backE5F2FF">
                                    <i class="icon iconfont icon-shang color0084ff"></i>赞同<span>3.1k</span>
                                </div>
                                <i class="icon iconfont icon-shang-copy color0084ff backE5F2FF"></i>
                                <a href="javascript:void(0)" class="font14 flexItem color8590a6 marginL24"><i class="icon iconfont icon-icon_comment color8590a6"></i><span class="marginLR4">666</span>条回答</a>
                            </div>
                        </div>
                    </el-tab-pane>
                    <el-tab-pane label="配置管理" name="second">配置管理</el-tab-pane>
                    <el-tab-pane label="角色管理" name="third">角色管理</el-tab-pane>
                    <el-tab-pane label="定时任务补偿" name="fourth">定时任务补偿</el-tab-pane>
                </el-tabs>
            </div>
            <div class="indexRight backfff rightContent felxCenterWrap">
                <div class="edit flexItem felxCenter">
                    <a href="javascript:void(0)" class="felxCenter">
                        <i class="icon iconfont icon-huida-copy felxCenter"></i>
                        <span>写回答</span>
                    </a>
                    <a href="javascript:void(0)" class="flexContent">
                        <i class="icon iconfont icon-xiewenzhang felxCenter"></i>
                        <span>写文章</span>
                    </a>
                    <a href="javascript:void(0)" class="flexContent">
                        <i class="icon iconfont icon-bangzhu-copy felxCenter"></i>
                        <span>写想法</span>
                    </a>
                </div>
                <div class="flexItem flexBetween caogao">
                    <span class="flexItem"><i class="icon iconfont icon-caogaoxiang"></i>
                    <span class="nbspLeft5">我的草稿</span>
                        </span>
                    <span class="headerImg backf6f6f6 color8590a6">2</span>
                </div>
            </div>
        </div>
    </div>
</template>
<script>
import axios from "axios";
export default {
  data: function() {
    return {
      input: "",
      activeName: "first",
      newsList: []
    };
  },
  created() {
    var that = this;
    var a=1;
    window.onscroll = function() {
      if (that.getScrollTop() == 0){
        console.log("顶部");
      }
    //   console.log(that.getScrollTop());
      if (that.getScrollTop() + that.getWindowHeight() == that.getScrollHeight()){
        console.log("已经到最底部了！!");
      }
    //   console.log(that.getScrollHeight()-that.getScrollTop());
      if(that.getScrollHeight()-that.getScrollTop()<2000){
          console.log(a);
          that.getList(a);
          a++;
          a=a;
      } 
    }; 
    this.getList(a);
  },
  methods: {
    getList: function(pageNum) {
      axios.get(
          "http://51eliao.com:9020/questionController/selectQuesionByParam.do?questionType=1&pageNum="+pageNum
        )
        .then(response => {
          console.log(response.data);
          this.newsList = response.data.data.list;
        })
        .catch(error => {
          console.log(error);
          alert("网络错误，不能访问");
        });
    },
    handleClick: function(tab, event) {
      // console.log(tab, event);
    },
    open3() {
      this.$prompt("请输入邮箱", "提示", {
        confirmButtonText: "确定",
        cancelButtonText: "取消",
        inputPattern: /[\w!#$%&'*+/=?^_`{|}~-]+(?:\.[\w!#$%&'*+/=?^_`{|}~-]+)*@(?:[\w](?:[\w-]*[\w])?\.)+[\w](?:[\w-]*[\w])?/,
        inputErrorMessage: "邮箱格式不正确"
      })
        .then(({ value }) => {
          this.$message({
            type: "success",
            message: "你的邮箱是: " + value
          });
        })
        .catch(() => {
          this.$message({
            type: "info",
            message: "取消输入"
          });
        });
    },
    getScrollTop: function() {
      var scrollTop = 0,
        bodyScrollTop = 0,
        documentScrollTop = 0;
      if (document.body) {
        bodyScrollTop = document.body.scrollTop;
      }
      if (document.documentElement) {
        documentScrollTop = document.documentElement.scrollTop;
      }
      scrollTop =
        bodyScrollTop - documentScrollTop > 0
          ? bodyScrollTop
          : documentScrollTop;
      return scrollTop;
    },
    //文档的总高度
    getScrollHeight: function() {
      var scrollHeight = 0,
        bodyScrollHeight = 0,
        documentScrollHeight = 0;
      if (document.body) {
        bodyScrollHeight = document.body.scrollHeight;
      }
      if (document.documentElement) {
        documentScrollHeight = document.documentElement.scrollHeight;
      }
      scrollHeight =
        bodyScrollHeight - documentScrollHeight > 0
          ? bodyScrollHeight
          : documentScrollHeight;
      return scrollHeight;
    },
    getWindowHeight: function() {
      var windowHeight = 0;
      if (document.compatMode == "CSS1Compat") {
        windowHeight = document.documentElement.clientHeight;
      } else {
        windowHeight = document.body.clientHeight;
      }
      return windowHeight;
    }
  }
};
</script>
<style scoped>
.topTitle {
  height: 60px;
  box-shadow: 0 1px 3px rgba(26, 26, 26, 0.1);
  background: #ffffff;
}
.logo {
  display: inline-block;
  width: 100px;
  height: 40px;
  background: url("./../../img/51logo.png") no-repeat center center;
  background-size: 100% 100%;
}
/* .navTitle{
        width: 400px;
    } */
.indexNav {
  width: 694px;
}
.indexRight {
  height: 100%;
  width: 296px;
}
.indexRightContent {
  width: 220px;
}
.navTitle > a {
  color: #8590a6;
  margin-left: 20px;
}
.navTitle > a:hover {
  color: #444;
}
.clinput {
  width: 300px;
}

.el-button--primary {
  margin-left: 10px;
}
.iconfont {
  font-size: 24px;
  /* color: #8590a6; */
}
.headerImg {
  width: 30px;
  height: 30px;
  border-radius: 4px;
  background: #8590a6;
}
.msg {
  position: relative;
  width: 30px;
  height: 30px;
  /* border: 1px solid red; */
}
.msg > span {
  position: absolute;
  display: inline-block;
  /* width: 30px;
        height: 20px; */
  width: 30px;
  height: 16px;
  /* padding: 2px; */
  text-align: center;
  line-height: 16px;
  color: #fff;
  background: #f1403c;
  border: 1px solid #fff;
  border-radius: 12px;
  font-size: 6px;
  top: -2px;
  right: -16px;
}
.indexTab {
  margin: 0px 10px;
  width: 100%;
}
.indexTab > .el-tabs__header {
  margin-bottom: 0px !important;
}
.indexListContent {
  height: 42px;
  overflow: hidden;
  text-overflow: ellipsis;
  display: -webkit-box;
  -webkit-box-orient: vertical;
  -webkit-line-clamp: 2;
}
.praise {
  /* border: 1px solid red; */
  border-radius: 2px;
  padding: 0 12px;
  line-height: 30px;
  color: #0084ff;
}
.praise > span {
  margin-left: 2px;
}
.icon-shang-copy {
  margin-left: 6px;
  /* padding-left: 8px; */
}
.rightContent {
  padding: 20px 0px;
}
.edit {
  width: 80%;
  display: flex;
  justify-content: space-between;
  padding-bottom: 10px;
  border-bottom: 1px solid #f0f2f7;
}
.edit > a {
  display: flex;
  flex-direction: column;
  justify-content: center;
  color: #8590a6;
}
.edit > a > span {
  margin-top: 10px;
  color: #444;
  font-size: 14px;
}
.caogao {
  width: 80%;
  padding-top: 10px;
}
.caogao span:nth-of-type(1) i {
  color: #8590a6;
}
.caogao span:nth-of-type(2) {
  background: #f6f6f6;
  text-align: center;
  line-height: 30px;
}
</style>

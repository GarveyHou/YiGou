<template>
  <header class="header">
    <!-- 头部的第一行 -->
    <div class="top">
      <div class="container">
        <div class="loginList">
          <p>易购网欢迎您！</p>
          <p v-if="!userName">
            <span>请</span>
            <router-link to="/login">登录</router-link>
            <router-link to="/register" class="register">免费注册</router-link>
          </p>
          <p v-else>
            <a>{{userName}}</a>
            <a class="register" @click="logout">退出登录</a>
          </p>
        </div>
        <div class="typeList">
          <router-link to="/center">我的订单</router-link>
          <router-link to="/shopcart">我的购物车</router-link>
          <a href="###">我的易购网</a>
          <a href="###">易购网会员</a>
          <a href="###">企业采购</a>
          <a href="###">关注易购网</a>
          <a href="###">合作招商</a>
          <a href="###">商家后台</a>
        </div>
      </div>
    </div>
    <!--头部第二行 搜索区域-->
    <div class="bottom">
      <h1 class="logoArea">
        <router-link class="logo" to="/home">
          <p id="title" style="{font-size:50px;margin-top:10px;margin-left:40px;}">易购网</p>
          <!-- <img src="@/assets/images/logo.png" alt=""> -->
        </router-link>
      </h1>
      <div class="searchArea">
        <form action="###" class="searchForm">
          <input type="text" id="autocomplete" class="input-error input-xxlarge" v-model="keyword" />
          <button class="sui-btn btn-xlarge btn-danger" type="button" @click='gosearch'>搜索</button>
        </form>
      </div>
    </div>
  </header>
</template>

<script>
import {mapState} from "vuex"
export default {
   data() {
    return {
      //收集表单数据---关键字的
      keyword: '',
    };
  },
  computed:{
    ...mapState("user",["userInfo"]),
    userName(){
      return this.userInfo.name
    }
  },
  mounted() {
    //监听自定义事件
    
    this.$bus.$on("changeKeyword", () => {
      //关键字置空
      this.keyword = "";
    });
  },
  methods:{
    gosearch(){
      if (this.$route.query) {
        console.log(this.$route.query);
        //本身点击搜索按钮，当年只是携带params，如果路径当中存在query参数，是需要把query参数页携带给search
        let location = {
          name: "search",
          params: { keyword: this.keyword || undefined },
        };
        location.query = this.$route.query;
        if (this.$route.path != "/home") {
          this.$router.replace(location);
        } else {
          this.$router.push(location);
        }
      }
    },
    async logout(){
      //退出登录需要做的事情
      //1:需要发请求，通知服务器退出登录【清除一些数据：token】
      //2:清除项目当中的数据【userInfo、token】
        try {
          //如果退出成功
          await this.$store.dispatch('user/userLogout');
          //回到首页
          this.$router.push('/home');
        } catch (error) {
          
        }
    }

  }
}
</script>

<style lang='less' scoped>
 .header {
        &>.top {
            background-color: #eaeaea;
            height: 30px;
            line-height: 30px;

            .container {
              
                width: 1200px;
                margin: 0 auto;
                overflow: hidden;

                .loginList {
                    float: left;
                    

                    p {
                        float: left;
                        margin-right: 10px;

                        .register {
                            border-left: 1px solid #b3aeae;
                            padding: 0 5px;
                            margin-left: 5px;
                        }
                    }
                }

                .typeList {
                    float: right;

                    a {
                        padding: 0 10px;

                        &+a {
                            border-left: 1px solid #b3aeae;
                        }
                    }

                }

            }
        }

        &>.bottom {
            width: 1200px;
            margin: 0 auto;
            overflow: hidden;

            .logoArea {
                float: left;
                height: 60px;
                width: 200PX;
                #title{
                  font-size: 40px;
                  // color: #eaeaea;
                   background: linear-gradient(to right, red, blue);
                    -webkit-background-clip: text;
                    color: transparent;
                }
                .logo {
                    img {
                        width: 175px;
                        margin: 25px 45px;
                    }
                }
            }

            .searchArea {
                float: right;
                margin-top: 35px;

                .searchForm {
                    overflow: hidden;

                    input {
                        box-sizing: border-box;
                        width: 490px;
                        height: 32px;
                        padding: 0px 4px;
                        border: 2px solid #ea4a36;
                        float: left;

                        &:focus {
                            outline: none;
                        }
                    }

                    button {
                        height: 32px;
                        width: 68px;
                        background-color: #ea4a36;
                        border: none;
                        color: #fff;
                        float: left;
                        cursor: pointer;

                        &:focus {
                            outline: none;
                        }
                    }
                }
            }
        }
    }
</style>
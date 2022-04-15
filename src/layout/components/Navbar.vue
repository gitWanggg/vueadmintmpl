<template>
  <div class="navbar">
    <hamburger :is-active="sidebar.opened" class="hamburger-container" @toggleClick="toggleSideBar" />

    <breadcrumb class="breadcrumb-container" />

    <div class="right-menu">
      <el-dropdown class="avatar-container" trigger="click">
        <div class="avatar-wrapper">
          <!-- <img :src="avatar+'?imageView2/1/w/80/h/80'" class="user-avatar">-->
          <i class="el-icon-caret-bottom" /> 
          <div>
              <el-avatar>{{ userName }}</el-avatar>
          </div>
        </div>
        <el-dropdown-menu slot="dropdown" class="user-dropdown">
          <!-- <div class="wdro-text">ID：{{ userID }}</div> -->
          <div class="wdro-text">用户：{{ userName }}</div>
          <!-- <el-divider></el-divider> -->
         <router-link to="/sso/editpwd">
            <el-dropdown-item>修改密码</el-dropdown-item>
          </router-link>
          <router-link to="/sso/switchapp">
            <el-dropdown-item>切换系统</el-dropdown-item>
          </router-link>
         
          <el-dropdown-item divided @click.native="logout">
            <span style="display: block">退出登录</span>
          </el-dropdown-item>
        </el-dropdown-menu>
      </el-dropdown>
    </div>
  </div>
</template>

<script>
import { mapGetters } from 'vuex'
import Breadcrumb from '@/components/Breadcrumb'
import Hamburger from '@/components/Hamburger'


export default {
  data(){
    return{
       "userID":"",
       "userName":""
    }
  },
  mounted() {              
     this.initUser();
  },
  components: {
    Breadcrumb,
    Hamburger
  },
  computed: {
    ...mapGetters([
      'sidebar',
      'avatar'
    ])
  },
  methods: {
    toggleSideBar() {
      this.$store.dispatch('app/toggleSideBar')
    },
    async logout() {
      await this.$store.dispatch('user/logout')
      // this.$router.push(`/login?redirect=${this.$route.fullPath}`)--原代码     
      // this.$router.replace(`/login?redirect=${this.$route.fullPath}`);
      location.href="/usercenter/index.html";
    },
   async initUser(){
      let uinfo={"ID":"0001","Name":"Admin"};
      // var uinfodata=await currentUser();     
      // var uinfo= uinfodata.data;    
       this.userID=uinfo.ID;
       this.userName=uinfo.Name;
      // this.$store.getters.umodel.info=uinfo; //初始化用户信息
    }
  }
}
</script>

<style lang="scss" scoped>

.wdro-text {
  padding: 8px 12px;
  font-size: 0.78rem;
}
.user-dropdown {
  position: fixed;
  top: 42px !important;
  white-space: nowrap;
}
.el-divider{margin: 8px 0;}
.navbar {
  height: 50px;
  overflow: hidden;
  position: relative;
  background: #fff;
  box-shadow: 0 1px 4px rgba(0,21,41,.08);
  z-index: 1001;

  .hamburger-container {
    line-height: 46px;
    height: 100%;
    float: left;
    cursor: pointer;
    transition: background .3s;
    -webkit-tap-highlight-color:transparent;

    &:hover {
      background: rgba(0, 0, 0, .025)
    }
  }
  .breadcrumb-container {
    float: left;
  }

  .right-menu {
    float: right;
    height: 100%;

    &:focus {
      outline: none;
    }

    .right-menu-item {
      display: inline-block;
      padding: 0 8px;
      height: 100%;
      font-size: 18px;
      color: #5a5e66;
      vertical-align: text-bottom;

      &.hover-effect {
        cursor: pointer;
        transition: background .3s;

        &:hover {
          background: rgba(0, 0, 0, .025)
        }
      }
    }

    .avatar-container {
      margin-right: 30px;

      .avatar-wrapper {
        margin-top: 4px;
        position: relative;

        .user-avatar {
          cursor: pointer;
          width: 36px;
          height: 36px;
          border-radius: 10px;
        }

        .el-icon-caret-bottom {
          cursor: pointer;
          position: absolute;
          color: #888;
          right: -20px;
          top: 14px;
          font-size: 12px;
        }
      }
    }
  }
}
</style>

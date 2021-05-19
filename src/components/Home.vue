<template>
    <!--引入container布局-->
    <el-container class="home-container">
        <!--头部布局-->
        <el-header>
            <div>
                <img src="../assets/touxiang.jpg">
                <span>个人运动平台</span>
            </div>
            <el-button type="info" @click="logout">安全退出</el-button>
        </el-header>
        <!--主体-->
        <el-container>
            <!--侧边栏-->
            <el-aside :width="isCollapse ?'64px':'200px'">
                <div class="toggle-button" @click="toggleCollapase">|||</div>
                <el-menu background-color="#545c64" text-color="#fff" active-text-color="#ffd04b" unique-opened
                         :collapse="isCollapse"
                         :collapse-transition="false"
                         :router="true"
                         :default-active="activePath">
                    <!--一级菜单-->
                    <el-submenu :index="item.id+''.toString()" v-for="item in menuList" :key="item.id">
                        <template slot="title">
                            <i :class="iconsObject[item.id]"></i>
                            <span>{{item.title}}</span>
                        </template>
                        <!--二级菜单-->
                        <el-menu-item :index="it.path.toString()" v-for="it in item.slist" :key="it.id" @click="saveNavState(it.path+'')">
                            <template slot="title">
                                <i :class="iconsObject[it.id]"></i>
                                <span>{{it.title}}</span>
                            </template>
                        </el-menu-item>
                    </el-submenu>
                </el-menu>
            </el-aside>
            <!--主体内容-->
            <el-main>
                <!--路由占位符-->
                <router-view></router-view>
            </el-main>
        </el-container>
    </el-container>
</template>

<script>
    export default {
        data() {
            return {
                //菜单列表
                menuList: [],
                iconsObject: {
                    '100': 'iconfont iconguanliyuan',
                    '200': 'iconfont iconsport',
                    '101': 'iconfont icondenglu',
                    '102': 'iconfont iconmima',
                    '103': 'iconfont iconsport',
                    '104': 'iconfont iconshangpin',
                    '201': 'iconfont iconshu',
                    '202': 'iconfont iconkaluli',
                    '203': 'iconfont iconshiwu',
                    '204': 'iconfont icondenglu',
                },
                isCollapse: false,
                // 默认路径
                activePath:'/welcome',
            }
        },
        //onload事件：页面一加载就执行
        created() {
            //查询muneList
            this.getMuneList();
            this.activePath = window.sessionStorage.getItem('activePath');// 取出session里的访问路径
        },
        methods: {
            logout() {
                window.sessionStorage.clear();
                this.$router.push("/login")
            },
            //获取导航栏方法
            async getMuneList() {
                const {data: res} = await this.$http.get("menus");
                // console.log(res);
                if (res.status != 200) return this.$message.error("操作失败！！！");//访问失败的错误信息
                this.menuList = res.data;
            },
            // 切换菜单折叠与展开
            toggleCollapase() {
                this.isCollapse = !this.isCollapse;
            },
            // 保存二级菜单的路径
            saveNavState(activePath){
                window.sessionStorage.setItem('activePath',activePath);// 存放点击的二级菜单
                this.activePath = activePath;// 给点击的菜单添加高亮
            },
        }
    }
</script>

<style lang="less" scoped>
    /*布局器样式*/
    .home-container {
        height: 100%;
    }

    /*头样式*/
    .el-header {
        background-color: #373D41;
        display: flex;
        justify-content: space-between;// 左右贴边
        padding-left: 0%;// 左边界
        align-items: center;// 水平
        color: #f5f2f0;
        font-size: 20px;

        > div { /*!/左侧div加布局*/
            display: flex;
            align-items: center;

            span {
                margin-left: 15px;
            }
        }
    }

    /*侧边栏*/
    .el-aside {
        background-color: #333744;

        .el-menu {
            border-right: none;
            width: 100%;
        }
    }

    /*主体样式*/
    .el-main {
        background-color: #EAEDF1;


    }

    //|||按钮样式
    .toggle-button {
        background-color: #4A5064;
        font-size: 10px;
        line-height: 24px;
        color: #fff;
        text-align: center;
        letter-spacing: 0.2em;
        cursor: pointer; // 显示鼠标指针为：小手
        width: 100%;
    }

    img {
        width: 60px;
        height: 60px;
    }

</style>
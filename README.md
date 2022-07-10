# MyBBS
BBS论坛开发
## 1.axios 的三种方式：
```html
               login(){
                    var userNo = this.userNo;
                    var pwd = this.password;
                    var remMe = this.remMe;
                    //方式一：
                    //http://localhost:5000/login/1126146235/111
                    axios.get("http://localhost:5000/login/"+userNo+"/"+pwd).then(
                        function(res){
                            console.log(res);
                        }
                    )
                    //方式二：
                    //http://localhost:5000/login?userNo=1126146235&password=111
                    axios.get("http://localhost:5000/login/",{params:{userNo,password:pwd}}).then(
                        function(res){
                            console.log(res);
                        }
                    )

                    //方式三（等价方式一）：
                    //http://localhost:5000/login/1126146235/111
                    axios.get(`http://localhost:5000/login/${userNo}/${pwd}/`,{params:{userNo,password:pwd}}).then(
                        function(res){
                            console.log(res);
                        }
                    )
                 }
```
## 2.状态保持（说白了就是session的使用） 下面为session 的原理
![状态保持](https://github.com/RanGuMo/MyBBS/blob/master/my_bbs_ui/assets/images/1657034294668.jpg)

## 3.ajax 发起请求
```html
 // 下面这种方式Ajax ，引入 <script src="http://libs.baidu.com/jquery/2.0.0/jquery.min.js"></script>
                    // 后端不用加上 ApiController 特性
                    // 数据格式 是Form 表格格式提交的 
                    // string userName,string userNo,string password 可以接收到
                    // $.post("http://localhost:5000/login",
                    //     {
                    //         userName: this.userName,
                    //         userNo: this.userNo,
                    //         password: this.password
                    //     }, function (res) {
                    //         console.log(res);
                    //     });  
```


客户端  点击 登录------》服务器端进行 登录校验 生成一个sessionid 并响应 回给 客户端 
下一次请求是 客户端（浏览器）总是会携带着seesionid 来请求 我们就可以通过获取 sessionid 来获取用户的信息了

## 3.令牌(就是把生成seesionid  改成 我们自己生成一个唯一标识)
![令牌](https://github.com/RanGuMo/MyBBS/blob/master/my_bbs_ui/assets/images/1657034750561.jpg)

## 4.登录页面（login.html）前端保持用户状态
```html
 new Vue({
            el:"#loginPage",
            data:{
                userNo:'',
                password:'',
                remMe:true
            },
            mounted() {//页面挂载时 
                //将 存储在localStorage 中的userInfo里面的值取出
                var userInfo = JSON.parse(localStorage["userInfo"]);//解析json
                this.userNo = userInfo.userNo;
                this.password = userInfo.password;
                //由于取出来的值是 字符串 类型，需要进行转换一下
                this.remMe = localStorage["isRem"] == "true"? true:false
            },
            methods: {
                login(){
                  var userInfo ={
                        userNo:'',
                        password:''
                    };
                    var userNo = this.userNo;
                    var pwd = this.password;
                    var remMe = this.remMe;
                    //方式三（等价方式一）：
                    //http://localhost:5000/login/1126146235/111
                    axios.get(`http://localhost:5000/login/${userNo}/${pwd}/`,{params:{userNo,password:pwd}}).then(
                        function(res){
                            //console.log(res);
                            //发送axions请求时 将后端的值进行localStorage 存储，页面挂载时取出
                            localStorage["token"] = res.data.token;
                            localStorage["isRem"] = remMe;
                            if(remMe){//勾选`记住我` 需要处理的内容
                                userInfo.userNo = userNo;
                                userInfo.password  =res.data.password;//后端经过MD5加密的值
                                localStorage["userInfo"] = JSON.stringify(userInfo); //JSON.stringify() 方法用于将 JavaScript 值转换为 JSON 字符串。
                            }else{
                                localStorage.removeItem("userInfo");//没有勾选`记住我`，就移除localStorage中的userInfo
                            }
                            // 登录成功后，进行页面跳转
                            location.href="postList.html"

                        }
                    )
                },
                back(){
                    history.go(-1);//返回上一级
                }
            },
        })
```

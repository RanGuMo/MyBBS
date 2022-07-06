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


客户端  点击 登录------》服务器端进行 登录校验 生成一个sessionid 并响应 回给 客户端 
下一次请求是 客户端（浏览器）总是会携带着seesionid 来请求 我们就可以通过获取 sessionid 来获取用户的信息了

## 3.令牌(就是把生成seesionid  改成 我们自己生成一个唯一标识)
![令牌](https://github.com/RanGuMo/MyBBS/blob/master/my_bbs_ui/assets/images/1657034750561.jpg)

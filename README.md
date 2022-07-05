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

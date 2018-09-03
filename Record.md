# 数组遍历，在引入hash
let need = ['email','password','password_confirmation']
       need.forEach((name)=>{
          let value = $('#signUpForm').find(`[name=${name}]`).val() //只是input输入的值
          hash[name] = value 
       })

# sign-up  注册
  if(path === '/sign-up' && methed === 'POST'){  //只要路径相同且是POST就成功，得到一串html字符串
    let string = fs.readFileSync('./sign-up.html', 'utf8')
    response.statusCode = 200
    response.setHeader('Content-Type', 'text/javascript;charset=utf-8')
    response.write(string)
    response.end()

# 请求体     前端所有东西变成字符串，后端解析前端传的字符串为hash
function readBody(request){
  return new Promise((resolve, reject)=>{
    let body = []
    request.on('data', (chunk) => {
      body.push(chunk);
    }).on('end', () => {
      body = Buffer.concat(body).toString();






      
      resolve(body)
    })
  })
}

readBody(request).then((body)=>{
      let strings = body.split('&')     // 分割,['email=1', 'password=2', 'password_confirmation=3']
      let hash = {}
      strings.forEach((string)=>{
        // string == 'email=1'
        let parts = string.split('=')   //再分割,['email', '1']
        let key = parts[0]
        let value = parts[1]
        hash[key] = decodeURIComponent(value) // hash['email'] = '1'
      })

# ES6语法   变量赋值
    let {email, password, password_confirmation} = hash

# 前后端撕逼协议
    后端用JSON对象语法，`{"errors": {"email": "invalid"}}`,且Header里面application/json
    
    前端会使用获取JSON中email的invalid，来判断
        (request)=>{                                            //request请求
          let {errors} = request.responseJSON                   //获得请求的JSON值，浏览器自动JSON.parse转换
          if(errors.email && errors.email === 'invalid'){       //errors.email对象中是'invalid'判断是否错误
            $form.find('[name="email"]').siblings('.error')     //验证提醒
              .text('邮箱格式错误')
          }

# 前端页面注册验证 
    $form.find('.error').each((index, span)=>{
        $(span).text('')
    .  }) 
    if(hash['email'] === ''){
        $form.find('[name="email"]').siblings('.error')
          .text('填邮箱呀同学')
        return
      }
      if(hash['password'] === ''){
        $form.find('[name="password"]').siblings('.error')
          .text('填密码呀同学')
        return
      }
      if(hash['password_confirmation'] === ''){
        $form.find('[name="password_confirmation"]').siblings('.error')
          .text('确认密码呀同学')
        return
      }
      if(hash['password'] !== hash['password_confirmation']){
        $form.find('[name="password_confirmation"]').siblings('.error')
          .text('密码不匹配')
        return
      }
# 后端条件判断,返回响应
    if(email.indexOf('@') === -1){//
            response.statusCode = 400
            response.setHeader('Content-Type', `application/json`;charset=utf-8')
            response.write(`{
            "errors": {
                "email": "invalid"
            }
            }`)

    }else if(password !== password_confirmation){
            response.statusCode = 400
            response.write('password not match')        


# 后端把数据存入服务器
    
    else{
        var users = fs.readFileSync('./db/users', 'utf8')   //第一个是文件，第二个是data
        try{
          users = JSON.parse(users)                         //users是个数组文件 []
        }catch(exception){
          users = []
        }

// 判断email是否存在

        let inUse = false       
        for(let i=0; i<users.length; i++){    //遍历users，
          let user = users[i]
          if(user.email === email){           //email存在，则赋值inUse = true，
            inUse = true
            break;
          }
        }
        if(inUse){                            //赋值inUse判断重复email，错误
          response.statusCode = 400
          response.write('email in use')
        }else{                                //判断没有email，就push存入数组
          users.push({email: email, password: password})
          var usersString = JSON.stringify(users)
          fs.writeFileSync('./db/users', usersString)
          response.statusCode = 200
        }
    }

# sign_in 登录
    
    $.post('/sign_in', hash)  
            .then((response)=>{
            window.location.href = '/'      //登录成功条首页
            }, (request)=>{
            alert('邮箱与密码不匹配')
            })

# 后端验证
      let found
      for(let i=0;i<users.length; i++){     //遍历字符串，验证账户密码是否存在usres
        if(users[i].email === email && users[i].password === password){
          found = true
          break
        }
      }
      if(found){
        response.setHeader('Set-Cookie', `sign_in_email=${email}`)    //cookie的头
        response.statusCode = 200
      }else{
        response.statusCode = 401
      }
      response.end()
    })

# Cookie，登录Set-Cookie

    Set-Cookie:<cookie-name> = <cookie-value
    response.setHeader('Set-Cookie', `sign_in_email=${email}`) 

    APPlocation里面可以改Cookie
    登录会setCookie

# 后端获取Cookie

    let string = fs.readFileSync('./index.html', 'utf8')
    let cookies =  request.headers.cookie.split('; ') // ['email=1@', 'a=1', 'b=2']
    let hash = {}
    for(let i =0;i<cookies.length; i++){    //遍历字符串数组传入hash
      let parts = cookies[i].split('=')
      let key = parts[0]
      let value = parts[1]
      hash[key] = value 
    }
    let email = hash.sign_in_email          //获取email对象值，并遍历其中cookies
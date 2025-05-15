这是一个个人博客模板仓库，用于快速搭建个人博客网站。  
用户只需要编写 Markdown 格式的博客内容，提交后即可自动发布到个人博客网站上。  
--- 
# 准备条件
- github 账号

# 创建个人博客网站仓库
在 github 上创建一个新的代码仓库, 此仓库用于保存网页代码等，来呈现你的个人博客。  
创建完成后你可以使用 https://xxx.github.io 来访问个人博客网站。
1. 点击 your repositories  
   ![image](https://github.com/tangchenyang/picx-images-hosting/raw/master/image.361smiekru.webp)  

2. 点击 New   
   ![image](https://github.com/tangchenyang/picx-images-hosting/raw/master/image.83a9gcu9y8.webp)  

3. 填写 Repository name 后点击 `Create repository`  
   **注意：** 仓库名格式必须是 `<your username>.github.io`, 例如 `tangchenyang.github.io`  
![image](https://github.com/tangchenyang/picx-images-hosting/raw/master/image.7i0lu24fp5.webp)  

4. 点击 `creating a new file` 创建一个初始页面
   ![image](https://github.com/tangchenyang/picx-images-hosting/raw/master/image.7i0lu28blh.webp)
   初始文件名为 `index.html`, 并编辑以下内容：   
   ```html
   <!DOCTYPE html>
   <html lang="en">
   <body>
       <h1>Hello, this is my personal blog</h1>
   </body>
   </html>
   ```
   编辑完成后点击 `Commit changes` 

   ![image](https://github.com/tangchenyang/picx-images-hosting/raw/master/image.51edf57flw.webp)
   ![image](https://github.com/tangchenyang/picx-images-hosting/raw/master/image.39lek8pk30.webp)

5. 等待 Github Action 部署完成  
   页面创建完成后，Github Action 将自动触发部署任务，部署完成后，你的个人网站将可以访问。  
   ![image](https://github.com/tangchenyang/picx-images-hosting/raw/master/image.4g4psuhvjp.webp)

6. 访问你的个人网站  
   在浏览器输入 `<your username>.github.io` 即可看到你的个人博客网站初始内容。  
   ![image](https://github.com/tangchenyang/picx-images-hosting/raw/master/image.73u637c7sz.webp)

# 创建博客文章仓库
此仓库用于维护 Markdown 格式的博客文章，提交后将自动发布到个人博客网站。
## 申请 GitHub PAT (Personal Access Token)
1. 点击 `Settings`
   ![image](https://github.com/tangchenyang/picx-images-hosting/raw/master/image.5q7n0nijk6.webp)  

2. 点击最底部的 Developers settings  
   ![image](https://github.com/tangchenyang/picx-images-hosting/raw/master/image.5tr8yddq50.webp)  

3. 依次点击下图按钮，进入New Personal Access Token 界面
   ![image](https://github.com/tangchenyang/picx-images-hosting/raw/master/image.4qrjnhk8jj.webp)

4. 设置有效期和权限，生成 PAT  
   `Note` 见名知意即可；`Expiration` 根据情况选择，如`no expiration`；Scope 选择 `repo` 即可。  
   ![image](https://github.com/tangchenyang/picx-images-hosting/raw/master/image.3k88ew05g4.webp)  
   勾选完成后，点击底部的 `Generate token`  
   ![image](https://github.com/tangchenyang/picx-images-hosting/raw/master/image.6wqy99km34.webp)
   生成 PAT 后，复制并妥善保管，离开此页面后将不再可见。


## 创建博客文章仓库
1. Github 创建博客仓库 `blog`
   ![image](https://github.com/tangchenyang/picx-images-hosting/raw/master/image.5q7n0n2oqb.webp)

2. 添加 Repository secrets  
   依次点击 `Settings` -> `Secrets and variables` -> `Actions` -> `New repository secret`  
   ![image](https://github.com/tangchenyang/picx-images-hosting/raw/master/image.83a9hvmezk.webp)
   `Name` 填写 `HEXO_DEPLOY_PAT`; `Secret` 将前面生成的Personal Access Token 填写至此；点击 `Add secret` 即可。
   ![image](https://github.com/tangchenyang/picx-images-hosting/raw/master/image.969ysrl8qa.webp)

## 关联模板仓库代码
1. 克隆模板博客仓库到本地，并重命名为 `blog`
   ```shell
   git clone https://github.com/tangchenyang/blog_template_repo.git blog
   ```

2. 修改提交信息
   进入 `blog` 目录下，编辑 .github/workflows/deploy.ml, 将 `GIT_HTTPS_URL`，`GIT_USER_NAME` 和 `GIT_USER_EMAIL` 按自己的信息替换即可，如：  
   ![image](https://github.com/tangchenyang/picx-images-hosting/raw/master/image.8z6qxdty3c.webp)

3. 关联 blog 仓库, 并推送到远端  
   ```shell
   cd blog
   rm -rf .git
   echo "我的个人博客" > README.md
   git init
   git add * && git add .*
   git commit -m "first commit"
   git branch -M master
   git remote add origin git@github.com:tangchenyang/blog.git
   git push -u origin master
   ```
   
4. 检查博客文章仓库 `blob` 的GHA (GitHub Action)  
   代码推送到远端 `blog` 仓库后，由于包含了 workflow, 因此 GHA 将自动触发 CI/CD Pipeline;   
   `blog/blogs` 目录下的所有markdown 文件将自动转换为静态网页代码，并自动提交到你的个人博客网站仓库 `<your username>.github.io`  
   ![image](https://github.com/tangchenyang/picx-images-hosting/raw/master/image.7w71mhaaey.webp)

5. 检查个人博客网站仓库 `<your username>.github.io` 的 GHA  
   由 `blog` GHA 自动提交的静态网页代码将自动触发 `<your username>.github.io` 的GHA，自动将静态网页部署到你的个人博客网站上。    
   ![image](https://github.com/tangchenyang/picx-images-hosting/raw/master/image.5tr8yffzsz.webp)  
   

# 访问个人博客网站  
`<your username>.github.io` 的 GHA 执行完成后，在浏览器输入 `<your username>.github.io` 即可看到你的个人博客网站初始内容。   
如我的个人博客： https://tangchenyang.github.io/   
![image](https://github.com/tangchenyang/picx-images-hosting/raw/master/image.map4yq6d.webp)  

# 开启评论和网站统计功能  
TODO： 敬请期待  

# BUILD

```bash
# 0. install nvm
# refrence (https://hackernoon.com/how-to-install-node-js-on-ubuntu-16-04-18-04-using-nvm-node-version-manager-668a7166b854)
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.34.0/install.sh | bash
bash install_nvm.sh
source ~/.profile
nvm --version
nvm ls-remote
nvm install 10.15
nvm use 10.15
node -v
npm -v
# 0. install yarn
npm i -g npm --registry https://registry.npm.taobao.org
npm i -g yarn --registry https://registry.npm.taobao.org
# set yarn use taobao proxy
yarn config set registry https://registry.npm.taobao.org
# install pkg
yarn install
# 安装包
npm install hexo-generator-searchdb --save
npm install -g hexo-cli
npm install -g hexo@3.9.0

# local start
hexo g
## 启动服务
hexo s

## 清理
hexo clean
```

# write 
```bash
# 创建博客（不能带空格，不能加特殊字符）
make k8s TITLE='解析k8s-yaml成client-go中的data-structs'
#make ps TITLE='Registry配置keycloak作为认证服务'
#make self TITLE='Gland使用技巧'
#make golang TITLE='Go-Modules版本控制和依赖管理'

# 创建草稿
hexo new draft hexo常用命令备忘录
# 本机预览草稿
hexo S --draft
# 将草稿发布为正式文章
hexo P hexo常用命令备忘录
```
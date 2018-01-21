# dev-images-docker
## ci目录下是在持续集成环境中的ubuntu开发环境
ci的docker镜像基于ubuntu16.05，因为需要在该系统运行前端自动化测试，所以内置Chromium， 因此构建时需要先在同级目录下载好Chromium，文件夹命名为Chrome-linux。
下载链接： https://www.chromium.org/getting-involved/download-chromium
## ubuntu-dev
该镜像用来本地开发，搭配了8.9.1的node版本，更换node源，安装cnpm，及一些常用开发工具


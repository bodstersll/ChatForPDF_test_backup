# ChatForPDF_test_backup
本项目已在 Python 3.9，CUDA 11.8 环境下完成测试。
vue前端需要node18环境

## 项目介绍

#### 本项目基于ChatGLM，具备多种语言能力，包括自我认知、提纲写作、信息抽取、文案写作。
#### 基于本地数据库实现问答功能，利用提示词进行微调效果提升
#### 单文档问答（支持PDF、Word）包括五个步骤：加载分割文本、并将其与用户问题进行匹配
#### 匹配的两种方式为字符搜索或语义搜索，后者更为常用
#### WebUI包括LLM对话、知识库测试和模型配置
#### 本地知识库问答可通过微调embedding模型提高本地知识库问答效果
#### 支持图片中的文字OCR识别


## Docker 部署

    sudo apt-get update
    sudo apt-get install -y nvidia-container-toolkit-base
    sudo systemctl daemon-reload 
    sudo systemctl restart docker

安装完成后，可以使用以下命令编译镜像和启动容器：

    docker build -f Dockerfile-cuda -t chatglm-cuda:latest .
    docker run --gpus all -d --name chatglm -p 7860:7860  chatglm-cuda:latest

若要使用离线模型，请配置好模型路径，然后此repo挂载到Container:

    docker run --gpus all -d --name chatglm -p 7860:7860 -v ~/github/langchain-ChatGLM:/chatGLM  chatglm-cuda:latest

## Windows 部署
    # 拉取仓库
    git clone https://github.com/imClumsyPanda/langchain-ChatGLM.git

    # 进入目录
    cd langchain-ChatGLM

    项目中 pdf 加载由先前的 detectron2 替换为使用 paddleocr，如果之前有安装过 detectron2 需要先完成卸载避免引发 tools 冲突
    pip uninstall detectron2

    # 安装依赖
    pip install -r requirements.txt

    # 验证paddleocr是否成功，首次运行会下载约18M模型到~/.paddleocr
    python loader/image_loader.py

## Web UI 或命令行交互

    执行 cli_demo.py 脚本体验命令行交互：
    python cli_demo.py

    执行 webui.py 脚本体验 Web 交互
    python webui.py

    执行 api.py 利用 fastapi 部署 API
    python api.py

    成功部署 API 后，执行以下脚本体验基于 VUE 的前端页面
    cd views 
    pnpm i
    npm run dev
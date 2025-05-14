[![MseeP.ai Security Assessment Badge](https://mseep.net/pr/chenningling-redbook-search-comment-mcp2-0-badge.png)](https://mseep.ai/app/chenningling-redbook-search-comment-mcp2-0)

# 小红书自动搜索评论工具（MCP Server 2.0）

<div align="right">

[English](README_EN.md) | 中文

</div>

> 本项目基于 [JonaFly/RednoteMCP](https://github.com/JonaFly/RednoteMCP.git) 并结合多次实战经验，进行全面优化和功能扩展（by windsurf）。在此向原作者的贡献表示由衷的感谢！

这是一款基于 Playwright 开发的小红书自动搜索和评论工具，作为 MCP Server，可通过特定配置接入 MCP Client（如Claude for Desktop），帮助用户自动完成登录小红书、搜索关键词、获取笔记内容及发布AI生成评论等操作。

## 主要特点与优势

- **深度集成AI能力**：利用MCP客户端（如Claude）的大模型能力，生成更自然、更相关的评论内容
- **模块化设计**：将功能分为笔记分析、评论生成和评论发布三个独立模块，提高代码可维护性
- **强大的内容获取能力**：集成多种获取笔记内容的方法，确保能完整获取各类笔记的标题、作者和正文内容
- **持久化登录**：使用持久化浏览器上下文，首次登录后无需重复登录
- **两步式评论流程**：先获取笔记分析结果，然后由MCP客户端生成并发布评论

## 2.0版本主要优化

- **内容获取增强**：重构了笔记内容获取模块，增加页面加载等待时间和滚动操作，实现四种不同的内容获取方法
- **AI评论生成**：重构评论功能，将笔记分析结果返回给MCP客户端，由客户端的AI能力生成更自然、更相关的评论
- **功能模块化**：将功能分为笔记分析、评论生成和评论发布三个独立模块，提高代码可维护性
- **搜索结果优化**：解决了搜索笔记时标题不显示的问题，提供更完整的搜索结果
- **错误处理增强**：添加更详细的错误处理和调试信息输出

## 一、核心功能

### 1. 用户认证与登录
- **持久化登录**：支持手动扫码登录，首次登录后保存状态，后续使用无需重复扫码
- **登录状态管理**：自动检测登录状态，并在需要时提示用户登录

### 2. 内容发现与获取
- **智能关键词搜索**：支持多关键词搜索，可指定返回结果数量，并提供完整的笔记信息
- **多维度内容获取**：集成四种不同的获取方法，确保能准确获取笔记的标题、作者、发布时间和正文内容
- **评论数据获取**：支持获取笔记的评论内容，包括评论者、评论文本和时间信息

### 3. 内容分析与生成
- **笔记内容分析**：自动分析笔记内容，提取关键信息并识别笔记所属领域
- **AI评论生成**：利用MCP客户端（如Claude）的AI能力，基于笔记内容生成自然、相关的评论
- **多类型评论支持**：支持四种不同类型的评论生成：
  - **引流型**：引导用户关注或私聊
  - **点赞型**：简单互动获取好感
  - **咨询型**：以问题形式增加互动
  - **专业型**：展示专业知识建立权威

### 4. 数据返回与反馈
- **结构化数据返回**：将笔记分析结果以JSON格式返回给MCP客户端，便于AI生成评论
- **评论发布反馈**：提供评论发布结果的实时反馈

## 二、安装步骤

1. **Python 环境准备**：确保系统已安装 Python 3.8 或更高版本。若未安装，可从 Python 官方网站下载并安装。

2. **项目获取**：将本项目克隆或下载到本地。

3. **创建虚拟环境**：在项目目录下创建并激活虚拟环境（推荐）：
   ```bash
   # 创建虚拟环境
   python3 -m venv venv
   
   # 激活虚拟环境
   # Windows
   venv\Scripts\activate
   # macOS/Linux
   source venv/bin/activate
   ```

4. **安装依赖**：在激活的虚拟环境中安装所需依赖：
   ```bash
   pip install -r requirements.txt
   pip install fastmcp
   ```

5. **安装浏览器**：安装Playwright所需的浏览器：
   ```bash
   playwright install
   ```

## 三、MCP Server 配置

在 MCP Client（如Claude for Desktop）的配置文件中添加以下内容，将本工具配置为 MCP Server：

### Mac 配置示例

```json
{
    "mcpServers": {
        "xiaohongshu MCP": {
            "command": "/绝对路径/到/venv/bin/python3",
            "args": [
                "/绝对路径/到/xiaohongshu_mcp.py",
                "--stdio"
            ]
        }
    }
}
```

### Windows 配置示例

```json
{
    "mcpServers": {
        "xiaohongshu MCP": {
            "command": "C:\\Users\\username\\Desktop\\MCP\\Redbook-Search-Comment-MCP2.0\\venv\\Scripts\\python.exe",
            "args": [
                "C:\\Users\\username\\Desktop\\MCP\\Redbook-Search-Comment-MCP2.0\\xiaohongshu_mcp.py",
                "--stdio"
            ]
        }
    }
}
```

> **重要提示**：
> - 请使用虚拟环境中Python解释器的**完整绝对路径**
> - Mac示例：`/Users/username/Desktop/RedBook-Search-Comment-MCP/venv/bin/python3`
> - Windows示例：`C:\Users\username\Desktop\MCP\Redbook-Search-Comment-MCP2.0\venv\Scripts\python.exe`
> - 同样，xiaohongshu_mcp.py也需要使用**完整绝对路径**
> - Windows路径中的反斜杠在JSON中需要双重转义（使用 `\`）

### Python 命令区分（python 与 python3）

不同系统环境中，Python 命令可能有所不同，这取决于您的系统配置。以下是如何确定您应该使用哪个命令：

1. **确定您的 Python 命令**：
   - 在终端中运行：`python --version` 和 `python3 --version`
   - 查看哪个命令返回 Python 3.x 版本（本项目需要 Python 3.8+）

2. **在虚拟环境中确认**：
   - 激活虚拟环境后，运行 `which python` 或 `where python`（Windows）
   - 这将显示 Python 解释器的完整路径

3. **配置中使用正确的命令**：
   - Mac：通常为 `python3` 或虚拟环境中的 `python`
   - Windows：通常为 `python` 或 `python.exe`

在配置文件中，始终使用虚拟环境中 Python 解释器的**完整绝对路径**，而不是命令名称。

## 四、使用方法

### （一）启动服务器

1. **直接运行**：在项目目录下，激活虚拟环境后执行：
   ```bash
   python3 xiaohongshu_mcp.py
   ```

2. **通过 MCP Client 启动**：配置好MCP Client后，按照客户端的操作流程进行启动和连接。

### （二）主要功能操作

在MCP Client（如Claude for Desktop）中连接到服务器后，可以使用以下功能：

### 1. 登录小红书

**工具函数**：
```
mcp0_login()
```

**在MCP客户端中的使用方式**：
直接发送以下文本：
```
帮我登录小红书账号
```
或：
```
请登录小红书
```

**功能说明**：首次使用时会打开浏览器窗口，等待用户手动扫码登录。登录成功后，工具会保存登录状态。

### 2. 搜索笔记

**工具函数**：
```
mcp0_search_notes(keywords="关键词", limit=5)
```

**在MCP客户端中的使用方式**：
发送包含关键词的搜索请求：
```
帮我搜索小红书笔记，关键词为：美食
```
指定返回数量：
```
帮我搜索小红书笔记，关键词为旅游，返回10条结果
```

**功能说明**：根据关键词搜索小红书笔记，并返回指定数量的结果。默认返回5条结果。

### 3. 获取笔记内容

**工具函数**：
```
mcp0_get_note_content(url="笔记URL")
```

**在MCP客户端中的使用方式**：
发送包含笔记URL的请求：
```
帮我获取这个笔记的内容：https://www.xiaohongshu.com/search_result/xxxx
```
或：
```
请查看这个小红书笔记的内容：https://www.xiaohongshu.com/search_result/xxxx
```

**功能说明**：获取指定笔记URL的详细内容，包括标题、作者、发布时间和正文内容。

### 4. 获取笔记评论

**工具函数**：
```
mcp0_get_note_comments(url="笔记URL")
```

**在MCP客户端中的使用方式**：
发送包含笔记URL的评论请求：
```
帮我获取这个笔记的评论：https://www.xiaohongshu.com/search_result/xxxx
```
或：
```
请查看这个小红书笔记的评论区：https://www.xiaohongshu.com/search_result/xxxx
```

**功能说明**：获取指定笔记URL的评论信息，包括评论者、评论内容和评论时间。

### 5. 发布智能评论

**工具函数**：
```
mcp0_post_smart_comment(url="笔记URL", comment_type="评论类型")
```

**在MCP客户端中的使用方式**：
发送包含笔记URL和评论类型的请求：
```
帮我为这个笔记写一条[类型]评论：https://www.xiaohongshu.com/explore/xxxx
```

**功能说明**：获取笔记分析结果，并返回给MCP客户端，由客户端生成评论并调用post_comment发布。

### 6. 发布评论

**工具函数**：
```
mcp0_post_comment(url="笔记URL", comment="评论内容")
```

**在MCP客户端中的使用方式**：
发送包含笔记URL和评论内容的请求：
```
帮我发布这条评论到笔记：https://www.xiaohongshu.com/explore/xxxx
评论内容：[评论内容]
```

**功能说明**：将指定的评论内容发布到笔记页面。

## 四、使用指南

### 0. 工作原理

本工具采用两步式流程实现智能评论功能：

1. **笔记分析**：调用`post_smart_comment`工具获取笔记信息（标题、作者、内容等）

2. **评论生成与发布**：
   - MCP客户端(如Claude)基于笔记分析结果生成评论
   - 调用`post_comment`工具发布评论

这种设计充分利用了MCP客户端的AI能力，实现了更自然、相关的评论生成。

### 1. 在MCP客户端中的使用方式

#### 基本操作

| 功能 | 示例命令 |
|---------|----------|
| **搜索笔记** | `帮我搜索关于[关键词]的小红书笔记` |
| **获取笔记内容** | `帮我查看这篇小红书笔记的内容：https://www.xiaohongshu.com/explore/xxxx` |
| **分析笔记** | `帮我分析这篇小红书笔记：https://www.xiaohongshu.com/explore/xxxx` |
| **获取评论** | `帮我查看这篇笔记的评论：https://www.xiaohongshu.com/explore/xxxx` |
| **生成评论** | `帮我为这篇小红书笔记写一条[类型]评论：https://www.xiaohongshu.com/explore/xxxx` |

#### 评论类型选项

| 类型 | 描述 | 适用场景 |
|---------|------|----------|
| **引流** | 引导用户关注或私聊 | 增加粉丝或私信互动 |
| **点赞** | 简单互动获取好感 | 增加曝光和互动率 |
| **咨询** | 以问题形式增加互动 | 引发博主回复，增加互动深度 |
| **专业** | 展示专业知识建立权威 | 建立专业形象，增强可信度 |

### 2. 实际工作流程示例

```
用户: 帮我为这个小红书笔记写一条专业类型的评论：https://www.xiaohongshu.com/explore/xxxx

Claude: 我会帮您写一条专业类型的评论。让我获取笔记内容并生成评论。
[调用post_smart_comment工具]

# 工具返回笔记分析结果，包含标题、作者、内容、领域和关键词

Claude: 我已经获取到笔记信息，这是一篇关于[主题]的笔记。基于内容，我生成并发布了以下专业评论：

"[生成的专业评论内容]"

[调用post_comment工具]

Claude: 评论已成功发布！
```

**注意**：上述流程中，`post_smart_comment`工具只负责获取笔记分析结果并返回给MCP客户端，实际的评论生成是由MCP客户端（如Claude）自身完成的。

### 3. 工作原理

新版小红书MCP工具采用了模块化设计，分为三个核心模块：

1. **笔记分析模块**（analyze_note）
   - 获取笔记的标题、作者、发布时间和内容
   - 分析笔记所属领域和关键词
   - 返回结构化的笔记信息

2. **评论生成模块**（由MCP客户端实现）
   - 接收笔记分析结果
   - 根据笔记内容和评论类型生成自然、相关的评论
   - 允许用户在发布前预览和修改评论

3. **评论发布模块**（post_comment）
   - 接收生成的评论内容
   - 定位并操作评论输入框
   - 发布评论并返回结果

## 五、代码结构

- **xiaohongshu_mcp.py**：实现主要功能的核心文件，包含登录、搜索、获取内容和评论、发布评论等功能的代码逻辑。
- **requirements.txt**：记录项目所需的依赖库。

## 六、常见问题与解决方案

1. **连接失败**：
   - 确保使用了虚拟环境中Python解释器的**完整绝对路径**
   - 确保MCP服务器正在运行
   - 尝试重启MCP服务器和客户端

2. **浏览器会话问题**：
   如果遇到`Page.goto: Target page, context or browser has been closed`错误：
   - 重启MCP服务器
   - 重新连接并登录

3. **依赖安装问题**：
   如果遇到`ModuleNotFoundError`错误：
   - 确保在虚拟环境中安装了所有依赖
   - 检查是否安装了fastmcp包

## 七、注意事项与问题解决

### 1. 使用注意事项

- **浏览器模式**：工具使用 Playwright 的非隐藏模式运行，运行时会打开真实浏览器窗口
- **登录方式**：首次登录需要手动扫码，后续使用若登录状态有效，则无需再次扫码
- **平台规则**：使用过程中请严格遵守小红书平台的相关规定，避免过度操作，防止账号面临封禁风险
- **评论频率**：建议控制评论发布频率，避免短时间内发布大量评论，每天发布评论数量不超过30条

### 2. 常见问题与解决方案

#### 浏览器实例问题

如果遇到“Page.goto: Target page, context or browser has been closed”类似错误，可能是浏览器实例没有正确关闭或数据目录锁文件问题，请尝试：

```bash
# 删除浏览器锁文件
rm -f /项目路径/browser_data/SingletonLock /项目路径/browser_data/SingletonCookie

# 如果问题仍然存在，备份并重建浏览器数据目录
mkdir -p /项目路径/backup_browser_data
mv /项目路径/browser_data/* /项目路径/backup_browser_data/
mkdir -p /项目路径/browser_data
```

#### 内容获取问题

如果无法获取笔记内容或内容不完整，可尝试：

1. **增加等待时间**：小红书笔记页面可能需要更长的加载时间，特别是包含大量图片或视频的笔记
2. **清除浏览器缓存**：有时浏览器缓存会影响内容获取
3. **尝试不同的获取方法**：工具集成了多种获取方法，如果一种方法失败，可以尝试其他方法

#### 平台变化适应

小红书平台可能会更新页面结构和DOM元素，导致工具无法正常工作。如遇到此类问题：

1. **检查项目更新**：关注项目最新版本，及时更新
2. **调整选择器**：如果您熟悉代码，可以尝试调整CSS选择器或XPath表达式
3. **提交问题反馈**：向项目维护者提交问题，描述遇到的具体问题和页面变化

## 八、免责声明

本工具仅用于学习和研究目的，使用者应严格遵守相关法律法规以及小红书平台的规定。因使用不当导致的任何问题，本项目开发者不承担任何责任。

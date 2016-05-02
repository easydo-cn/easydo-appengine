# easydo-appengine
the python app engine under easydo.cn

脚本化web应用开发引擎

## 功能

- 软件包管理
- 脚本的运行
- 规则
- 主模板
- 工作流
- 个人主目录管理

## 故事

### 站点

我们创建一个站点:

    >>> from easydo_appengine.site import Site
    >>> root = Site()
    >>> root.object_types
    ['Root', 'AppContainer', 'Container']

每个站点下面，都包含：

- 个人区 members 容器，存放登录人员相关的信息
- 软件包 packages 容器，存放安装的扩展应用软件包

我们可以看看:

    >>> print list(root.keys())
    ['members', 'packages']
    >>> root['members'].object_types
    ['HomeContainers', 'AppContainer', 'Container']
    >>> root['packages'].object_types
    ['PackagesContainer', 'Container']

在站点下面添加一个文件夹:

    >>> root.add_folder('files', '文件库')
    >>> print list(root.keys())
    ['members', 'packages', 'files']
    >>> root['files'].object_types
    ['Folder', 'Container']
    >>> root['files'].add_file('test.txt', 'sample text')
    >>> root['files']['test.txt'].object_types
    ['File', 'Item']

也可以添加一个表单库:

    >>> root.add_datacontainer('tasks', '任务单')
    >>> print list(root.keys())
    ['members', 'packages', 'files', 'tasks']
    >>> root['tasks'].object_types
    ['DataContainer', 'Container']
    
### 成员管理

我们可以进行成员管理：

    root.profiles.get_home(pid)

### 软件包管理

安装一个软件包:

    root.packages.install('zopen.tasks')

也可以升级软件包:

    root.packages.upgrade('zopen.tasks')

得到软件包中的一个表单定义，渲染一个表单：

    form_obj = root.packages.get_form_obj('zopen.tasks:task')
    form = ui.form(form_obj).button('save', 'Save').on('submit', submit_url)
    return form.html()

### 表单

得到软件包中的流程定义，执行流程:

    workflow_obj = root.pakcages.get_workflow_obj('zopen.tasks:task')
    task.workitems.start('zopen.tasks:task')

    >>> root['tasks'].add_dataitem('001', data={'title':'fix some bug', description='bug description'})
    >>> task = root['tasks']['001']
    >>> task.object_types
    ['DataItem', 'Item']


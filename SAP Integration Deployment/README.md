# SAP与Power Platform集成方案配置部署

## 业务背景

待补充

## 前提准备

待补充

## 部署解决方案

**1.新建Unmanaged Solution，将需要修改的部分资产移入**

在solution导入环境成功后，新建非托管解决方案，将导入解决方案中的所有Flow以及环境变量添加进新的解决方案。总计20个云端流程,3个环境变量。如下图所示：

![new_solution](./Images/add_new_solution.png)

**2.修改环境变量默认值**

修改环境变量的default值，修改内容如下：
| 变量名称        | 修改说明   |
| --------   | :-----  |
| SAP Application Server     | 您部署的SAP应用服务器地址   |
| SAP Development       |   {"AppServerHost":"你的应用服务地址","Client":"你的客户端代码","SystemNumber":"你的系统编号","LogonType":"ApplicationServer"}   |
| SAP SSO with Load Balancing        |    无需修改    |

修改位置参考下图：
![modify_variable](./Images/modify_variable.png)

**3.修复流程，并全部开启**

  _3.1修复流程中所有的SAP ERP连接_

  初始状态，进入每个流程的编辑页面，你会发现每个SAP ERP的Action都有错误提示，如下：
  ![flow_error](./Images/flow_error.png)

  点击新建连接引用，创建SAP ERP的Connection,如下所示：
  ![build_connection](./Images/build_connection.png)

  修复完成后，点击右上角进行保存操作。返回流程列表，将流程设置为开启状态，如下图：
  ![turnon_flow](./Images/turnon_flow.png)

  针对列表中的flow,逐一进行修复操作，确保所有的20个flow最终状态都为打开状态。
  从第二个流程修复开始，无需重新建立connection，复用原有的connection即可，请参考下图：
  ![click_connection](./Images/click_connection.png)
  部分流程中需要更改相关SAP参数配置：
  ![fix_connection](./Images/fix_connection.png)
  确认最终所有的流程状态如下：
  ![turn_on_flow](./Images/turn_on_flow.png)

  图示未开启的流程需要Office365 License，非必须开启。

  _3.2针对特殊流程进行参数修改_

  进入ReadCustomerList，按照下图将客户端代码进行修改
  ![fix_flow1](./Images/fix_flow1.png)

**4.导入相关业务主数据**

  _4.1 按照文档下载数据导入工具，参考[环境间迁移数据](https://learn.microsoft.com/en-us/dynamics365/marketing/transfer-data)_

  _4.2 下载数据包，请前往[文档路径configdata.zip](https://github.com/charlielv926/Biz-App-TechSolution/blob/main/SAP%20Integration%20Deployment/Asset/configdata.zip)_

  _4.3 借助工具（CRM Configuration Migration）导入数据包。_

  ![import_data1](./Images/import_data1.png)

  ![import_data2](./Images/import_data2.png)

  _4.4 提示导入成功后，请前往solution中检查配置数据情况。_
  >MenuItem
  >MenuGroup
  >DropDownValues

  ![import_data3](./Images/import_data3.png)

  _4.5 修改App菜单配置数据。在App列表中，分别找到【Purchase Orders】和【Sales Orders】应用所对应的URL，如下图所示：_
  ![import_data4](./Images/import_data4.png)

  _4.6 找到MenuItems表，并分别替换两个App菜单的URL,如下图所示：_
  ![import_data5](./Images/import_data5.png)

**5.修复Canvas App中的连接**

  _5.1 进入Canvas的编辑页面。以【Purchase Orders】为例，在左侧栏Power Automate选项卡中，可以发现连接状态都处于 ___未连接___ 状态_
  ![fix_canvas1](./Images/fix_canvas1.png)

  _5.2 逐一将连接进行"删除"和"再添加"的操作。直至所有的连接状态正常，如下图所示：_
  ![fix_canvas2](./Images/fix_canvas2.png)

  _5.3点击右上角进行保存和发布操作。确保两个Canvas App的连接状态都正常。_

## 联系人信息

任何问题，请联系：Charlielv0926@163.com

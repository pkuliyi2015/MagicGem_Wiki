---
description: 配置、命令和权限
---

# 插件用法

## 全局配置文件

```
#====================
# MagicGem v1.05 正式版
# 使用说明详见WIKI
# 插件验证管理系统
#   42.192.128.205
#====================
lang: zh_CN
# 若无通知, 请不要更改此处IP地址
license-ip:
  - 42.192.128.205:2021
# 在下方填写您的用户名.
# 注意冒号后面需要有一个空格！
user:
# 全局设置
# 工作台检测开关
workbench: true
# 背包检测开关
backpack: false
# Lore识别宝石 检测开关
gem-lore: false
# 是否允许将宝石作为原版合成材料（不推荐修改）
gem-craft: false
# 技能引擎开关
skill-engine: true
# 全局药水限制
potion-global-limit:
  example-world: 5
```

## 指令

在游戏内输入mgem 或mgem help即可看到如下命令

```
/mgem reload   重载配置文件
/mgem apply <玩家> <宝石名> (槽位) - 给玩家的指定装备位置使用1颗宝石(默认主手)
/mgem give <玩家> <宝石名> (数量)   给玩家指定数量的某种宝石(默认1个)
/mgem get   打开管理员GUI, 查看说明并获取宝石
/mgem view   打开宝石预览页面
/mgem embed (镶嵌台名) 打开镶嵌台, 镶嵌或拆卸装备宝石 (默认名字是default)
/mgem embed addblock (镶嵌台名)  指定脚下方块为镶嵌台
/mgem embed delblock (镶嵌台名)  删除脚下的镶嵌台方块
/mgem signal (玩家|@all) (信号内容) - 触发全体玩家或某个玩家的信号技能"
# 备注: 下列功能可以用Tab读到输入栏直接复制(颜色代码会转换成&)"
/mgem enchant   列出主手物品的附魔(包括其它插件附魔)
/mgem lore   列出主手物品的Lore
/mgem material   显示主手物品的名称和子ID
/mgem nbt   列出主手中物品的NBT标签和类型
/mgem nbt <属性名>   读取主手物品的指定属性
```

## 权限

宝石的权限系统尚不完善，尚待后续改进。当前权限如下：

* magicgem.command.admin 一切权限
* magicgem.command.view 宝石预览权限 ​
* magicgem.command.embed 指令打开镶嵌台权限


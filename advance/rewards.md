---
description: 宝石奖励详细指南
---

# 奖励大全

奖励是宝石插件的功能核心。分为物品奖励和玩家奖励。

* 物品奖励只对物品宝石有效
* 玩家奖励对物品和玩家宝石都有效
* 你只能把奖励写在物品或玩家宝石里，写在随机宝石里不起作用。

物品奖励的格式与MythicMobs类似

```
Rewards:
  - 奖励1类型{参数1=值1;参数2=值2;..}
  - 奖励2类型{参数1=值1;参数2=值2;..}
```

* 这些奖励会严格按照顺序执行。使用正序，拆卸逆序
* 某些参数有别名，使用别名也可以识别。

## **物品奖励**

1.  **原版属性**

    为装备增加指定的原版属性

    ```
    - Attribute{name=属性英文;operation=操作;slot=生效部位;var=强化公式;inv=拆卸公式;limit=上限}
    ```

    * operation有别名op
    * 可用属性英文共9个，**注意拼写不能错误**

    | 属性     | 生命上限    | 移动速度    | 攻击速度             | 攻击击退                  | 物理伤害   |
    | ------ | ------- | ------- | ---------------- | --------------------- | ------ |
    | 名称     | health  | move    | attack\_speed    | attack\_knockback     | damage |
    | **属性** | **幸运值** | **盔甲值** | **盔甲韧性**         | **击退抗性**              |        |
    | 名称     | luck    | armor   | armor\_toughness | knockback\_resistance |        |

    * 操作有三种：0（加和）1（增加一个比例）2（增加总系数）
    * 生效部位有七个选项："mainhand", "offhand", "head", "chest", "legs", "feet", "auto"
      * 分别对应主手、副手、头盔、胸甲、护腿、靴子，以及根据装备类型自动推断
    * var是**表达式**，用于计算强化后的属性，但此处不能使用物品或PAPI变量，且必须包含字母v。例如
      * var=v+1 表示将属性值+1
      * var=1.05\*v表示将属性增加5%
      * var=v+$RANDOM()表示属性随机增加0-1
    * inv也是和var一样的**表达式**，用于拆卸时还原属性，不能使用物品或PAPI变量。
      * **没有inv则宝石不可拆卸也不可降级。**
      * 请将inv写成var的逆计算式子
      * 如var=v+1，则inv=v-1
    * **之后的奖励中如有强化公式与拆卸公式，与此处完全一致，不再赘述**
    * limit是上限。填任意大于零整数，则属性超过上限时不能再打宝石。
2.  **物品条件奖励**

    判断物品是否满足条件，若满足则执行奖励

    ```
     - Conditional{condition=条件公式;reward=其它物品或玩家奖励}
    ```

    * condition有别名c，reward有别名r
    * condition需要填写一个逻辑公式，支持宝石物品变量和玩家PAPI
    * reward可以套娃宝石的**任何奖励**
    * 不满足条件则无法强化
3.  **CustomModelData**

    为装备添加CustomModelData，使其可以配合材质包

    ```
     - CustomModelData{id=数字;force=是否强制添加}
    ```

    * ID为你想要加的数字
    * force有别名f。控制是否强行给装备添加CustomModelData，默认为true
      * 如果为false且装备已经有CustomModelData，则不能再打宝石
    * **拆卸时CustomModelData会被移除**
4.  **销毁物品**

    将被打宝石的物品或装备变为空气

    ```
     - ItemDestroy
    ```

    * **此奖励不可拆卸**
5.  **耐久度**

    恢复装备耐久

    ```
     - Durability{amount=耐久度数量}
    ```

    * 为防万一，请不要对没有耐久的物品使用此奖励
    * 物品满耐久的情况下也可以用
    * 此奖励拆卸不会引发物品耐久度下降
6.  **空奖励**

    什么也不做

    ```
     - Empty
    ```
7.  **附魔**

    强化装备附魔

    ```
     - Enchant{name=附魔名;level=要提升的等级;limit=上限}
    ```

    * 此处附魔名称是游戏内置的附魔名，因此可能和您的想象完全不同
    * 请在游戏内用/mgem enchant查看正确的附魔名
    * 兼容EcoEnchants、GoldenEnchants附魔插件
8.  **附魔设置**

    将装备附魔设置到一个指定的等级

    ```
     - EnchantSet{name=附魔名;level=要设置的等级}
    ```

    * 此处附魔名称是游戏内置的附魔名，因此可能和您的想象完全不同
    * 与Enchant不同，这里不是提升多少级，而是设置为多少级
    * 0级代表去除指定附魔
9.  **物品标志**

    为装备添加某些原版标志

    ```
     - ItemFlag{flag=标志名}
    ```

    * flag有别名f
    *   可用的标志有七种：

        | 标志名                   | 作用            |
        | --------------------- | ------------- |
        | HIDE\_ATTRIBUTES      | 隐藏装备原版属性      |
        | HIDE\_ENCHANTS        | 隐藏装备附魔        |
        | HIDE\_UNBREAKABLE     | 隐藏装备的不可破坏属性   |
        | HIDE\_POTION\_EFFECTS | 隐藏药水效果        |
        | HIDE\_DYE             | 隐藏皮革护甲的彩色燃料信息 |
        | HIDE\_DESTROYS        | _隐藏装备可破坏的东西_  |
        | HIDE\_PLACED\_ON      | _隐藏装备可被放置的地方_ |
    * 注意HIDE\_DYE只在1.16.3以后的版本存在
    * 最后两个标志的作用作者也不清楚
10. **直接更改物品**

    将装备直接设置为指定的物品。物品通过材料构造

    ```
     - ItemSet{材料}
    ```

    * 材料的格式与宝石外观一节中的材料格式完全一致
    * **但是你需要压缩到一行来写，用=号赋值并用分号分割**
    * 例如 - ItemSet{Material=STONE;Glow=true}
    * 你还可以直接给宝石，格式是 - ItemSet{gem=宝石名;amount=数量}
    * 数量超过64个时会放到背包，若背包也放不下则会掉落在地上
11. **拆卸宝石**

    允许在打一颗宝石的时候尝试移除另一颗已经镶嵌的宝石，适用于宝石升级

    ```
     - GemRemove{name=宝石名;clear=是否清除该宝石的奖励}
    ```

    * 尝试从已经镶嵌的宝石中拆一颗下来
    * 如果undo取true（默认）则拆卸的时候会尝试清除该宝石的效果，清除失败则无法打宝石
    * 否则强行移除宝石，无视其效果
12. **奖励表**

    用于将多个物品奖励拼成一行

    ```
     - List{reward=奖励1;reward=奖励2;...}
    ```
13. **添加Lore**

    给装备添加Lore，支持颜色代码&

    ```
     - LoreAdd{lore=要添加的文本;mode=模式;locator=定位lore;limit=添加次数上限;force=是否强行添加}
    ```

    * mode是模式，有五种可选:
      * first(第一行),
      * last(最后一行)
      * before(在locator前)
      * after(在locator后)
      * line(在locator那行追加lore)
    * before，after，line这三种模式均要求使用locator
    * **物品的lore只需要包含locator即可被识别，识别无视颜色**
    * force为true或false，控制在未找到locator时，是否强行添加lore。**默认为true**
      * 设置为true时，如果没找到lore
        * 如果mode=before，插件会在lore的**第一行**处插入一行lore，一行locator
        * 如果mode=after，插件会在lore的末尾追加一行locator，一行lore
        * 如果mode=line，插件会在lore的最后加入一行locator拼接lore
      * 设置为false时，如果mode为before，after，line之一且没找到locator，则无法打宝石
    * limit为添加次数上限，采用NBT记录，哪怕物品上添加的lore被修改了也能正确工作。
14. **删除Lore**

    删除物品上指定的Lore，支持颜色代码&

    ```
     - LoreDel{lore=要删除的文本;mode=模式}
    ```

    * mode是模式，有三种可选：
      * first（从前往后查找，删除第一个发现的lore）
      * last（从后往前查找，删除第一个发现的lore）
      * all（删除所有lore）
    * 插件会匹配包含指定lore（可以为空字符串""）的所有行，且不会忽略颜色
    * **拆卸时lore会被加回到第一行(first)或最后一行(其它情况)**
15. **替换Lore**

    将物品上指定的Lore替换为新的lore，支持颜色代码&

    ```
    - LoreReplace{old=要替换的旧lore;lore=要添加的新lore;mode=模式;locator=定位器;limit=替换限制次数}
    ```

    * mode是模式，有四种可选：
      * first替换正数第一行发现的lore
      * last替换倒数第一行发现的lore
      * all只要匹配全部替换,
      * line替换locator所在的行内的lore
    * **注意locator只对行内替换有效，识别无视颜色**
16. 替换多行Lore

    将物品上多行lore替换为新的多行lore，支持颜色代码&

    ```
    - LoreMultiReplace{old=要替换的第一行;第二行;第三行;lore=要添加的新lore第一行;第二行;第三行;limit=替换限制次数}
    ```
17. **Lore变量编辑**

    编辑物品Lore数字

    ```
    - LoreVar{lore=变量名;var=强化公式;inv=拆卸公式;limit=限制公式;format=格式;(允许新增才填)mode=模式;locator=定位lore;prefix=是否把数字加到宝石前面}
    ```

    * 变量名：要编辑的数字前面的识别标志。如"物理伤害"。
      * 插件将会读取其后的第一个整数或者浮点数参与计算
      * 识别变量名和读取数字均忽略颜色代码
    * var是**强化公式**，inv是**拆卸公式**（与原版属性强化相同），ivar必填，没有inv则无法拆卸。
    * limit是**限制公式**，必须包含v。与强化公式的不同之处在于，这个公式的结果必须是逻辑值（true或false）
      * 比如，v<=10的意思是变量v必须在10以内，超出则无法强化
      * 你可以编写更加复杂的逻辑表达式，如 v>=2&\&v<=8 要求v大于等于2且小于等于8
    * format是格式。此处使用C语言scanf的浮点数格式，具体而言
      * format=%.nf，其中n代表小数位数。如%.3f表示保留三位小数
      * 除了%之外的其它字符都可以随便添加，比如format=【速度+ %.2f】
      * 如果你要添加百分号，请加入两个连续的%%。这是必须遵守的语言限制
      * 支持罗马数字，这时候就不需要%.nf了，直接使用ROMAN。例如format=【斗之力 ROMAN 段】
    * mode是模式。变量名没有找到时，如果设置了模式，则插件会根据locator在合适的位置添加变量名。有五种模式可选：
      * mode=first，在第一行添加变量名
      * mode=last，在最后一行添加变量名
      * mode=before，若找到locator则在locator前一行添加变量名，否则在最后一行添加
      * mode=after，若找到locator则在locator后一行添加变量名，否则在最后一行添加
      * mode=line，若找到locator则将其替换为变量名，**否则无法打宝石**
    * locator是定位器，结合mode使用，把变量加到locator的前面、后面或替换之
    * **一旦触发添加，变量值将被设置为0，代入强化公式算出结果**
    * prefix为true或false，为true时会把属性值加在词条的前面，false时加在后面
    * **每一条仅只支持一个数字，暂不支持范围。你可以曲线救国：**
      * 要强化范围，用特殊符号做标记，然后写两条LoreVar. 例如
      * ```
        - LoreVar{lore=物理伤害：;var=v+1;inv=v-1;format=%.0f - (&0第二个数字的特殊黑色标记符号)}
        - LoreVar{lore=(&0第二个数字的特殊黑色标记符号);var=v+1.2;inv=v-1.2;format=%.0f}
        ```
18. **物品材料修改**

    修改物品或装备的材料，将一种物品变成另一种物品

    ```
     - Material{material=材料名}
    ```

    * material有别名m
    * **拆卸时不会还原材料**
19. **MMOItems技能添加和强化**

    在服务端装有MMOItems前置时，使用这个奖励可以为物品添加MMOItems技能

    ```
    - MMOAbility{id=技能;mode=触发模式;属性名=属性值{var=强化公式;inv=拆卸公式;limit=限制公式}}
    ```

    * 技能可在MMOItems wiki上寻找\[Ability List · Wiki · MythicCraft / MMOItems · GitLab]\(https://git.mythiccraft.io/mythiccraft/mmoitems/-/wikis/Ability List)
    * 属性名等价于wiki中的modifier，属性值是不做强化时的初始值
    * 强化、拆卸和限制公式写法和Lore变量编辑相同
20. **MythicArtifact或MythicCrucible物品欺骗**

    在服务端装有MythicArtifacts或MythicCrucible之一时，该奖励会改写物品特征，使其被MythicArtifacts或MythicCrucible识别，从而实现打宝石释放出MM技能。

    ```
     - MythicItem{name=MM物品名;force=是否强制改写}
    ```

    * 对于MythicCrucible，该奖励会添加一个名为MYTHIC\_TYPE的NBT标签
    * 对于MythicArtifacts，该奖励会修改武器的显示名和材料
    * 这个奖励拆卸时会移除MYTHIC\_TYPE标签或者移除显示名
21. **物品改名**

    修改物品显示名，支持颜色代码&

    ```
     - Name{name=新名字}
    ```

    * name有别名n
    * **拆卸不会还原名字**
22. **物品名称数字修改**

    修改物品显示名，支持颜色代码&

    ```
     - NameVar{name=已有的名字的一部分;var=强化公式;inv=拆卸公式;limit=限制公式;format= 格式;force=是否强制改名}
    ```

    * 同上，name有别名n，**拆卸不会还原名字**
    *   用法和LoreVar一样，但是修改装备名称里面，已有的名字后面的第一个数字。暂不支持改第 二个数字

        例如你有一把倚天剑+1，打了NameVar以后，可以把它变成倚天剑+2

        * NameVar{name=&6倚天剑;var=v+1;inv=v-1;format=+%.0f}
    * 和LoreVar一样，也支持罗马数字(改成+I，+II，+III，+IV......)
23. **物品NBT浮点数修改**

    修改物品的NBT浮点数，适用于在装备上存入玩家看不见的数字，或者修改一些使用NBT作为属性的插件的属性（比如MMOItems）

    ```
     - NBTDouble{name=数值名称;var=强化公式;inv=拆卸公式;limit=限制公式}
    ```

    * name有别名key
    * var、inv、limit的写法与Lore变量编辑完全一致
24. **物品NBT字符串修改**

    修改物品的NBT字符串

    ```
     - NBTString{name=字符串名称;var=字符串内容;force=是否允许覆盖}
    ```

    * name有别名key
    * var如果不写就是删除整个字符串（此时奖励不可拆卸）
25. **添加NBT技能**

    为物品添加宝石技能库中的技能，采用NBT识别因此玩家不能在装备上看见

    * 备注：如果你打算用Lore识别，请直接用添加Lore奖励

    ```
     - SkillAdd{name=技能名}
    ```

    * name有别名skill，s
    * **不能重复添加**
26. **删除NBT技能**

    删除物品上的某个宝石技能

    ```
    - SkillDel{name=技能名}
    ```

    * name有别名skill，s
    * 技能名就是宝石技能库里面的条目
    * **装备上没有此技能则无法打宝石**
27. **替换NBT技能**

    将物品上的某个宝石技能替换为另一个技能

    ```
     - SkillReplace{old=旧技能名;new=新技能名}
    ```

    * new有别名name，skill，s
    * **装备上没有旧技能则无法替换**
28. **Lore技能转NBT技能**

    将物品上的某个靠Lore触发的宝石技能转换为靠NBT触发，从而实现技能隐藏的效果

    ```
     - SkillToNBT{name=技能名}
    ```

    * name有别名skill，s
    * **宝石上没有指定技能，或指定技能没有识别Lore时无法打宝石**
29. **无法破坏**

    使装备变得无法破坏

    ```
     - Unbreakable
    ```

**物品奖励通用标志：**

* $ignorable：跟在某条奖励后面。如果有这个标志，哪怕奖励无法执行，宝石也将继续强化。

## **玩家奖励**

1.  **动作栏消息**

    通过动作栏向玩家发送消息，支持颜色代码&

    ```
    - ActionBar{messages=消息1;消息2;消息3..;interval=间隔的ticks数}
    ```
2.  **聊天栏消息**

    通过聊天栏向玩家发送消息，支持颜色代码&

    ```
     - Chat{messages=消息1;消息2;消息3..;interval=间隔的ticks数}
    ```
3.  **发送标题**

    向玩家发送标题信息，支持颜色代码&

    ```
     - Title{messages=大标题1;小标题1;大标题2;小标题2;..;interval=间隔的ticks数;fadein=淡入ticks数;fadeout=淡出ticks数;stay=停留ticks数}
    ```

    **备注：上述三个奖励的messages均有别名m**
4.  **执行指令**

    执行指令，支持PlaceholderAPI

    ```
     - Command{command=指令;console=是否后台执行;op=是否以OP身份执行}
    ```

    * command有别名c
    * console为true或false，控制是否由后台执行，**默认为true**
    * op为true或false，仅当console为false时生效。默认为false
      * 为true将临时给予玩家op权限执行此指令
      * 完全同步执行，并使用try-finally保证不让玩家卡op
      * 若为false则仅仅以玩家身份执行
5.  **给予经验等级**

    给予或扣除玩家一定的原版经验等级

    ```
     - ExpLevel{amount=表达式}
    ```

    * amount有别名level，l
    * amount为**表达式**，你可以使用PAPI和各种函数
    * **可以使用物品变量，但仅在物品宝石中生效，在玩家宝石中无效**
    * 计算结果为正数则增加玩家经验等级，为负数则扣除玩家经验等级
6.  **治疗玩家**

    治疗或伤害玩家一定生命

    ```
     - Heal{amount=非0小数}
    ```

    * 为正数则治疗玩家，为负数则伤害玩家
    * 不会超过实际生命上限
7.  **玩家装备编辑**

    对玩家某个槽位的装备施加物品奖励

    ```
     - ItemEdit{slot=槽位名;reward=物品奖励}
    ```

    * 槽位名与宝石配置中的FastUse写法相同
    * 物品奖励可以套娃一切宝石物品奖励。如reward=Durability{amount=1000}
8.  **玩家物品给予**

    给予玩家一定数量的物品。物品通过材料解析

    ```
     - ItemGive{材料}
    ```

    * 材料的写法与物品奖励ItemSet的完全相同
    * 数量超过背包空位时会掉在地上
9.  **玩家物品消耗**

    从背包中收取玩家物品

    ```
     - ItemTake{物品1=数量1;物品2=数量2..;}
    ```

    * 物品的格式与宝石Require完全相同。例如
    * ItemTake{DIAMOND=1;LORE:强化材料=2}
10. **玩家最大生命值奖励**

    增加或减少玩家生命上限

    ```
     - MaxHealth{amount=整数;limit=上限}
    ```

    * amount为正数则增加，为负数则减少，为0则只进行上限检查
    * 生命上限只计算裸装基础值，不包括盔甲的附加值
11. **金币奖励**

    给玩家金币或者向玩家收费

    ```
     - Money{amount=表达式}
    ```

    * amount为**表达式**，你可以使用PAPI和各种函数
    * **可以使用物品变量，但仅在物品宝石中生效，在玩家宝石中无效**
    * 计算结果为正数则给玩家钱，为负数则向玩家收费
12. **点券奖励**

    给玩家点券或者向玩家收费

    ```
     - Point{amount=表达式}
    ```

    * amount为**表达式**，你可以使用PAPI和各种函数
    * **可以使用物品变量，但仅在物品宝石中生效，在玩家宝石中无效**
    * 计算结果为正数则给玩家点券，为负数则向玩家收费
13. **给予MM物品**

    从MM物品库中取得物品并给予玩家

    ```
     - MythicItemGive{name=物品名;amount=数量}
    ```

    * 数量超过背包容量时会掉在地上
14. **原版药水效果**

    给玩家指定等级和市场的原版药水效果

    ```
     - Potion{name=药水名;level=药水等级;duration=持续的秒数}
    ```

    *   截至1.17.1可用药水共32种，注意某些药水在低版本不能使用。下表展示了药水和有效的别名

        | 伤害吸收                                                    | 不祥征兆                                                                | 失明                                     | 潮涌能量                                                                     |
        | ------------------------------------------------------- | ------------------------------------------------------------------- | -------------------------------------- | ------------------------------------------------------------------------ |
        | ABSORPTION，ABSORB                                       | BAD\_OMEN，OMEN\_BAD, PILLAGER                                       | BLINDNESS, BLIND                       | CONDUIT\_POWER, CONDUIT, POWER\_CONDUIT                                  |
        | **反胃**                                                  | **伤害抗性**                                                            | **海豚的恩惠**                              | **急迫**                                                                   |
        | CONFUSION, NAUSEA, SICKNESS                             | DAMAGE\_RESISTANCE, RESISTANCE, ARMOR, DMG\_RESIST, DMG\_RESISTANCE | DOLPHINS\_GRACE, DOLPHIN, GRACE        | FAST\_DIGGING, HASTE, SUPER\_PICK, DIGFAST, DIG\_SPEED, QUICK\_MINE      |
        | **防火**                                                  | **发光**                                                              | **瞬间伤害**                               | **治愈**                                                                   |
        | FIRE\_RESISTANCE, FIRE\_RESIST, RESIST\_FIRE            | GLOWING, GLOW, SHINE, SHINY                                         | HARM, DAMAGE, INJURE, HARMING, INFLICT | HEAL, HEALTH, INSTA\_HEAL, INSTANT\_HEAL, INSTA\_HEALTH, INSTANT\_HEALTH |
        | **生命提升**                                                | **村庄英雄**                                                            | **饥饿**                                 | **力量**                                                                   |
        | HEALTH\_BOOST, BOOST\_HEALTH, BOOST, HP                 | HERO\_OF\_THE\_VILLAGE, HERO, VILLAGE\_HERO                         | HUNGER, STARVE, HUNGRY                 | INCREASE\_DAMAGE, STRENGTH, BULL, STRONG, ATTACK                         |
        | **隐身**                                                  | **跳跃提升**                                                            | **漂浮**                                 | **幸运**                                                                   |
        | INVISIBILITY, INVISIBLE, HIDE, VANISH, INVIS, DISAPPEAR | JUMP, LEAP, JUMP\_BOOST                                             | LEVITATION, LEVITATE                   | LUCK, LUCKY                                                              |
        | **夜视**                                                  | **剧毒**                                                              | **生命恢复**                               | **饱和**                                                                   |
        | NIGHT\_VISION, VISION, VISION\_NIGHT                    | POISON, VENOM                                                       | REGENERATION, REGEN                    | SATURATION, FOOD                                                         |
        | **缓慢**                                                  | **挖掘疲劳**                                                            | **缓降**                                 | **速度**                                                                   |
        | SLOW, SLOWNESS, SLUGGISH                                | SLOW\_DIGGING, DULL, FATIGUE, DIG\_SLOW, DIGGING, SLOW\_DIG         | SLOW\_FALLING, SLOW\_FALL, FALL\_SLOW  | SPEED, SPRINT, RUN\_FAST, SWIFT, FAST                                    |
        | **厄运**                                                  | **水下呼吸**                                                            | **虚弱**                                 | **凋零**                                                                   |
        | UNLUCK                                                  | WATER\_BREATHING                                                    | WEAKNESS                               | WITHER                                                                   |
    *
15. **发动技能**

    发动宝石技能或者MM技能

    ```
     - Skill{skill=技能名}
    ```

    * skill有别名s
    * skill既可以是宝石技能库中的技能，也可以是mm技能库中的技能
    * 注意，这里**不能使用目标选择器**。请在宝石或者MM技能组中写目标选择器
    * **即便技能因为没有目标而发动失败，宝石也会被消耗**。请合理编写技能，使其发动必然成功。

**玩家奖励通用标志**

* $onSuccess 加在任何玩家奖励后面
  * **有这个标志时，奖励在宝石使用成功后才会执行**
* $onFail 加在任何玩家奖励后面
  * **有这个标志时，奖励在宝石使用失败后才会执行**
* 上述两个标志都没有时，不论成功与否奖励都会执行
* $onRemove 加在任何玩家奖励后面，**只对可镶嵌的物品宝石生效**
  * 有这个标志时，该奖励只在拆卸宝石时执行

## 宝石条件

* _1.05版条件暂时只有三个_
*   格式为

    ```
    Conditions:
     - 条件1
     - 条件2
    ```
* 同样分为物品条件和玩家条件

### **玩家条件**

1.  PAPI条件

    检查玩家PAPI是否满足条件

    ```
     - PAPI{p=逻辑公式;tip=失败提示}
    ```

    * 逻辑公式和物品条件奖励中的条件写法一致。举个例子，如果你想要判断某个变量（如%level%）的大小：
    * ```
      - PAPI{p=%level%>=5;tip=你必须达到五级才能使用该宝石}
      ```
    * tip是字符串，失败提示。只能显示在聊天栏
2.  玩家权限

    检查玩家有或者没有某权限

    ```
     - Permission{p=权限;has=有或无}
    ```

    * has=true时，有权限则通过检查；has=false时，无权限才能通过检查
3.  背包空位

    检查玩家背包空位数目

    ```
     - InvEmpty{amount=需要的空位数目}
    ```

    * 若玩家背包空位不足，则不能通过检查

### **物品条件**

* 暂无

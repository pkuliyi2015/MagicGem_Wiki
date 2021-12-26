---
description: 技能配置方法
---

# 宝石技能

## **基本配置**

* 宝石技能保存在配置文件夹的skills目录下
* 以yml形式储存，**一段一个技能，段名就是技能名**
* 识别方式：支持四种识别方式
  1. 不做任何配置：用宝石奖励SkillAdd打在装备上的技能，默认是NBT识别
  2. Lore：填写要识别的Lore，武器上有这个Lore时就能够放技能
     * **注意，识别无视颜色且只识别前缀**
       * 也就是说，如果一个Lore是 技能I，另一个是 技能II
       * 则技能II总是会被识别为技能I，因为
       * 技能II = 技能I + I
     * **总而言之，技能的识别Lore文字不能互为前缀，否则只识别短的那个技能。**
     * **这一设计是因为药水BUFF的同一个Lore前缀对应多个等级的药水的缘故，还请理解**
  3. Display：装备显示名
     * 支持颜色代码&
  4. MythicType：识别MMC武器
     * 这个标签是MythicCrucible自动加在装备上的标签
     * MM版本4.13以下，需要在MM武器上写一个空的Skills，才会产生这个标签
* Slot：字符串或字符串列表，限制技能只能在哪些槽位上发动。
  * 槽位的写法与宝石FastUse格式完全相同
* Success：技能发动成功率**表达式**，与宝石成功率写法完全相同
* SuccessTip：技能发动成功的提示信息，与宝石提示信息写法完全相同。如果不写就没有信息
* FailTip：技能发动失败的提示信息，同上
* Cooldown：技能冷却时间，单位为秒
* CooldownTip：冷却时间提示，格式为
  * ```
    位置{p=提示前缀;s=提示后缀;empty=空进度格子样式;full=满进度格子样式;length=进度条长度}
    ```
  * 其中位置同样可以选择ActionBar（动作栏）、Chat（聊天框）、Title（标题）
  * **提示后缀必须要包含浮点数占位符%.nf**
  * 例如，如下配置会在玩家动作栏显示
    * 技能冷却时间:||||||||||||||||||| x.xx秒
  * ```
    CooldownTip: ActionBar{p=&a技能冷却时间;s=&a%.2fs;empty=&f|;full=&a|;length=75}
    ```
* Duration：**持续时间技能专用**，指定这项后，技能中必须要有TimeStartSkill来启动持续时间。单位为秒。
* Skills：字符串列表，与MythicMobs技能类似，一行一个技能
* Conditions：字符串列表，技能施法者条件，与MythicMobs条件类似，一行一个条件
* TriggerConditions：字符串列表，技能触发者条件，与MythicMobs条件类似，一行一个条件
* TargetConditions：字符串列表，技能目标条件，与MythicMobs条件类似，一行一个条件
* **本插件完全兼容MM技能。你可以直接把技能挪到宝石装备库中**

## **触发器**

* 宝石技能有54个触发器，并且支持复合触发信息
* 与MythicMobs一样，触发器均以短波浪线+on开头，跟在技能后面
* **复合触发信息的使用方式为\~on触发器:可触发信息1;可触发信息2;...**
  * 用冒号紧跟触发器，不要加空格
  * 用分号分隔每个触发信息
  * 例如\~onKill:PIG;COW 触发器就能被杀死猪或者牛触发
*   下面是每个触发器和详细说明

    | 触发器               | 触发事件           | 复合触发信息                                                                                                            |
    | ----------------- | -------------- | ----------------------------------------------------------------------------------------------------------------- |
    | Jump              | 跳跃             |                                                                                                                   |
    | Signal            | 指令/mgem signal | 信号名                                                                                                               |
    | Counter           | 计数技能发动         | 计数技能tag                                                                                                           |
    | ShiftSwing        | Shift+左键       |                                                                                                                   |
    | ShiftUse          | Shift+右键使用物品   |                                                                                                                   |
    | ShiftRightClick   | Shift+右键空地     |                                                                                                                   |
    | KillEntity        | 杀死实体           | 杀死的生物类型                                                                                                           |
    | KillMythicMobs    | 杀死MythicMobs怪物 | MythicMobs怪物名                                                                                                     |
    | DamagedProjectile | 被投射物打伤         | 射箭生物类型                                                                                                            |
    | DamagedBlock      | 被方块弄伤          | 伤害类型， [详见网址](https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/entity/EntityDamageEvent.DamageCause.html) |
    | DamagedEntity     | 被实体攻击受伤        | 攻击者生物类型                                                                                                           |
    | Drop              | 丢物品            | 物品材料名                                                                                                             |
    | TridentHit        | 三叉戟**落地**      |                                                                                                                   |
    | Hit               | 投射物**落地**      | 投射物类型                                                                                                             |
    | BowHit            | 弓箭打伤生物         | 目标生物类型                                                                                                            |
    | TridentAttack     | 三叉戟打伤生物        | 目标生物类型                                                                                                            |
    | SnowballAttack    | 雪球打伤生物         | 目标生物类型                                                                                                            |
    | FireballAttack    | 火焰弹打伤生物        | 目标生物类型                                                                                                            |
    | ProjectileAttack  | 投射物打伤生物        | 目标生物类型                                                                                                            |
    | Combat            | 战斗（攻击、受伤）      | 涉及的生物类型                                                                                                           |
    | Teleport          | 传送触发           |                                                                                                                   |
    | Block             | 放置方块           |                                                                                                                   |
    | TridentThrow      | 投掷三叉戟          |                                                                                                                   |
    | Despawn           | 死亡或退出游戏        |                                                                                                                   |
    | Tame              | 驯服生物           | 驯服的生物类型                                                                                                           |
    | Attack            | 攻击             | 目标生物类型                                                                                                            |
    | Shoot             | 射箭             |                                                                                                                   |
    | BlockPlace        | 放置方块           |                                                                                                                   |
    | BlockBreak        | 破坏方块           | 破坏的方块类型                                                                                                           |
    | Consume           | 食用/消耗物品        |                                                                                                                   |
    | Crouch            | 蹲下             |                                                                                                                   |
    | Uncrouch          | 起身             |                                                                                                                   |
    | Damaged           | 受伤             | 攻击者生物类型或伤害类型                                                                                                      |
    | Death             | 死亡             |                                                                                                                   |
    | Interact          | 与实体交互          | 目标实体类型                                                                                                            |
    | Kill              | 杀死生物           | 目标生物类型                                                                                                            |
    | KillPlayer        | 杀死玩家           |                                                                                                                   |
    | Spawn             | 玩家进入游戏或复活      |                                                                                                                   |
    | SplashPotion      | 投扔喷溅或滞留药水      |                                                                                                                   |
    | Swing             | 左键             |                                                                                                                   |
    | Use               | 右键使用           |                                                                                                                   |
    | RightClick        | 右键空地           |                                                                                                                   |
    | Timer             | 定时技能           | 每多少Ticks触发一次                                                                                                      |
    | Fish              | 一切钓鱼事件         | 钓到东西的种类（如果有的话）                                                                                                    |
    | FishBite          | 鱼或者实体咬钩        | 钓到东西的种类（如果有的话）                                                                                                    |
    | FishCatchFish     | 钓起鱼            | 钓到东西的种类（如果有的话）                                                                                                    |
    | FishCatchEntity   | 钓起实体           | 钓到东西的种类（如果有的话）                                                                                                    |
    | FishGround        | 钓钩命中方块         |                                                                                                                   |
    | FishReel          | 收回空钩           |                                                                                                                   |
    | FishFail          | 有鱼咬钩但没钓到       |                                                                                                                   |
    | ArmorEquip        | 穿上装备           |                                                                                                                   |
    | ArmorUnequip      | 脱下装备           |                                                                                                                   |

## **内置技能**

* **注意1：内置技能必须写在宝石技能库中**
* **注意2：MM目标选择器对内置技能无效，内置技能有它自己的目标**

1.  **宝石奖励**

    套娃一切宝石奖励

    ```
     - Reward{宝石奖励}
    ```

    * 宝石奖励与前面的写法完全一致
    * 物品奖励会打在发动技能的装备上
    * 玩家奖励会施加给发动技能的玩家
2.  **切换技能**

    为同一个装备切换不同技能

    ```
     - Switch{技能1;技能2;技能3}
    ```

    * 只会对触发技能的装备生效
    * 每触发一次，就会切换到下一个技能，原先的技能失效。循环往复。
    * 只能切换NBT识别的（用宝石SkillAdd打上去的）技能，不能切换Lore识别的技能
3.  **可切换奖励**

    配合切换技能使用，切换到这个技能组时执行奖励，切走的时候执行拆卸

    ```
     - RewardSwitch{宝石奖励}
    ```

    * 只会对触发技能的装备生效
    * 例如 - RewardSwitch{Enchant{name=DAMAGE\_ALL;level=10}}
    * 则用 - Switch切换到这个技能组时，武器锋利+10；切出时武器锋利-10
4.  **药水技能**

    给玩家持续性的原版药水Buff

    ```
     - PotionBuff{type=药水名;level=等级表达式;duration=持续时间秒数表达式;refresh=刷新时间秒数;roman=罗马数字识别} $世界名1:上限1 $世界名2:上限2 @目标
    ```

    * type有别名potion，p，name
    * level有别名l，duration有别名d
    * roman为true时识别罗马数字
    * 药水BUFF只能有两种目标：@self 和 @target
    * 触发器推荐用onTimer
    * 相比用MM制作药水BUFF，使用这个技能有如下的显著优势：
      1. 可以根据装备上的Lore动态读取药水buff等级，无需每种等级写一个技能
      2. 可以设置刷新时间，如果剩余的时间高于这个时间就不刷新
         * 避免反复的伤害吸收、频繁的生命提升
      3. 可以正确处理覆盖，低等级BUFF不会覆盖高等级药水
      4. 可以设置世界药水限制，在指定的世界药水等级自动被压制到某一等级
      5. 异步生效，不卡服
5.  **闪现技能**

    顾名思义，瞬移到目标地点

    ```
     - Blink{distance=距离上限表达式} @目标
    ```

    *   目标有九种可选

        | **@entity**   | **闪现到命中的实体** | **@down**  | **向下闪现** |
        | ------------- | ------------ | ---------- | -------- |
        | **@location** | **闪现到目标地点**  | **@left**  | **向左闪现** |
        | **@foward**   | **向前闪现**     | **@right** | **向右闪现** |
        | **@backward** | **向后闪现**     | **@bed**   | **向床闪现** |
        | **@up**       | **向上闪现**     |            |          |
    * 其中location和forward最常用
    * 相比MM技能，本技能可以**一行写闪现**
6. **持续时间技能**
   * **当且仅当技能指定了Duration时有效**
   * 在未开始计时的时候，整个技能都不可使用
   *   你必须写一个开始计时技能才能启动整个技能

       ```
        - TimeStart{tip=技能开始的提示}
       ```

       * 技能开始提示也支持ActionBar 、Chat、Title，格式和宝石奖励的一样
       * 持续时间技能一旦开始，技能组里的其它技能都变得可用且没有冷却
       * 技能一旦开始，持续时间内不能再次开始
   *   你可以再写一个技能来**增加持续时间**

       ```
        - TimeAdd{time=要增加的秒数表达式;tip=提示信息}
       ```

       * 从而做到杀人杀怪增加持续时间
   * 持续时间结束后技能继续冷却
   * 注意，**持续时间内技能的冷却也在计算**，因此加的太多，技能就没有冷却了
   * 相比用MM计分板，本技能的好处在于可以**方便地增加时长**
7.  **DEBUFF技能**

    给目标玩家或者生物加上Debuff

    ```
     - Debuff{type=类型;duration=持续时间秒数表达式} @目标
    ```

    * 目标只能是@self和@target
    *   type类型有十一种可以选择

        | 名称      | 作用        | 名称         | 作用       |
        | ------- | --------- | ---------- | -------- |
        | USE     | 禁止使用物品    | ATTACK     | 禁止造成伤害   |
        | SHOOT   | 禁止射箭      | JUMP       | 禁止跳跃     |
        | FLY     | 禁飞        | MOVE       | 禁止移动     |
        | BREAK   | 禁止破坏方块    | PLACE      | 禁止放置     |
        | HEAL    | 禁止生命恢复    | TELEPORT   | 禁止传送     |
        | SKILL   | 禁止使用宝石技能  | POTIONBUFF | 禁止药水效果刷新 |
        | CONSUME | 禁止食用或消耗物品 |            |          |
    * 所有Debuff对玩家有效
    * Attack，Shoot，Heal，Teleport对生物有效
    * Paper系列端1.16.以上，Jump对生物也有效
8.  **吸血技能**

    百分比吸血

    ```
     - BloodSuck{factor=吸血系数表达式}
    ```

    * factor有别名f，k
    * 必须是有伤害的事件才能附带吸血，因此请使用\~onAttack之类的触发器
    * 吸血系数表达式计算出的系数会被直接乘到伤害上。
      * 如 BlookSuck{k=0.1} \~onAttack 代表一切攻击附带10%吸血
    * 相比MM吸血技能，**本技能不会给玩家额外的MCMMO经验**
9.  **计数技能**

    记录触发器触发的次数，次数满足一定条件则触发技能

    ```
     - Counter{condition=逻辑公式;dynamic=变量表达式;persist=是否永久计数;tag=计数器名称;allslot=是否触发全身格子的计数技能}
    ```

    * condition有别名c，dynamic有别名d，persist有别名p
    * 逻辑公式是一个为true或为false的表达式，**其中必须有变量v**，代表触发的次数
      * 如 v%3==0 表示判断v对3取余数，余数为0
    * 逻辑公式里面可以使用变量d，d是物品变量公式，由dynamic指定
      * 如condition=v%(10-d)==0,dynaminc=$LORE:技能等级?0$
      * 意思是，武器上的技能等级为0时，每10次触发一次；为1时，每9次触发一次...
    * persist为true或false，默认为false
      * 为true时，计数会以NBT形式保存在武器上
      * 为false时，计数会存在内存里，服务器重启刷新
    * tag为计数器名。配合触发器\~onCounter使用
    * allslot为true或false，默认为false
      * 为true时，会触发全身的onCounter技能
      * 为false时，只会触发当前装备的onCounter技能
    *   示例：每五下暴击一下

        ```
        Skills:
         - Counter{c=v%5==0;tag=暴击计数} ~onAttack
         - Damage{amount=10} @targetentity ~onCounter:暴击计数
        ```
    * 相比MM计分板，使用本技能的显著优势是**节省工作量**
10. **取消事件**

    取消触发技能的事件

    ```
     - CancelEvent
    ```

    * 本技能与MM相应技能的作用完全相同，**但兼容性更好**
    * 配合复合触发信息，你可以取消更细致的事件。比如
      * 使玩家免疫掉落伤害

    ```
     - CancelEvent ~onDamagedBlock:FALL
    ```

    * 请注意，在Skills中，取消事件技能的前面
      * 不能存在异步技能（需专门指定技能的sync=true）
      * 不能存在延时技能
    * 上述两种情况都会使得取消事件失效

## **MythicMobs兼容**

**技能兼容**

* 宝石技能完全兼容MythicMobs技能，
* 宝石技能库中的技能可以指向任意MythicMobs技能。
* MythicMobs、MythicArtifacts、MythicCrucible也可以用宝石技能库中的技能，**但是宝石特色功能将失效**
  * 宝石内置技能，冷却时间显示，持续时间

**掉落物**

* 可以将宝石作为MythicMobs掉落物
*   **只需在怪物配置中加入**

    ```
    Drops:
     - Gem{name=宝石名称}
    ```
* 其它写法与MythicMobs掉落物一致

## MythicMobs附加

宝石插件提供了以下MM附加条件和技能

### **附加条件**

1.  NoArmorStand

    阻止技能对盔甲架触发。用法：在技能中加上

    ```
     TargetConditions:
       - NoArmorStand
    ```
2.  PvPManager

    只选取开启了PvP的玩家。需要PvPManager支持。建议同时筛选发动者和目标

    ```
    TargetConditions:
       - PvPManager
    Conditions:
       - PvPManager
    ```
3.  ResidenceFlag

    判断领地条件。需要Residence支持

    ```
    Conditions:
      - RedidenseFlag{f=要判断的标志}
    ```

### **附加技能**

1.  SheildDamage

    对护盾（黄心）造成伤害，但不影响玩家真实血条

    ```
     - ShieldDamage{d=伤害量}
    ```

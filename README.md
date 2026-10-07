# gorokid
# 墨局 · AR 桌游原型 v0.3

两名玩家在同一台 PC 轮流操作。新版包含原创水墨战场、手牌悬停与飞牌、攻击轨迹、伤害浮字、平滑血条及残影、受击闪烁/震动与合成音效。目前是屏幕桌游展示，未接入真实 AR、Rokid、AI 或联网。

## 导入与运行

1. 将本包 Assets/Scripts/ARCardPrototype 复制到 Unity 项目的 Assets/Scripts 下，保留 Resources。ZIP 是源码包，不是 unitypackage。
2. 新建普通 3D 场景，保留 Main Camera 和唯一的 AudioListener。
3. 创建空对象 ARCardGame，添加 GameBootstrap。会自动添加 PCInput、TabletopView、TabletopPresenter、DemoDirector、SynthCombatAudio 和 AudioSource。
4. 点击 Play，Game 窗口推荐 1600×900。点击右上角「一键演示」观看杀→闪、杀→受伤、桃→回血；结束后可直接手动操作。

GameManager、Player、TurnManager、Card 是纯 C# 类，无需挂载。不要额外挂载旧版 SimpleGameUI / CombatPresenter。

目标使用 Unity 2022.3 / 2023.x 基础 API；实际验证版本为 Unity 6000.3.18f1，旧版编辑器尚未实测。默认界面使用 IMGUI，无需 TMP。中文字体来自操作系统，包内不分发字体；其他平台缺字时需配置合法中文字体。

## 操作与测试

- 点击手牌→点击角色→确认。键盘 1–9 选牌，Q/W 选择左/右角色，Enter 确认，Esc 取消。
- 选择杀攻击对方，动画后由防守者选闪并确认，或按 S /「承受伤害」。观察 -1 和 HP 从 4/4 平滑到 3/4。
- 按 E /「结束回合」切到受伤玩家，使用桃恢复。满血不能消耗桃。
- 每回合最多一张杀，闪只可响应，桃只治疗自己。结束回合自动弃牌到当前 HP 张，下一人摸两张。
- 初始 4 HP、手牌杀/闪/桃/杀，先手多摸两张。默认种子 303，重开可复现。
- HP 为零立即结束。未实现濒死、装备、距离、武将技能和完整三国杀规则。
- 动画和自动演示期间屏蔽手动命令，右上角可静音、停止演示和重开。

默认旧输入系统可用；启用新版 Input System 时使用新版分支，Both 时只读取新版。

## 最终目录

Assets/Scripts/ARCardPrototype 下：

- GameLogic：GameManager、Player、TurnManager、TargetingRules；Cards 下 Card、AttackCard、DodgeCard、PeachCard。独立程序集，无 Unity / Rokid 引用。
- CombatEvents：CombatEvent、CombatEventStream；不可变事件，独立程序集。
- Input：IGameInput、GameInputAdapter、PCInput、RokidInput。后者只有适配接口及 TODO。
- Runtime：GameBootstrap 组装入口。
- Presentation：TabletopPresenter 事件队列和血条插值；TabletopView 和 InkDraw 绘制战场/卡牌/VFX；DemoDirector 自动演示；SynthCombatAudio 合成声音。
- Resources：InkBattlefield.png 原创生成插画；PrototypeUnlit.shader 供旧版世界空间模块使用。

规则提交 HP 并发布 DamageEvent → TabletopPresenter → TabletopView / SynthCombatAudio。表现层不反过来扣血，关闭表现仍可结算。未来 Rokid SDK 回调映射到 IGameInput 的语义命令。

新版 VFX 即时绘制，无需每次生成 GameObject。旧版 CombatPresenter、PlayerView、SmoothHealthBar、HitFeedback、AttackVfx、DamagePopup、EffectPool、VisualFactory、CombatAudio、SimpleGameUI 保留为世界空间参考，不默认挂载。

## 随包资料

开源项目分析与架构.md 记录许可证和借鉴范围；完整源码.md 包含所有脚本；ART_PROVENANCE.md 记录图片提示词；THIRD_PARTY_NOTICES.md 保留来源说明；验证结果.md 记录验证范围。Preview 包含实际运行截图，测试构建带 Development Build 标记。


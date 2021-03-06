---
title: 关于故障指标
date: 2020-04-17 22:43:50
tags: 系统设计
---

最近做团队的 Q2 规划，我希望有一个简单明确的指标可以代表目前服务的质量。这篇文章记录下我对故障指标的一些思考。

## 失效的 P1 故障计次指标
过去几个季度，团队一直尝试用 P1 故障次数来做这个指标，但效果并不好。究其原因，主要以下几点：
* P1 故障的定级标准是模糊的，比如「非核心功能重大故障」，多重大才算 P1 呢？以及「核心故障轻微故障」，那么影响 1 个用户算 P1 么？
* 因为上面这个原因，用 P1 故障计次来评估线上质量是不可靠的。比如一次影响 10K 人的故障，显然比 10 次影响 1 人的故障更加严重。
* 系统一定会失效，不可能 100% 不出问题，追求 0 P1 故障不可能。追求 0 故障的成本远远超出团队所能实际支付的范围。
* 甚至数学归纳法告诉我们，如果追求 0 P1 故障不可能，那么追求 n 故障也是不可能的。
* P1 故障次数作为团队目标会成为一个负向指标。当在季度开始时如果团队突破了故障指标，那么这一个季度这个指标就变得毫无意义了。

<!-- more -->

究其根本，我认为对于一个高速迭代的商业产品来说，是需要量化团队对于意外、故障的容忍度的。即便大部分人会说 「我们不能容忍系统不稳定」「我们的系统不应该失效」，但实际上，如果我们真正做到了这些，这个系统可能根本没有人用，只有不存在变化的系统，才是 100% 稳定的系统。

## 寻求新的方案
P1 故障计次不能作为故障衡量指标的原因，在于它无法真正的衡量出团队对「不稳定」的容忍度。我想，一个更好的目标，应该有以下特点:
* 能真正衡量故障对于商业价值的影响。
* 是切实可能，通过团队的努力可以达到的。

最终我们选择了「季度内故障人次占季度内完课访问用户人次的比例」作为课程体验的指标。这个指标定义如下:

> 故障率 = 每次 P1 故障影响的用户数 / 季度内每天完课访问的用户人次之和

这个指标可以衡量出每次故障对于我们产品带来的损害。例如，我们认为如果我们的故障影响范围小于 0.01%，那么对于大部分的用户来说，在使用周期内都不会遇到阻塞他使用的故障。这个限度应该是团队可以容忍的最低底线。

它也是切实可行的。可行性可以根据 上一个季度的故障指标分析得出。上个季度我们的故障率为 0.49%（计算公式如上），其中一个系统失效的问题占整个季度访问人次的 0.47%。我认为如果我们这个季度可以做到例行压力测试、例行线上容量评估，完全可以避免同样的大规模系统失效。

## 从用户视角出发
不同的团队，对于故障指标的定义可能完全不同。这取决于团队服务的目标群体与商业价值所在。例如，对于一个内部工程效率来讲，他们的故障指标很可能就是对于开发人员人时的浪费；对于一个电商团队，可能是交易额的损失。

不同的计量方法，会带来不同的思考方式与解决问题的方案，甚至影响系统的设计。一个基于故障人次的指标下，快速测试、快速灰度、快速验证、及时控制影响范围与快速修复问题是会被鼓励的行为，这会带来一个相对宽松、鼓励小步灰度迭代的架构。

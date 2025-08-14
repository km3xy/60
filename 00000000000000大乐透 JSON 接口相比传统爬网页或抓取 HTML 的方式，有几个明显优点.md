✅ 功能说明

1. 前区选号表单

可随机选中 1~8 个号码

可清空、反选遗漏号码

最大可选 30 个

2. AJAX 提交

调用 ajax/ps5.php 生成选号结果

可返回遗漏号码


3. 最新开奖显示

通过 JSON 接口获取最新一期开奖

红球红色圆圈，蓝球蓝色圆圈

4. 完全在一个页面显示

手机浏览器访问 KSWeb 即可使用


大乐透 JSON 接口相比传统爬网页或抓取 HTML 的方式，有几个明显优点：


---

1️⃣ 数据结构化

JSON 是标准化的数据格式，直接包含字段如：

lotteryDrawNum（期号）

lotteryDrawTime（开奖日期）

lotteryDrawResult（开奖号码）


不需要解析 HTML、正则提取，直接 json_decode() 就能得到 PHP 数组或对象。



---

2️⃣ 稳定可靠

网页 HTML 可能随时改版（比如 <div> 类名变化），爬取可能失效。

JSON 接口一般由官方或 API 提供，结构固定，不易随意改动。



---

3️⃣ 访问高效

JSON 数据体积小，只返回必要字段，不像网页有大量样式、脚本和图片。

请求速度快，降低带宽消耗。



---

4️⃣ 易于扩展功能

可以轻松获取多期历史数据，用于：

趋势统计

遗漏号码分析

热号、冷号统计


方便前端用 JS 绘制走势图、柱状图、饼图等。



---

5️⃣ 跨平台友好

不依赖浏览器渲染 HTML，PHP、Python、Node.js 都能直接解析 JSON。

手机端、桌面端、小程序都能复用同一接口。

总之，使用 JSON 接口减少维护成本、提高效率、利于数据分析和可视化，是构建大乐透选号工具的最佳方式。

如果你需要，我可以帮你列一个JSON 接口返回字段说明表，方便你直接用 PHP/JS 调用。
你希望我帮你整理吗？
明白了，你想要一个 PHP 调用大乐透 JSON 接口并解析显示最新开奖的完整示例。下面我给你一个稳定、整洁、可直接用的版本：

<?php
// 配置
$pageSize = 1; // 最新一期
$url = "https://webapi.sporttery.cn/gateway/lottery/getHistoryPageListV1.qry";

// 构建请求参数
$params = http_build_query([
    "gameNo"     => 85, // 大乐透编号
    "provinceId" => 0,
    "pageSize"   => $pageSize,
    "isVerify"   => 1,
    "pageNo"     => 1
]);

// HTTP 请求上下文
$opts = [
    "http" => [
        "method" => "GET",
        "header" => "User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64)\r\n"
    ]
];
$context = stream_context_create($opts);

// 获取 JSON 数据
$json = file_get_contents($url . "?" . $params, false, $context);
$data = json_decode($json, true);

// 解析最新开奖
$latestDraw = !empty($data['value']['list'][0]) ? $data['value']['list'][0] : null;
$latestReds = $latestDraw ? array_slice(explode(" ", $latestDraw['lotteryDrawResult']), 0, 5) : [];
$latestBlues = $latestDraw ? array_slice(explode(" ", $latestDraw['lotteryDrawResult']), 5) : [];
?>

<!DOCTYPE html>
<html lang="zh-CN">
<head>
<meta charset="UTF-8">
<title>大乐透最新开奖</title>
<style>
body { font-family:"微软雅黑",Arial,sans-serif; background:#f8f8f8; padding:20px; }
h1 { text-align:center; color:#333; }
.red-ball, .blue-ball { display:inline-block; width:28px; height:28px; line-height:28px; border-radius:50%; color:#fff; margin:2px; font-weight:bold; text-align:center; }
.red-ball { background:#e74c3c; }
.blue-ball { background:#3498db; }
</style>
</head>
<body>

<h1>大乐透最新开奖</h1>

<?php if($latestDraw): ?>
<p><strong>第<?= $latestDraw['lotteryDrawNum'] ?>期</strong> - <?= $latestDraw['lotteryDrawTime'] ?></p>
<p>
红球：
<?php foreach($latestReds as $r): ?>
<span class="red-ball"><?= $r ?></span>
<?php endforeach; ?>
</p>
<p>
蓝球：
<?php foreach($latestBlues as $b): ?>
<span class="blue-ball"><?= $b ?></span>
<?php endforeach; ?>
</p>
<?php else: ?>
<p style="color:red;">❌ 未能获取开奖数据</p>
<?php endif; ?>

</body>
</html>

✅ 功能说明：

1. 使用 JSON 接口，不依赖网页爬虫，稳定可靠。

2. 显示最新一期的 期号、开奖日期、红球、蓝球。

3. 样式简单，可直接在手机/PC 浏览器打开。

4. 可扩展：增加历史开奖列表、走势图、前区复选框选号等功能。
优点


json接口的优点，无需申请api接口，


不依赖第三方付费 API（直接用中国体彩网官方接口）。

不需要执行 JS，手机 KSWeb 本地也能运行。

支持翻页查询历史数据，不卡死在最新一期。

结构稳定，接口是 JSON，不会因为网页改版就失效。

















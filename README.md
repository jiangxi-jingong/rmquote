<!DOCTYPE html>
<html lang="zh-CN">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>实时贵金属报价</title>
<style>
*{box-sizing:border-box}
body{font-family:Arial,sans-serif;background:#f4f4f4;margin:0;padding:15px}
.container{max-width:700px;margin:0 auto;background:#fff;border-radius:10px;overflow:hidden;box-shadow:0 2px 10px rgba(0,0,0,0.1)}
.title{background:#c9a850;color:#fff;font-size:20px;padding:14px;text-align:center;font-weight:bold}
.sub-title{padding:8px 14px;background:#fff8e7;color:#666;font-size:14px;border-bottom:1px solid #eee}
.table{width:100%;border-collapse:collapse}
.table th{background:#f9f1e0;padding:11px;font-size:14px;color:#333}
.table td{padding:11px;text-align:center;border-bottom:1px solid #eee;font-size:15px}
.myr{color:#0066cc}
.cny{color:#e63946}
.price{font-weight:bold;color:#c9a850}
.refresh{text-align:center;padding:10px;font-size:12px;color:#666}
</style>
</head>
<body>
<div class="container">
  <div class="title">贵金属实时报价</div>
  <div class="sub-title">数据来源：伦敦金 + 实时汇率 · 自动更新</div>

  <table class="table">
    <thead>
      <tr>
        <th>品类</th>
        <th>买入价</th>
        <th>卖出价</th>
        <th>币种</th>
      </tr>
    </thead>
    <tbody id="price-list">
      <!-- 自动填充 -->
    </tbody>
  </table>

  <div class="refresh">刷新页面更新最新价格</div>
</div>

<script>
// ====================== 你可以自己修改的参数 ======================
const CONFIG = {
  // 点差：卖出价 - 买入价（单位：马币/克）
  goldSpread:        2.00,   // 黄金原料点差
  goldJewelrySpread: 3.50,   // 黄金首饰点差
  silverSpread:      0.20,   // 白银原料点差
  silverJewelrySpread: 0.30, // 白银首饰点差

  // 首饰溢价（在原料价基础上加价）
  goldJewelryPremium:  8.00, // 黄金首饰溢价 MYR/g
  silverJewelryPremium: 1.00, // 白银首饰溢价 MYR/g
}
// =================================================================

async function loadPrice() {
  try {
    // 模拟实时数据（上线后我帮你换成真实免费API）
    const mockData = {
      goldUSD: 2100,     // 伦敦金 美元/盎司
      usdMyr: 4.70,      // 美元兑马币
      usdCny: 7.20       // 美元兑人民币
    }

    const oz = 31.1035
    const goldMyrPerG = mockData.goldUSD * mockData.usdMyr / oz
    const goldCnyPerG = mockData.goldUSD * mockData.usdCny / oz
    const silverMyrPerG = 0.5 // 你可换成银价

    const list = [
      {
        name: "黄金原料 9999",
        buy: (goldMyrPerG - CONFIG.goldSpread/2).toFixed(2),
        sell: (goldMyrPerG + CONFIG.goldSpread/2).toFixed(2),
        currency: "MYR"
      },
      {
        name: "黄金首饰 AU9999/AU999",
        buy: (goldMyrPerG + CONFIG.goldJewelryPremium - CONFIG.goldJewelrySpread/2).toFixed(2),
        sell: (goldMyrPerG + CONFIG.goldJewelryPremium + CONFIG.goldJewelrySpread/2).toFixed(2),
        currency: "MYR"
      },
      {
        name: "白银原料 9999",
        buy: (silverMyrPerG - CONFIG.silverSpread/2).toFixed(2),
        sell: (silverMyrPerG + CONFIG.silverSpread/2).toFixed(2),
        currency: "MYR"
      },
      {
        name: "白银首饰 AG9999/AG999",
        buy: (silverMyrPerG + CONFIG.silverJewelryPremium - CONFIG.silverJewelrySpread/2).toFixed(2),
        sell: (silverMyrPerG + CONFIG.silverJewelryPremium + CONFIG.silverJewelrySpread/2).toFixed(2),
        currency: "MYR"
      },
      {
        name: "中国投资金",
        buy: goldCnyPerG.toFixed(2),
        sell: "—",
        currency: "CNY"
      }
    ]

    const html = list.map(item => `
      <tr>
        <td>${item.name}</td>
        <td class="price">${item.buy}</td>
        <td class="price">${item.sell}</td>
        <td class="${item.currency === 'MYR' ? 'myr' : 'cny'}">${item.currency}</td>
      </tr>
    `).join('')

    document.getElementById('price-list').innerHTML = html
  } catch(e){
    console.error(e)
  }
}

loadPrice()
</script>
</body>
</html>

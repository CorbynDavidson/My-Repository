const chart = LightweightCharts.createChart(document.getElementById('chart'), {
  width: 600,
  height: 400,
});
const candleSeries = chart.addCandlestickSeries();

fetch('https://api.binance.com/api/v3/klines?symbol=XRPUSDT&interval=1h&limit=50')
  .then(res => res.json())
  .then(data => {
    const formatted = data.map(k => ({
      time: k[0] / 1000,
      open: parseFloat(k[1]),
      high: parseFloat(k[2]),
      low: parseFloat(k[3]),
      close: parseFloat(k[4])
    }));
    candleSeries.setData(formatted);
});

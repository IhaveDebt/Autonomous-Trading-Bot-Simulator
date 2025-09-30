/**
 * trading_simulator.ts
 *
 * Simulated market ticks and a simple momentum strategy.
 *
 * Run:
 *  ts-node src/trading_simulator.ts
 */
function genTicks(n=200, base=100){ const a=[]; let p=base;
  for(let i=0;i<n;i++){ p += (Math.random()-0.5)*2; a.push({ts:Date.now()+i*1000, price:+p.toFixed(2)}); } return a;
}

function momentumStrategy(ticks:any[], window=5){
  let position=0, pnl=0;
  for(let i=window;i<ticks.length;i++){
    const prev = ticks.slice(i-window,i).reduce((s,t)=>s+t.price,0)/window;
    const price = ticks[i].price;
    if (price > prev && position<=0){ position = 1; console.log('BUY',price); }
    else if (price < prev && position>=0){ position = -1; console.log('SELL',price); }
    // mark-to-market
    pnl += position*(ticks[i].price - ticks[i-1].price);
  }
  return pnl;
}

(function demo(){
  const ticks = genTicks();
  const pnl = momentumStrategy(ticks,10);
  console.log('Simulated PnL:', pnl.toFixed(2));
})();

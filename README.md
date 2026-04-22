import React, { useState } from 'react';
import { Card, CardContent } from '@/components/ui/card';
import { Button } from '@/components/ui/button';

export default function CalculatorApp(){
  const [display,setDisplay]=useState('0');
  const [stored,setStored]=useState(null);
  const [op,setOp]=useState(null);
  const [resetNext,setResetNext]=useState(false);

  const input=(val)=>{
    if(resetNext){ setDisplay(String(val)); setResetNext(false); return; }
    setDisplay(display==='0' ? String(val) : display + val);
  };

  const clearAll=()=>{setDisplay('0');setStored(null);setOp(null);};
  const choose=(nextOp)=>{ setStored(parseFloat(display)); setOp(nextOp); setResetNext(true); };
  const calc=()=>{
    if(stored===null||!op) return;
    const cur=parseFloat(display);
    let r=cur;
    if(op==='+') r=stored+cur;
    if(op==='-') r=stored-cur;
    if(op==='×') r=stored*cur;
    if(op==='÷') r=cur===0 ? 'Error' : stored/cur;
    setDisplay(String(r)); setStored(null); setOp(null); setResetNext(true);
  };

  const keys=['7','8','9','4','5','6','1','2','3','0','.'];
  return (
    <div className='min-h-screen flex items-center justify-center bg-slate-100 p-4'>
      <Card className='w-full max-w-sm rounded-2xl shadow-xl'>
        <CardContent className='p-4 space-y-4'>
          <h1 className='text-2xl font-bold text-center'>Calculadora</h1>
          <div className='bg-black text-white text-right text-4xl rounded-2xl p-4 min-h-20 overflow-hidden'>{display}</div>
          <div className='grid grid-cols-4 gap-2'>
            <Button onClick={clearAll}>C</Button>
            <Button onClick={()=>choose('÷')}>÷</Button>
            <Button onClick={()=>choose('×')}>×</Button>
            <Button onClick={()=>choose('-')}>-</Button>
            {keys.slice(0,9).map(k=><Button key={k} onClick={()=>input(k)}>{k}</Button>)}
            <Button onClick={()=>choose('+')}>+</Button>
            <Button onClick={()=>input('0')} className='col-span-2'>0</Button>
            <Button onClick={()=>input('.')}>.</Button>
            <Button onClick={calc}>=</Button>
          </div>
        </CardContent>
      </Card>
    </div>
  );
}

# lamdba

```javascript
////////////
// lamdba
////////////
// x | t t | λx.t

////////////
// 图灵机
////////////
N  = x=>x?++x:1
P  = x=>console.info('1')

////////////
// 自然数
////////////
ZERO  = f=>x=>x
ONE   = f=>x=>f(x)
TWO   = f=>x=>f(f(x))
THREE = f=>x=>f(f(f(x)))
FOUR  = f=>x=>f(f(f(f(x))))
FIVE  = f=>x=>f(f(f(f(f(x)))))

// 后继
SUCC = (n)=>f=>x=>(f)(n(f)(x))
// 前驱
PRED = (n)=>f=>x=>n(g=>h=>h(g(f)))(u=>x)(u=>u)

/////////////
// 算数运算
/////////////
PLUS = (x,y)=>x(SUCC)(y)
SUB  = (x,y)=>y(PRED)(x)
MULT = (x,y)=>f=>x(y(f))

/////////////
// 逻辑运算
/////////////
TRUE  = x=>y=>x
FALSE = x=>y=>y

AND   = (x,y)=>x(y)(FALSE)
OR    = (x,y)=>x(TRUE)(y)
NOT   = (x)=>x(FALSE)(TRUE)

IS_ZERO = x=>x(FALSE)(NOT)(FALSE)
EQUAL = (x,y)=>IS_ZERO(SUB(x,y))

/////////////
// 数据结构
/////////////
PAIR = (x,y)=>f=>f(x)(y)
FST  = (x)=>x(TRUE)
SED  = (x)=>x(FALSE)

// 后继
NEXT  = (p)=>PAIR(SED(p),SUCC(SED(p)))
// 前驱
PRED  = (x)=>FST(x(NEXT)(PAIR(ZERO,ZERO)))

/////////////
// 分支判断
/////////////
IF  = (z,x,y)=>z(x)(y)

/////////////
// 递归循环
/////////////

WHILE = (n,f)=>n(f)

Y = y=>(x =>y(x(x)))(x=>y(n=>x(x)(n)));

// 阶乘计算
fact1 = f=>n=>n?n*f(n-1):1;
fact2 =f=>n=>IF(EQUAL(n,ONE),ONE,MULT(n,x=>f(PRED(n))(x)));

// 5的阶乘
Y(fact1)(5);
Y(fact2)(FIVE)(N)();



/////////////
// 测试
/////////////
Assert = (x,y)=>{
   // lamdba表达式 转 阿拉伯数字
    var trans=x=>x instanceof Function?( num=x(N)()||0):x;
    x=trans(x);
    y=trans(y);
    var color=(x==y)?'color:blue':'color:red';
    var normal='color:#000';
    console.log('λ: %c%s %cequal %c%s %c= %c%s',color,x,normal,color,y,normal,color,(x==y));
};

console.info('---------自然数---------')
Assert(ZERO,ZERO)
Assert(ONE,ONE)
Assert(TWO,TWO)
Assert(THREE,THREE)

Assert(0,ZERO)
Assert(1,ONE)
Assert(2,TWO)
Assert(3,THREE)

console.info('后继函数')
Assert(1,SUCC(ZERO))
Assert(2,SUCC(ONE))
Assert(3,SUCC(TWO))
Assert(4,SUCC(THREE))

console.info('前继函数')
Assert(0,PRED(ONE))
Assert(1,PRED(TWO))
Assert(2,PRED(THREE))
Assert(3,PRED(FOUR))

console.info('---------算数运算---------')
Assert(3,PLUS(ONE,TWO))
Assert(5,PLUS(THREE,TWO))

Assert(1,SUB(THREE,TWO))
Assert(2,SUB(THREE,ONE))

Assert(2,MULT(ONE,TWO))
Assert(6,MULT(THREE,TWO))

console.info('---------逻辑运算---------')
Assert(1,TRUE(1)(0))
Assert(0,FALSE(1)(0))

Assert(0,NOT(TRUE)(1)(0))
Assert(0,AND(TRUE,FALSE)(1)(0))
Assert(1,AND(TRUE,TRUE)(1)(0))
Assert(1,OR(FALSE,TRUE)(1)(0))
Assert(0,OR(FALSE,FALSE)(1)(0))

console.info('---------数据结构---------')
Assert(0,FST(PAIR(ZERO,ONE)))
Assert(1,SED(PAIR(ZERO,ONE)))

Assert(0,FST(NEXT(PAIR(ZERO,ZERO))))
Assert(1,FST(NEXT(NEXT(PAIR(ZERO,ZERO)))))
Assert(2,FST(NEXT(NEXT(NEXT(PAIR(ZERO,ZERO))))))

Assert(3,PRED(FOUR))
Assert(4,PRED(FIVE))

console.info('---------分支---------')
Assert(1,IF(TRUE,1,0))
Assert(0,IF(FALSE,1,0))

console.info('---------循环递归---------')
Assert(4,WHILE(FOUR,SUCC)(ZERO))
Assert(120,Y(fact1)(5))
Assert(120,Y(fact2)(FIVE))

```

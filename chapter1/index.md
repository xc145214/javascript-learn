#1. ���Ժ���
===
##1.1 �����븳ֵ


```
//����ע��

/**
* ����ע��
*/

//�����Ǳ�ʾֵ��һ�����ţ�ͨ��var �ؼ�������
var x ;   //����һ������X

//ͨ���ȺŸ�ֵ������
x = 0;  //��ֵ
x   //=>0:ͨ������ȡֵ

//Javascript֧�ֶ�����������
x = 1 ;   //����
x = 0.01; //������ʵ������һ����������
x = "hello world!"; //�ַ���
x = 'javascript';   //�ַ���
x = true;       //boolean
x = false ; 
x = null;       //null��һ�������ֵ����
x = underfined; //����null
```

##1.2 ����������
```
//javascript ������Ҫ�������Ƕ��� ��������/ֵ�Եļ��ϻ����ַ���ӳ�䵽ֵ�ü���
var book = {        //���������{}��ʽ
  topic : "javaScript",     //���Ե�ֵ
  fat : true
};

// ͨ��. ���� [] �����ʶ��������
book.topic 
book['fat']
book.author = "Flangan";  //��ֵ����������
book.contents = {};   //{}�Ǹ��ն���

//����������Ϊ�±��������б�
var primes = [2,3,4,5,7];   //��[]��������
primes[0]                   //��һ��Ԫ�أ�����Ϊ0
primes.length               //����ĳ���
primes.[primes.lenth -1]    //���һ��Ԫ��
primes[5] = 9;              //�����Ԫ��
primes[5] = 11;             //��ֵ
var empty = [];             //������
empty.length                // => 0

//����Ͷ�����԰�����һ����������
var points = [
  {x:0,y:0},            //����2�����������
  {x:1,y:1}             //ÿ��Ԫ�ض��Ƕ���
];
var data = {                //�����������ԵĶ���
  trail1 : [[1,2],[3,4]],   //ÿ��Ԫ�ص����Զ�������
  trial2 : [[2,3],[4,5]]    //�����Ԫ��Ҳ������
}
```

##1.3 �����
```
// ����������ڲ�����������һ���µ�ֵ
3 + 2                       // =>5 �ӷ�
3 - 2                       // =>1 ����
3 * 2                       // =>5 �˷�
3 / 2                       // =>1.5 ����
points[1].x - point[0].x    // =>1
"3" +��"2"                  // =>"32",+ ������Ϊ�ӷ��������Ҳ������Ϊ�ַ�������

//һЩ��д�����
var count = 0;              //�������
count ++;                   //����1
count --;                   //�Լ�
count += 2;
count *= 3;
count                       // =>6:������ʡҲ��һ�����ʽ

//��ȹ�ϵ��������ж���ֵ�Ƿ���ȣ�����true����false
var x = 2, y = 3;           
x == y                //false
x != y                //true
x <= y                //true
x < y                 //true
x >= y                //false
x > y                 //false
"two" == "three"      //false
"two" > "three"       //true: "tw"����ĸ��������"th"
false == (x > y)      //true:false��false���

//�߼�������Ƕ�Boolean�ĺϲ�����
(x == 2)&&(y == 3)      //true:��
(x > 3)||(y < 3)        //false�� ��
!(x == 3)               //true:��
```

##1.4 ����
```
//������һ�δ��в����Ĵ���Σ����Ա���ε���
function plus1(x){  //���庯��
  return x + 1;     //����ֵ
}
var y = 3;
plus1(y)        //=>4

var square = function(x) {  //������һ��ֵ�����Ը��Ƹ�����
  return x*x;
};
square(plus(y)) //=> 16
```

���������дʱ�ͱ���˷�����
```
//��������ֵ�����������ʱ����Ϊ���������е�javascript�����з���
var a = [];               //��������
a.push(1,2,3);            //����ķ���push�����Ԫ��
a.reverse();              //����ķ���reverse:��תԪ��

var points = [
  {x:0,y:0},            //����2�����������
  {x:1,y:1}             //ÿ��Ԫ�ض��Ƕ���
];
//�����Լ��ķ�����this �ؼ���������
points.dist = function(){     //����2����
  var p1 = this[0];           //this���õ�ǰ����
  var p2 = this[1];
  var a = p2.x - p1.x;
  var b = p2.y - p1.y;
  return Math.sqrt(a*a + b*b);
};
points.dist()               //=>1.4142135623730951
```

##1.5 �������
```
//�ж���ѭ��
//����ֵ����
function abs(x){
  if(x >=0 ){
    return x;
  }else{
    return -x;
  }
}

//�׳˺���
function factorial(n){
  var product = 1;
  while(n > 1){
    product *= n;
    n--;
  }
  return product;
}
factorial(4)        //=>24 : 1*4*3*2

function factorial2(n){
  var i ,product = 1;
  for (i = 2;i <= n; i ++){
    product *= i;
  }
  return product;
}
fatorial2(5)      //=>120:1*2*3*4*5
```

##1.6 �������
```
//����һ�����캯���Գ�ʼ��һ���µĶ���
function Point(x,y){        //���չ��������캯���Դ�д��ĸ��ͷ
  this.x = x;               //thisָ����ʼ����ʵ�����������Ĳ����洢Ϊ���������
  this.y = y;               //����Ҫreturn
}

//ʹ��new ����ʵ��
var p = new Point(1,1);

//ͨ�����캯����prototype����ֵ����Point�����巽��
Point.prototype.r = function(){
  return Math.sqrt(this.x*this.x + this.y*this.y);
}

//Point��ʵ������̳з���r()
p.r()           //=>1.4142135623730951
```


<html lang="en">
<head>
  <meta charset="UTF-8">
  <title> iphone calculator</title>
  <style>
  *{
    box-sizing: border-box;
    margin:0;
    border:0;
    user-select:none;
  }
   
   body{
     margin:25px;
     font-family:arial;
     
   }
   
   .calculator{
     background:black;
     width:563px;
     height:1218px;
     border-radius:50px;
     padding:20px;
     color:white;
     position:relative;
   }
   
   .top-container{
     
     padding: 0 20px;
     display:flex;
     justify-content:space-between;
     height:250px;
     
   }
   
   .value{
     font-size:130px;
     font-weight:300;
    margin-bottom:20px;
    height:158px;
    text-align:right;
    margin-right:20px;
    font-family: Calibri;
    overflow-y: auto;
    
   }
   
   .buttons-container{
     display:grid;
     grid-template-rows:repeat(5,1fr);
      grid-template-columns:repeat(4,1fr);
      grid-gap:20px;
   }
   
   .button{
     height:110px;
     width:110px;
     font-size: 45px;
     border-radius: 50%;
     background:#333;
     display:flex;
     justify-content:center;
     align-items:center;
     cursor:pointer;
     transition: filter .3s;
   }
   
   .button.function{
     color:black;
     background:#a5a5a5
   }
   
   .button.operator{
     background:#f1a33c
   }
   
   .button.number-0{
     
     width:250px;
     border-radius: 55px;
     justify-content:flex-start;
     padding-left:43px;
     grid-column:1/span 2;
    
   }
   .button:active,
   .buton:focus{
     filter: brightness(120%);
   }
   
   .bottom{
     width:200px;
     height:5px;
     background:white;
     border-radius:4px;
     position:absolute;
     bottom:10px;
     left:50%;
   transform: translateX(-50%);
     
   }
   
  </style>
</head>
<body>
<div class="calculator">
  <div class="top-container">
    <div class="clock">
    <span class="hour"></span>:<span class="minute"></span>
    </div>
    <div class="status">
      <img src=" https://raw.githubusercontent.com/angle943/iphone-calculator-js/master/status.png" alt="status">
    </div>
  </div>
  <div class="value">0</div>
  <div class="buttons-container">
  <div class="button function ac">AC</div>
  <div class="button function pm">±</div>
  <div class="button function percent">%</div>
  <div class="button operator division">÷</div>
  <div class="button number-7">7</div>
  <div class="button number-8">8</div>
  <div class="button number-9">9</div>
  <div class="button operator multipulcation">×</div>    
  <div class="button number-4">4</div>
  <div class="button number-5">5</div>
  <div class="button number-6">6</div>
  <div class="button operator subtraction">−</div>  
  <div class="button number-1">1</div>
  <div class="button number-2">2</div>
  <div class="button number-3">3</div>
  <div class="button operator addition">+</div>
  <div class="button number-0">0</div>
  <div class="button decimal">.</div>
  <div class="button operator equal">=</div>
            
  </div>
  <div class="bottom"></div>
</div>
  <script>
  // dom elements
  const hourEl = document.querySelector('.hour');
  const minuteEl = document.querySelector('.minute');  
  const valueEl = document.querySelector('.value');
  
  const acEl = document.querySelector('.ac');
  const pmEl = document.querySelector('.pm');
  const percentEl = document.querySelector('.percent'); 
  
  const additionEl = document.querySelector('.addition');
  const subtractionEl = document.querySelector('.subtraction');
  const multipulcationEl = document.querySelector('.multipulcation');
  const divisionEl = document.querySelector('.division');
  const equalEl = document.querySelector('.equal');
  
  const decimalEl = document.querySelector('.decimal');
  const number0El = document.querySelector('.number-0');
  const number1El = document.querySelector('.number-1');
  const number2El = document.querySelector('.number-2');
  const number3El = document.querySelector('.number-3');
  const number4El = document.querySelector('.number-4');
  const number5El = document.querySelector('.number-5');
  const number6El = document.querySelector('.number-6');
  const number7El = document.querySelector('.number-7');
  const number8El = document.querySelector('.number-8');
  const number9El = document.querySelector('.number-9');
  
  const numberElArray = [
  
  number0El, number1El, number2El, number3El, number4El, number5El, 
  number6El, number7El, number8El, number9El 
  ];
  
  //variables
  let valueStrInMemory = null;
  let operatorInMemory = null;
  
  //functions
  
  const getValueAsStr = () => {
  const currentDisplayStr = valueEl.textContent;
  return currentDisplayStr.split(',').join('');
  
  }
  
  const getValueAsNum = () => {
  return parseFloat(getValueAsStr());
  }
  
  const setStrAsValue = (valueStr) => {
  if(valueStr[valueStr.length - 1] === '.'){
  valueEl.textContent += '.';
  return;
  }
  
  
  const [wholeNumStr, decimalStr] = valueStr.split('.');
  if(decimalStr){
   valueEl.textContent = parseFloat(wholeNumStr).toLocaleString() + '.' + decimalStr;
  }else{
  valueEl.textContent = parseFloat(wholeNumStr).toLocaleString();
  };
  
  
  };
  
  const handleNumberClick = (numStr) => {
  const currentValueStr = getValueAsStr();
  if(currentValueStr === '0'){
  setStrAsValue(numStr);
  }else{
  
  setStrAsValue(currentValueStr + numStr);
  
  }
  };
  
  
  const getResultOfOperationAsStr = () => {
  const currentValueNum = getValueAsNum();
  const valeNumInMemory = parseFloat(valueStrInMemory);
  let newValueNum;
  if (operatorInMemory === 'addition'){
  newValueNum = valueStrInMemory + currentValueNum;
  }else if (operatorInMemory === 'subtraction'){
  newValueNum = valueStrInMemory - currentValueNum;
  }else if (operatorInMemory === 'multipulcation'){
  newValueNum = valueStrInMemory * currentValueNum;
  }else if (operatorInMemory === 'division'){
  newValueNum = valueStrInMemory / currentValueNum;
  }
  
  return newValueNum.toString();
  };
  
  const handleOperatorClick = (operation) => {
  const currentValueStr = getValueAsStr();
  
  if (!valueStrInMemory){
  valueStrInMemory = currentValueStr;
  operatorInMemory = operation;
  setStrAsValue('0');
  return;
  }
  valueStrInMemory = getResultOfOperationAsStr(); 
  operatorInMemory = operation;
  setStrAsValue('0');
  };
  
  
  // add event listeners to functions
  acEl.addEventListener('click',()=>{
  setStrAsValue('0');
  valueStrMemory = null;
  operatorInMemory = null;
  });
  
  pmEl.addEventListener('click', () =>{
  const currentValueNum = getValueAsNum();
  const currentValueStr = getValueAsStr();
  
  if (currentValueStr === '-0'){
  setStrAsValue('0');
  return;
  }
  
  
  if (currentValueNum >= 0){
  setStrAsValue('-' + currentValueStr);
  }else{
  setStrAsValue(currentValueStr.substring(1));
  }

});
  
  percentEl.addEventListener('click',() => {
  const currentValueNum = getValueAsNum();
  const newValueNum = currentValueNum / 100;
  setStrAsValue(newValueNum.toString());
  valueStrInMemory = null;
  operatorInMemory = null;
  });
  
  
 //add event Listeners to operaters
 additionEl.addEventListener('click', () => {
  handleOperatorClick('addition');
});
subtractionEl.addEventListener('click', () => {
  handleOperatorClick('subtraction');
});
multipulcationEl.addEventListener('click', () => {
  handleOperatorClick('multipulcation');
});
divisionEl.addEventListener('click', () => {
  handleOperatorClick('division');
});
  
  equalEl.addEventListener('click', () =>{
  if(valueStrInMemory){
  setStrAsValue(getResultOfOperationAsStr());
  valueStrInMemory = null;
  operatorInMemory = null;
  }
  });
 
   
  
  
  
  // add event listeners to numbers abd decimal
  for (let i=0; i < numberElArray.length; i++){
  const numberEl = numberElArray[i];
  numberEl.addEventListener('click',() => {
  
  handleNumberClick(i.toString());
  
  });
  
  }
  decimalEl.addEventListener('click',() => {
  const currentValueStr = getValueAsStr();
  
  if(!currentValueStr.includes('.')){
  setStrAsValue(currentValueStr + '.');
  
  }
  });
  
  
  
  
  
  // set up the time
  const updateTime = () => {
  
  const currentTime = new Date();
  
 let  currentHour = currentTime.getHours();
  const currentMinute = currentTime.getMinutes();
  
  if ( currentHour > 12){
  
  currentHour-=12;
  };
  
  
  
  hourEl.textContent = currentHour.toString();
  minuteEl.textContent = currentMinute.toString().padStart(2, '0');
  }
  
  
  
  setInterval(updateTime, 1000);
  updateTime();
  </script>
</body>
</html>

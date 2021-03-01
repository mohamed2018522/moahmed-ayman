<html>
<style>
*{
padding: 0;
margin: 0;
font-family: 'Noto sans';
outline: none;
box-sizing: border-box;
user-select: none;
}
html{
font-size: 25px;
}
body{
background-color: #0093d4;
}
.PlayTypeWin, .WinnerWin{
background-color: #FFF;
}
.center{
position: absolute;
top: 50%;
transform: translate(-50%, -50%);
transition: left 0.9s;
padding: 10px;
left: -100%;
border-radius: 5px;
max-width: 360px;
width: 92%;
}
header{
padding: 5px;
background-color: #FFF;
border-radius: 5px;
}
.players{
position: relative;
display: flex;
}
.player{
flex-basis: 50%;
text-align: center;
z-index: 1;
font-size: 90%;
transition: color 0.5s;
color: #0093d4;
padding: 15px 0;
text-transform: capitalize;
}
.currentPlayer{
color: #FFF;
}
.current{
position: absolute;
top: 0;
height: 100%;
width: 50%;
transition: left 0.5s;
background-color: #0093d4;
border-radius: 5px;
}
.playBtns{
margin-top: 25px;
width: 100%;
}
.playBtns .container{
display: flex;
flex-direction: column;
height: 100%;
}
.row{
display: flex;
flex-basis: 100%;
margin-bottom: 2%;
}
.select{
margin-right: 2%;
flex-basis: 100%;
transition: transform 0.2s;
color: #0093d4;
background-color: #FFF;
border-radius: 5px;
position: relative;
}
.select:last-child, .row:last-child{
margin: 0;
}
.select p{
font-size: 250%;
position: absolute;
top: 50%;
left: 50%;
transform: translate(-50%, -50%);
}
.text{
margin-bottom: 20px;
font-size: 100%;
}
.text h1{
font-size: 90%;
font-weight: 400;
text-transform: capitalize;
color: #0093d4;
margin-bottom: 10px;
border-bottom: 1px solid #0093d4;
padding-bottom: 5px;
word-spacing: -4;
}
.text p{
color: #555;
font-size: 70%;
line-height: 1.5;
padding: 0 10px;
text-transform: capitalize;
}
.text h1 span{
font-size: 70%;
letter-spacing: 0.5px;
word-spacing: -1px;
text-transform: lowercase;
}
.buttons{
justify-content: center;
max-width: 350px;
margin: 0 auto;
display: flex;
border-radius: 5px;
}
.buttons button:last-child{
margin-right: 0;
}
.buttons button{
flex-basis: 100%;
margin-right: 3%;
font-size: 70%;
color: #FFF;
border: none;
padding: 15px 0;
transition: transform 0.2s;
background-color: #0093d4;
border-radius: 5px;
}
.winBtn{
transition: background-color 0.5s;
background-color: #005d86;
}
.buttons button:active,
.select:active{
transform: scale(0.91);
}
</style>
<body>
<section class="center PlayWin">
<header>
<div class="players">
<div class="player X">
<label>X&#39;s turn</label>
</div>
<div class="player O">
<label>O&#39;s turn</label>
</div>
<div class="current"></div>
</div>
</header>
<section class="playBtns">
<div class="container">
<div class="row">
<span class="select" id="select1"></span>
<span class="select" id="select2"></span>
<span class="select" id="select3"></span>
</div>
<div class="row">
<span class="select" id="select4"></span>
<span class="select" id="select5"></span>
<span class="select" id="select6"></span>
</div>
<div class="row">
<span class="select" id="select7"></span>
<span class="select" id="select8"></span>
<span class="select" id="select9"></span>
</div>
</div>
</section>
</section>
<section class="center PlayTypeWin">
<div class="text">
<h1>tic&#45;tac&#45;toe / <span>number of players</span></h1>
<p>choice the number of players you want...</p>
</div>
<div class="buttons">
<button value="single">one player</button>
<button value="double">two players</button>
</div>
</section>
<section class="center WinnerWin">
<div class="text">
<h1>tic&#45;tac&#45;toe / <span>Winner</span></h1>
<p>___</p>
</div>
<div class="buttons">
<button>replay</button>
</div>
</section>
<script>
let play_typeWin, selects, playWin, cur, play_types, xlab, olab, winnerWin, winTitle, currentPlayer, players, plays, play_type;
play_typeWin = document.querySelector('.PlayTypeWin');
playWin = document.querySelector('.PlayWin');
winnerWin = document.querySelector('.WinnerWin');
selects = document.querySelectorAll('.select');
cur = document.querySelector('.current');
xlab = document.querySelector('.X');
olab = document.querySelector('.O');
play_types = document.querySelectorAll('.PlayTypeWin .buttons button');
winTitle = document.querySelector('.WinnerWin .text p');
players = {X : ['0', xlab, []], O : ['50%', olab, []] };
currentPlayer = 'X';
document.querySelector('.WinnerWin .buttons button').addEventListener('click', function(){init();show(winnerWin, play_typeWin);});
var show = function(currentWin, newWin){
if(currentWin !== undefined){
currentWin.style.left = '-100%';
}
newWin.style.left = '50%';
}
show(undefined, play_typeWin);
play_types.forEach(el => el.addEventListener('click', function(){
show(play_typeWin,playWin);
[xlab, olab].forEach(el => el.classList.remove('currentPlayer'));
changePlayer(currentPlayer);
play_type = el.value;
init();
}));
var init = function(){
selects.forEach(el => el.innerHTML = '');
players.X[2] = [];
players.O[2] = [];
plays = [1,2,3,4,5,6,7,8,9];
robotPlay();
selects.forEach(el => {
el.addEventListener('click', gamePlay);
el.classList.remove('winBtn');
});
}
var robotPlay = function(){
setTimeout(() => {
if(currentPlayer === 'O' && play_type === 'single' && plays.length !== 0){
var choice = 'select' + plays[Math.floor(Math.random() * plays.length)];
play(choice);
}
}, 500);
}
var iswinner = function(cur){
const solves = [[1,2,3],[4,5,6],[7,8,9] , [1,4,7],[2,5,8],[3,6,9] , [1,5,9], [3,5,7]];
let iswin;
for(const x of solves){
iswin = true;
for(const y of x){
iswin = players[cur][2].includes(y);
if(iswin === false){break;}
else{continue;}
}
if(iswin){
for(const btn of x){
document.querySelector('#select' + btn).classList.add('winBtn');
}
selects.forEach(el => {
el.removeEventListener('click', gamePlay);
});
winner(`The Player ${cur}'s win !`);
break;
}
}
if(iswin === false){
if(plays.length === 0){
selects.forEach(el => el.classList.add('winBtn'));
winner('the game ended with draw...');
}
robotPlay();
}
}
var winner = function(winner){
winTitle.textContent = winner;
setTimeout(() => {
show(playWin, winnerWin);
}, 1000);
}
var gamePlay = function(){
if(currentPlayer === 'X' && play_type === 'single' || play_type === 'double'){
play(this.id);
}
}
var play = function(item){
const btn = document.querySelector('#' + item);
if(btn.innerHTML === ''){
plays.splice(plays.indexOf(Number(item[item.length - 1])), 1)
btn.innerHTML = '<p>' + currentPlayer + '</p>';
players[currentPlayer][2].push(Number(item[item.length - 1]));
iswinner(currentPlayer);
changePlayer(currentPlayer);
currentPlayer = currentPlayer === 'X' ? 'O' : 'X';
changePlayer(currentPlayer);
}
}
var changePlayer = function(player){
players[player][1].classList.toggle('currentPlayer');
cur.style.left = players[currentPlayer][0];
}
var resize = function(){
var btns = document.querySelector('.PlayBtns');
btns.style.height = btns.clientWidth;
return resize;
};
window.onresize = resize();
</script>
</body>
</html>

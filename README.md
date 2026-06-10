# Maths
A game of obstacles 
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Math Runner</title>
<style>
body{
    margin:0;
    background:#222;
    overflow:hidden;
    font-family:Arial,sans-serif;
}
#game{
    width:100vw;
    height:100vh;
    position:relative;
}
#player{
    width:60px;
    height:60px;
    background:lime;
    position:absolute;
    bottom:50px;
    left:100px;
}
.door{
    width:100px;
    height:140px;
    background:white;
    border:3px solid black;
    position:absolute;
    bottom:50px;
    text-align:center;
    font-size:30px;
    font-weight:bold;
    line-height:140px;
}
#question{
    position:absolute;
    top:20px;
    left:50%;
    transform:translateX(-50%);
    color:white;
    font-size:40px;
}
#score{
    position:absolute;
    top:20px;
    right:20px;
    color:white;
    font-size:30px;
}
</style>
</head>
<body>

<div id="game">
    <div id="question"></div>
    <div id="score">Score: 0</div>
    <div id="player"></div>
</div>

<script>
const game=document.getElementById("game");
const player=document.getElementById("player");
const questionBox=document.getElementById("question");
const scoreBox=document.getElementById("score");

let lane=1;
let score=0;
let speed=6;

const laneX=[100,250,400];

function setLane(){
    player.style.left=laneX[lane]+"px";
}

document.addEventListener("keydown",e=>{
    if(e.key==="ArrowLeft" && lane>0){
        lane--;
        setLane();
    }
    if(e.key==="ArrowRight" && lane<2){
        lane++;
        setLane();
    }
});

let doors=[];
let correctLane=0;

function createQuestion(){

    doors.forEach(d=>d.remove());
    doors=[];

    const a=Math.floor(Math.random()*10)+1;
    const b=Math.floor(Math.random()*10)+1;
    const answer=a+b;

    questionBox.innerText=`${a} + ${b} = ?`;

    correctLane=Math.floor(Math.random()*3);

    let answers=[];

    for(let i=0;i<3;i++){
        if(i===correctLane){
            answers.push(answer);
        }else{
            let wrong;
            do{
                wrong=answer+Math.floor(Math.random()*8)-4;
            }while(wrong===answer || answers.includes(wrong));
            answers.push(wrong);
        }
    }

    for(let i=0;i<3;i++){
        const door=document.createElement("div");
        door.className="door";
        door.innerText=answers[i];
        door.style.left=(laneX[i])+"px";
        door.style.bottom="50px";
        game.appendChild(door);

        doors.push(door);
    }

    let x=window.innerWidth;

    const move=setInterval(()=>{

        x-=speed;

        doors.forEach(d=>{
            d.style.left=(parseInt(d.style.left)-speed)+"px";
        });

        if(x<160){

            clearInterval(move);

            if(lane===correctLane){
                score++;
                scoreBox.innerText="Score: "+score;
                speed+=0.2;
                createQuestion();
            }else{
                alert("Game Over! Score: "+score);
                location.reload();
            }
        }

    },20);
}

setLane();
createQuestion();
</script>

</body>
</html>

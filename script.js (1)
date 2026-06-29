/*==================================================
        RACHJUMPER // MUSE ARCHIVE
==================================================*/

/*==========================
GLOBAL VARIABLES
==========================*/

let muses = [];
let quotes = [];

let currentFilter = "all";
let currentSort = "alphabetical";
let searchQuery = "";

let favorites =
JSON.parse(localStorage.getItem("favorites")) || [];

const gallery = document.getElementById("gallery");
const search = document.getElementById("search");
const suggestions = document.getElementById("suggestions");
const totalCount = document.getElementById("totalCount");
const favoriteCount = document.getElementById("favoriteCount");

/*==========================
BOOT
==========================*/

/*==========================
BOOT
==========================*/

const bootMessages = [
"RACHJUMPER OS // BOOTING",
"Connecting archive core...",
"Verifying character database...",
"Decrypting muse files...",
"Loading subject profiles...",
"Synchronizing favorites...",
"Database integrity: OK",
"ACCESS GRANTED",
];


async function bootSequence(){

const status = document.getElementById("bootText");
const progress = document.getElementById("loadingFill");
const boot = document.getElementById("bootScreen");


if(!boot) return;


for(let i=0;i<bootMessages.length;i++){

    if(status)
    status.textContent = bootMessages[i];


    if(progress)
    progress.style.width =
    ((i+1)/bootMessages.length)*100 + "%";


    await sleep(500);

}


await sleep(700);


boot.style.opacity="0";


await sleep(900);


boot.remove();


}

/*==========================
UTILITY
==========================*/

function sleep(ms){

return new Promise(resolve=>setTimeout(resolve,ms));

}

function random(array){

return array[Math.floor(Math.random()*array.length)];

}

/*==========================
LOAD DATABASE
==========================*/

async function loadDatabase(){

const museData=await fetch("data/muses.json");
muses=await museData.json();

const quoteData=await fetch("data/quotes.json");
quotes=await quoteData.json();

renderGallery();

updateSidebar();

loadDailyQuote();

}

/*==========================
SEARCH
==========================*/

search.addEventListener("input",()=>{

searchQuery=search.value.toLowerCase();

renderGallery();

updateSuggestions();

});

/*==========================
SUGGESTIONS
==========================*/

function updateSuggestions(){

suggestions.innerHTML="";

if(searchQuery===""){

suggestions.style.display="none";

return;

}

const results=muses.filter(m=>

m.name.toLowerCase().includes(searchQuery)

).slice(0,5);

if(results.length===0){

suggestions.style.display="none";

return;

}

results.forEach(muse=>{

const item=document.createElement("div");

item.className="suggestion";

item.textContent=muse.name;

item.onclick=()=>{

search.value=muse.name;

searchQuery=muse.name.toLowerCase();

suggestions.style.display="none";

renderGallery();

};

suggestions.appendChild(item);

});

suggestions.style.display="block";

}

/*==========================
FILTER BUTTONS
==========================*/

document.querySelectorAll(".filter").forEach(button=>{

button.onclick=()=>{

document.querySelector(".filter.active")?.classList.remove("active");

button.classList.add("active");

currentFilter=button.dataset.filter;

renderGallery();

};

});

/*==========================
SORT
==========================*/

/*==========================
FAVORITES
==========================*/

function toggleFavorite(id){

if(favorites.includes(id)){

favorites=favorites.filter(x=>x!==id);

}else{

favorites.push(id);

}

localStorage.setItem("favorites",JSON.stringify(favorites));

renderGallery();

updateSidebar();

}

/*==========================
RENDER
==========================*/

function renderGallery(){

gallery.innerHTML="";

let list=[...muses];

if(currentFilter!=="all"){

list=list.filter(m=>m.tags.includes(currentFilter));

}

if(searchQuery!==""){

list=list.filter(m=>

m.name.toLowerCase().includes(searchQuery) ||

m.species.toLowerCase().includes(searchQuery) ||

m.tags.some(t=>t.toLowerCase().includes(searchQuery))

);

}

switch(currentSort){

case "alphabetical":

list.sort((a,b)=>a.name.localeCompare(b.name));

break;

case "species":

list.sort((a,b)=>a.species.localeCompare(b.species));

break;

case "favorites":

list.sort((a,b)=>

favorites.includes(b.id)-favorites.includes(a.id)

);

break;

case "random":

list.sort(()=>Math.random()-0.5);

break;

}

list.forEach(createCard);

}

/*==========================
CARD
==========================*/

function createCard(muse){

const card=document.createElement("div");
card.className="museCard";

const img=document.createElement("img");
img.src=muse.gallery[0];

const overlay=document.createElement("div");
overlay.className="overlay";

const title=document.createElement("h2");
title.textContent=muse.name;

const species=document.createElement("p");
species.textContent=muse.species;

overlay.append(title,species);

const heart=document.createElement("button");

heart.className="favorite";

if(favorites.includes(muse.id))
heart.classList.add("active");

heart.innerHTML="♥";

heart.onclick=(e)=>{

e.stopPropagation();

toggleFavorite(muse.id);

};

card.append(img,overlay,heart);

card.onclick=()=>{

openDossier(muse.id);

};

gallery.appendChild(card);

}

/*==========================
SIDEBAR
==========================*/

function updateSidebar(){

totalCount.textContent=muses.length;

favoriteCount.textContent=favorites.length;

}

/*==========================
DAILY LOG
==========================*/

function loadDailyQuote(){

document.getElementById("dailyLog").textContent=

random(quotes);

}

/*==========================
NAVIGATION
==========================*/

document.querySelectorAll(".navButton").forEach(button=>{

button.onclick=()=>{

document.querySelector(".navButton.active")
?.classList.remove("active");

button.classList.add("active");

const page=button.dataset.page;

switch(page){

case "database":

renderGallery();

break;

case "favorites":

showFavorites();

break;

case "random":

showRandomCharacter();

break;

case "about":

showAbout();

break;

}

};

});

/*==========================
MUSIC
==========================*/

const music=document.getElementById("music");
const musicButton=document.getElementById("musicButton");

let playing=false;

musicButton.onclick=()=>{

playing=!playing;

if(playing){

music.play();

musicButton.textContent="❚❚";

}else{

music.pause();

musicButton.textContent="♫";

}

};

/*==========================
KEYBOARD
==========================*/

document.addEventListener("keydown",e=>{

if(e.key==="Escape"){

closeDossier();

}

if(e.ctrlKey && e.key==="k"){

e.preventDefault();

search.focus();

}

});

/*==========================
PLACEHOLDERS
==========================*/

function openDossier(id){

// PART 2

}

function closeDossier(){

document.getElementById("dossierOverlay")
.classList.add("hidden");

}

function showFavorites(){

gallery.innerHTML="";

muses
.filter(m=>favorites.includes(m.id))
.forEach(createCard);

}

function showRandomCharacter(){

const randomMuse=random(muses);

openDossier(randomMuse.id);

}

function showAbout(){

document.getElementById("aboutOverlay")
.classList.remove("hidden");

document
.getElementById("closeAbout")
.onclick = () => {

document
.getElementById("aboutOverlay")
.classList.add("hidden");

};

document
.getElementById("aboutOverlay")
.addEventListener("click", e=>{

if(e.target.id==="aboutOverlay"){

document
.getElementById("aboutOverlay")
.classList.add("hidden");

}

});

}

/*==========================
START
==========================*/

(async()=>{

await bootSequence();

await loadDatabase();

})();

/*==================================================
        PART 2 // CHARACTER DOSSIER
==================================================*/


let currentDossier = null;

let currentImage = 0;


/*==========================
OPEN DOSSIER
==========================*/

function openDossier(id){

const muse = muses.find(m=>m.id===id);

if(!muse) return;


currentDossier = muse;

currentImage = 0;


const overlay=document.getElementById("dossierOverlay");

const content=document.getElementById("dossierContent");


overlay.classList.remove("hidden");


content.innerHTML="";


const wrapper=document.createElement("div");

wrapper.className="dossierWindow";


/*
 IMAGE AREA
*/

const imageArea=document.createElement("div");

imageArea.className="dossierImage";


const image=document.createElement("img");

image.id="dossierImage";

image.src=muse.gallery[currentImage];


const left=document.createElement("button");

left.className="galleryButton";

left.textContent="◀";


const right=document.createElement("button");

right.className="galleryButton";

right.textContent="▶";


left.onclick=()=>changeImage(-1);

right.onclick=()=>changeImage(1);


imageArea.append(
left,
image,
right
);



/*
 INFO AREA
*/

const info=document.createElement("div");

info.className="dossierInfo";


const title=document.createElement("h1");

title.textContent=muse.name;



const species=document.createElement("h3");

species.textContent=muse.species;



const fav=document.createElement("button");

fav.className="dossierFavorite";

fav.textContent=

favorites.includes(muse.id)

?"♥"

:"♡";


fav.onclick=()=>{

toggleFavorite(muse.id);

fav.textContent=

favorites.includes(muse.id)

?"♥"

:"♡";

};



/*
 BASIC DATA
*/

const data=document.createElement("div");

data.className="infoGrid";


const fields=[

["AGE",muse.age],

["HEIGHT",muse.height],

["ROLE",muse.occupation],

["STATUS",muse.status],

["PRONOUNS",muse.pronouns]

];


fields.forEach(field=>{


if(!field[1]) return;


const box=document.createElement("div");

box.className="infoBox";


box.innerHTML=`

<strong>${field[0]}</strong>

<span>${field[1]}</span>

`;


data.appendChild(box);


});



/*
 QUOTE
*/

const quote=document.createElement("div");

quote.className="quoteBox";

quote.textContent=

`"${muse.quote || "No quote recorded."}"`;



/*
 DESCRIPTION
*/

const description=document.createElement("section");

description.innerHTML=`

<h2>DESCRIPTION</h2>

<p>

${muse.description || ""}

</p>

`;



/*
 BACKSTORY
*/

const story=document.createElement("section");

story.innerHTML=`

<h2>BACKSTORY</h2>

<p>

${muse.backstory || "No history available."}

</p>

`;



/*
 TRIVIA
*/

const trivia=document.createElement("section");


trivia.innerHTML=`

<h2>TRIVIA</h2>

</br>

`;



if(muse.trivia){

muse.trivia.forEach(t=>{

const item=document.createElement("p");

item.textContent="• "+t;

trivia.appendChild(item);

});

}



/*
 RELATIONSHIPS
*/

const relationships=document.createElement("section");


relationships.innerHTML=`

<h2>RELATIONSHIPS</h2>

`;


if(muse.relationships?.length){


muse.relationships.forEach(r=>{


const character=muses.find(

x=>x.id===r.id

);



if(!character)return;



const relation=document.createElement("button");

relation.className="relationship";


relation.innerHTML=

`

${character.name}

<span>

${r.type}

</span>

`;


relation.onclick=()=>{

openDossier(character.id);

};



relationships.appendChild(relation);



});


}
else{

relationships.innerHTML+=

"<p>No relationships recorded.</p>";

}





info.append(

title,

species,

fav,

data,

quote,

description,

story,

trivia,

relationships

);



wrapper.append(

imageArea,

info

);



content.appendChild(wrapper);



}



/*==========================
IMAGE CAROUSEL
==========================*/


function changeImage(direction){

if(!currentDossier) return;


const images=currentDossier.gallery;


currentImage += direction;


if(currentImage<0)

currentImage=images.length-1;


if(currentImage>=images.length)

currentImage=0;



document.getElementById("dossierImage").src=

images[currentImage];

}



/*==========================
CLOSE DOSSIER
==========================*/


function closeDossier(){

const overlay=document.getElementById("dossierOverlay");


overlay.classList.add("hidden");


currentDossier=null;


}



/*==========================
OVERLAY CLICK CLOSE
==========================*/


document.getElementById("dossierOverlay")

.addEventListener("click",e=>{


if(e.target.id==="dossierOverlay"){

closeDossier();

}


});



/*==========================
CLOSE BUTTON
==========================*/


document.getElementById("closeDossier")

.onclick=()=>{

closeDossier();

};

/*==================================================
        PART 3 // IMMERSION SYSTEMS
==================================================*/


/*==========================
RANDOM CHARACTER PAGE
==========================*/


let randomCharacter = null;


function showRandomCharacter(){

randomCharacter = random(muses);


document.getElementById("randomImage").src =
randomCharacter.gallery[0];


document.getElementById("randomName").textContent =
randomCharacter.name;


document.getElementById("randomSpecies").textContent =
randomCharacter.species;


document.getElementById("randomDescription").textContent =
randomCharacter.description;


document
.getElementById("randomOverlay")
.classList.remove("hidden");

}



/*==========================
ABOUT PAGE
==========================*/


function showAbout(){

document
.getElementById("aboutOverlay")
.classList.remove("hidden");

}

document
.getElementById("closeAbout")
.onclick = () => {

    document
    .getElementById("aboutOverlay")
    .classList.add("hidden");

};



/*==========================
PAGE CLOSE BUTTONS
==========================*/


document
.querySelectorAll(".closePage")
.forEach(button=>{


button.onclick=()=>{


button.parentElement
.parentElement
.classList.add("hidden");


};


});



/*==========================
PARTICLES
==========================*/


function createParticles(){

const container=
document.getElementById("particles");


if(!container)return;



for(let i=0;i<60;i++){


const particle=
document.createElement("span");


particle.className="particle";


particle.style.left=
Math.random()*100+"%";


particle.style.top=
Math.random()*100+"%";


particle.style.animationDelay=
Math.random()*10+"s";


particle.style.animationDuration=
(5+Math.random()*10)+"s";



container.appendChild(particle);


}

}


createParticles();



/*==========================
CARD 3D TILT
==========================*/


document.addEventListener(
"mousemove",
e=>{


const card=
e.target.closest(".museCard");


if(!card)return;



const rect=
card.getBoundingClientRect();



const x=
e.clientX-rect.left;


const y=
e.clientY-rect.top;



const rotateY=
((x/rect.width)-0.5)*15;


const rotateX=
((y/rect.height)-0.5)*-15;



card.style.transform=

`

perspective(800px)

rotateX(${rotateX}deg)

rotateY(${rotateY}deg)

translateY(-8px)

`;



});



document.addEventListener(
"mouseleave",
e=>{


const card=
e.target.closest(".museCard");


if(card)

card.style.transform="";


},
true);



/*==========================
DOSSIER ANIMATION
==========================*/


const dossierObserver =
new MutationObserver(()=>{


const dossier=
document.querySelector(".dossierWindow");


if(dossier){


dossier.classList.add("opening");


}



});


dossierObserver.observe(

document.body,

{

childList:true,

subtree:true

}

);




/*==========================
SOUND SYSTEM
==========================*/


const sounds={

click:new Audio(
"music/click.mp3"
),

open:new Audio(
"music/open.mp3"
),

close:new Audio(
"music/close.mp3"
)

};



function playSound(name){

if(!sounds[name])
return;


sounds[name].volume=.35;


sounds[name].currentTime=0;


sounds[name].play()
.catch(()=>{});


}



/* buttons */

document.addEventListener(
"click",
e=>{


if(
e.target.tagName==="BUTTON"
){

playSound("click");

}


});



/* dossier sounds */


const oldOpen=openDossier;


openDossier=function(id){


playSound("open");


oldOpen(id);


};



const oldClose=closeDossier;


closeDossier=function(){


playSound("close");


oldClose();


};




/*==========================
TERMINAL MODE
==========================*/


let terminal=false;



document.addEventListener(
"keydown",
e=>{


if(e.key==="`"){


terminal=!terminal;


toggleTerminal();


}



});



function toggleTerminal(){


let terminalBox=
document.getElementById(
"terminal"
);



if(!terminalBox){


terminalBox=
document.createElement("div");


terminalBox.id="terminal";


terminalBox.innerHTML=`

<div>

<h2>RACH OS TERMINAL</h2>

<p id="terminalText"></p>

<input id="terminalInput">

</div>

`;



document.body.appendChild(
terminalBox
);


}



terminalBox.style.display=
terminal?"flex":"none";



if(terminal){


document
.getElementById("terminalInput")
.focus();


}


}



document.addEventListener(
"keydown",
e=>{


if(
e.key==="Enter" &&
terminal
){


executeCommand(
document.getElementById(
"terminalInput"
).value
);


}

});



function executeCommand(command){


const output=
document.getElementById(
"terminalText"
);



switch(command.toLowerCase()){


case "help":

output.textContent=

"commands: help, count, random, clear";

break;



case "count":

output.textContent=

`DATABASE: ${muses.length} FILES`;

break;



case "random":

showRandomCharacter();

break;



case "clear":

output.textContent="";

break;



default:

output.textContent=

"UNKNOWN COMMAND";

}



}



/*==========================
INIT EXTRA SYSTEMS
==========================*/


window.addEventListener(
"load",
()=>{


createParticles();


});

/*==================================================
        PART 4 // FINAL POLISH
==================================================*/


/*==========================
DATABASE ERROR HANDLING
==========================*/


async function safeLoad(url){

try{

const response=await fetch(url);


if(!response.ok){

throw new Error(
"FILE NOT FOUND"
);

}


return await response.json();


}

catch(error){

console.error(error);


return [];

}

}




/*
 Replace your old loadDatabase()
 with this version
*/


async function loadDatabase(){

muses=
await safeLoad(
"data/muses.json"
);


quotes=
await safeLoad(
"data/quotes.json"
);



if(muses.length===0){

console.warn(
"No characters found."
);

}



renderGallery();

updateSidebar();

loadDailyQuote();

}



/*==========================
IMAGE FALLBACK
==========================*/


document.addEventListener(
"error",
e=>{


if(e.target.tagName==="IMG"){

e.target.src=
"images/placeholder.png";

}


},
true
);



/*==========================
SAVE SETTINGS
==========================*/


const settings =
JSON.parse(
localStorage.getItem("settings")
)
||
{
sort:"alphabetical",
filter:"all"
};



function saveSettings(){

localStorage.setItem(
"settings",
JSON.stringify(settings)
);

}



/* restore */


window.addEventListener(
"load",
()=>{


currentSort=settings.sort;

currentFilter=settings.filter;


const sort=
document.getElementById("sort");


if(sort)

sort.value=currentSort;


});




/*==========================
BETTER FAVORITES VIEW
==========================*/


function showFavorites(){


gallery.innerHTML="";


const favs=
muses.filter(
m=>favorites.includes(m.id)
);



if(favs.length===0){


gallery.innerHTML=`

<div class="empty">

<h2>NO FAVORITES</h2>

<p>
Your archive has no saved files.
</p>

</div>

`;

return;

}



favs.forEach(createCard);



}




/*==========================
DOSSIER TABS
==========================*/


function createTabs(){


const tabs=[

"PROFILE",

"STORY",

"GALLERY",

"TRIVIA"

];


const bar=
document.createElement("div");


bar.className="dossierTabs";



tabs.forEach(tab=>{


const button=
document.createElement("button");


button.textContent=tab;


button.onclick=()=>{


document
.querySelectorAll(".dossierSection")
.forEach(x=>

x.style.display="none"

);



document
.getElementById(
tab.toLowerCase()
)
.style.display="block";


};



bar.appendChild(button);


});


return bar;


}



/*==========================
PAGE TRANSITIONS
==========================*/


function fade(element){


element.classList.add(
"fade"
);


setTimeout(()=>{

element.classList.remove(
"fade"
);

},400);


}



/*==========================
BETTER RANDOM
==========================*/


function randomFile(){


const file=
random(muses);


openDossier(
file.id
);


}



/*==========================
SHORTCUTS
==========================*/


document.addEventListener(
"keydown",
e=>{


// R = random character

if(
e.key.toLowerCase()==="r"
&&
document.activeElement.tagName!=="INPUT"

){

randomFile();

}



// F = favorites

if(
e.key.toLowerCase()==="f"
&&
document.activeElement.tagName!=="INPUT"

){

showFavorites();

}


});



/*==========================
AUTO DAILY LOG ROTATION
==========================*/


setInterval(()=>{


loadDailyQuote();


},60000);




/*==========================
ONLINE STATUS EFFECT
==========================*/


function pulseStatus(){


const status=
document.querySelector(".online");


if(!status)return;


status.style.opacity=.5;


setTimeout(()=>{

status.style.opacity=1;

},500);


}



setInterval(
pulseStatus,
5000
);

const aboutOverlay = document.getElementById("aboutOverlay");
const closeAbout = document.getElementById("closeAbout");

closeAbout.onclick = () => {
    aboutOverlay.classList.add("hidden");
};

aboutOverlay.onclick = (e) => {
    if (e.target === aboutOverlay) {
        aboutOverlay.classList.add("hidden");
    }
};

document.getElementById("closeAbout").onclick = () => {
    document
        .getElementById("aboutOverlay")
        .classList.add("hidden");
};

window.addEventListener("load", () => {

    document.getElementById("closeAbout").onclick = () => {
        document.getElementById("aboutOverlay").classList.add("hidden");
    };

});

window.addEventListener("DOMContentLoaded", () => {

    const aboutOverlay = document.getElementById("aboutOverlay");
    const closeAbout = document.getElementById("closeAbout");

    if (closeAbout) {
        closeAbout.addEventListener("click", () => {
            aboutOverlay.classList.add("hidden");
        });
    }

    if (aboutOverlay) {
        aboutOverlay.addEventListener("click", (e) => {
            if (e.target === aboutOverlay) {
                aboutOverlay.classList.add("hidden");
            }
        });
    }

});

const randomOverlay = document.getElementById("randomOverlay");

document.getElementById("closeRandom").onclick = () => {
    randomOverlay.classList.add("hidden");
};

randomOverlay.onclick = (e) => {
    if (e.target === randomOverlay) {
        randomOverlay.classList.add("hidden");
    }
};

document.getElementById("rerollRandom").onclick = () => {
    showRandomCharacter();
};

document.getElementById("openRandom").onclick = () => {

    randomOverlay.classList.add("hidden");

    openDossier(randomCharacter.id);

};
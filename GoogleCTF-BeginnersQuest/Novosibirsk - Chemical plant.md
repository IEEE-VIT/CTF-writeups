# Question-Name

![date](https://img.shields.io/badge/date-31.10.2021-brightgreen.svg)
<!-- Date you solved the question -->

![category](https://img.shields.io/badge/category-Web-lightgrey.svg)
<!-- Category of the question -->

![score](https://img.shields.io/badge/score--/500-blue.svg)
<!-- Score of the question / Max Score possible in any question -->

## Description


"You must wonder why we have summoned you, AGENT? It has come to our attention that something terrible is about to take place. There is still time to prevent the disaster, and we could not think of anyone more suited for this task than you. We believe that if you can’t solve this quest, neither can anybody else. You have to travel to Novosibirsk, and investigate a suspicious chemical plant. This mission must be executed in secrecy. It’s classified, and it regards the safety of the whole world, therefore we can’t tell you anything more just yet. Go now, you have the fate of the world in your hands."

Challenge: CCTV (rev)
You arrive at your destination. The weather isn't great, so you figure there's no reason to stay outside and you make your way to one of the buildings. No one bothered you so far, so you decide to play it bold - you make yourself a cup of coffee in the social area like you totally belong here and proceed to find an empty room with a desk and a chair. You pull out our laptop, hook it up to the ethernet socket in the wall, and quickly find an internal CCTV panel - that's a way better way to look around unnoticed. Only problem is... it wants a password.

Link: https://cctv-web.2021.ctfcompetition.com/

## Summary

Password was found encrypted(?) in the frontend :)

## Detailed solution

- Viewing the Page Source showed a `checkPassword()` function in js. that looked like this: 
```js
const checkPassword = () => {
  const v = document.getElementById("password").value;
  const p = Array.from(v).map(a => 0xCafe + a.charCodeAt(0));

  if(p[0] === 52037 &&
     p[6] === 52081 &&
     p[5] === 52063 &&
     p[1] === 52077 &&
     p[9] === 52077 &&
     p[10] === 52080 &&
     p[4] === 52046 &&
     p[3] === 52066 &&
     p[8] === 52085 &&
     p[7] === 52081 &&
     p[2] === 52077 &&
     p[11] === 52066) {
    window.location.replace(v + ".html");
  } else {
    alert("Wrong password!");
  }
}
```

- So basically they added `0xCafe` to each of the character and did and checked it char by char.
- I made an array `p` of these characters in that order in the console, quickly wrote a line:
```js
for(int i=0;i<p.length;i++) console.log(String.charCodeFrom(p[i]-0xCafe))
```
- which gave the output `GoodPassword`, which showed the CCTV Screen and the flag.

## Flag

```
CTF{IJustHopeThisIsNotOnShodan}
```
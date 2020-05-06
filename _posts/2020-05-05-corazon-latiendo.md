---
layout: post
title: Corazon Latiendo
date: 2020-05-05 21:05 -0500
---

<style>
    .container {
        display: grid;
        justify-items: center;
        align-items: center;
        height: 400px;
    }
    .container * {
        box-sizing: border-box;
    }
    .heart {
        height: 200px;
        width: 200px;
        background-color: red;
        rotate: 45deg;
        border-bottom-right-radius: 1em;
        box-shadow: 20px 20px 80px;
        animation: latir 1.2s linear infinite;
    }
    .heart::after {
        border-radius: 50% 50% 0 0;
        background-color: red;
        top: -100px;
    }
    .heart::before {
        border-radius: 50% 0 0 50%;
        background-color: red;
        left: -100px;
    }
    .heart::after, .heart::before {
        content: '';
        position: absolute;
        width: 200px;
        height: 200px;
    }
    @keyframes latir {
        0% {
            scale: 100%;
        }
        50% {
            scale: 125%;
        }
        
    }
</style>

<div class="container">
    <div class="heart"></div>
</div>
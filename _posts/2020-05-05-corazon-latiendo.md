---
layout: post
title: Corazon Latiendo
date: 2020-05-05 21:05 -0500
---

<style>
    :root {
        --heart-color: rgb(225, 3, 3);
    }
    .container {
        display: grid;
        justify-items: center;
        align-items: center;
        height: 200px;
    }
    .container * {
        box-sizing: border-box;
    }
    .heart {
        height: 100px;
        width: 100px;
        background-color: var(--heart-color);
        transform: rotate(45deg);
        border-bottom-right-radius: 1em;
        box-shadow: 10px 10px 60px;
        animation: latir 1.2s linear infinite;
    }
    .heart::after {
        border-radius: 50% 50% 0 0;
        top: -50px;
    }
    .heart::before {
        border-radius: 50% 0 0 50%;
        left: -50px;
    }
    .heart::after, .heart::before {
        content: '';
        position: absolute;
        width: 100px;
        height: 100px;
        background: var(--heart-color);
    }
    @keyframes latir {
        0% {
            transform: rotate(45deg) scale(1);
        }
        25% {
            transform: rotate(45deg) scale(1.2);
        }
        50% {
            transform: rotate(45deg) scale(.8);
        }
        75% {
            transform: rotate(45deg) scale(1.2);
        }
        100% {
            transform: rotate(45deg) scale(1);
        }
    }
</style>

<div class="container">
    <div class="heart"></div>
</div>

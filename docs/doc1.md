---
id: doc1
title: Style Guide
sidebar_label: Style Guide
slug: /
---

# 1. 시작해요 히히히
## fragment shader 가 뭐지요?
In the previous chapter we described shaders as the equivalent of the Gutenberg press for graphics. Why? And more importantly: what's a shader?
![From Letter-by-Letter, Right: William Blades (1891). To Page-by-page, Left: Rolt-Wheeler (1920).](https://i.imgur.com/GJ9NvZi.png)

If you already have experience making drawings with computers, you know that in that process you draw a circle, then a rectangle, a line, some triangles until you compose the image you want. That process is very similar to writing a letter or a book by hand 

- 그렇습니다. 한번 그리고 다음 것을 그리고.또 다음 것들 그리고... 연속된 초식의 집합체(또는 세트)라고 할 수 있겠네요.

Shaders도 마찬가지로 명령의 세트입니다. 그러나 셰이더는 조금 다릅니다. 한번에 한번씩 차례대로 답답하게 일처리를 안하지요. 화면의 모든 픽셀들이 동시에 실행됩니다. are also a set of instructions, but the instructions are executed all at once for every single pixel on the screen. That means the code you write has to behave differently depending on the position of the pixel on the screen. Like a type press, your program will work as a function that receives a position and returns a color, and when it's compiled it will run extraordinarily fast.

![](https://i.imgur.com/5MpLwnF.png)

Why are shaders fast?
To answer this, I present the wonders of parallel processing.

Imagine the CPU of your computer as a big industrial pipe, and every task as something that passes through it - like a factory line. Some tasks are bigger than others, which means they require more time and energy to deal with. We say they require more processing power. Because of the architecture of computers the jobs are forced to run in a series; each job has to be finished one at a time. Modern computers usually have groups of four processors that work like these pipes, completing tasks one after another to keep things running smoothly. Each pipe is also known as a thread.

![](https://i.imgur.com/cvwWaOV.png)